# agentgateway-updates - 2026-11-04

No new release shipped in the window (latest stable is still agentgateway v1.7.0, published 2026-10-27); `main` added CEL `upstreamCalls`, the `ultrafast` pricing tier, nested virtual models, Azure OpenAI + Copilot Decisions/SystemOne, and a large direct-response logging overhaul — 83 PRs merged across 3 repos (55 agentgateway, 27 website, 1 benchmarks).

Window: merged/released since **2026-10-28** (no prior `latest.md` found in `sebbycorp/agent-summary`, so the default last-7-days window was used).

## Releases

**None published inside the window.** Current released state of the main repos (for reference):

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway/agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (2026-10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (2026-10-26) | v1.7.0 landed one day *before* this window: model catalog exposes context window/modalities/capabilities via `/v1/models` + CEL, service-tier cost accounting, OpenAI Decisions + TypeSafe SystemOne, Perplexity, Copilot on K8s, virtual-model callout router, MCP CEL guardrails, agent session tracking, `agctl standalone`, raw-YAML UI editing. Breaking: standalone always encrypts MCP sessions, prompt guards inspect tool outputs by default, CEL repeated-header lists, dynamic backends blocked from loopback/link-local, error responses no longer leak internal details. [Full notes](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) |
| [agentgateway/website](https://github.com/agentgateway/website/releases) | [v1.7.0 – "Docs for v1.7.0 release"](https://github.com/agentgateway/website/releases/tag/v1.7.0) (2026-10-27) | – | Docs cut for the 1.7 release; no newer tag |
| [agentgateway/benchmarks](https://github.com/agentgateway/benchmarks) | No releases | – | CI/harness work only (see below) |
| [agentgateway/community](https://github.com/agentgateway/community) | No releases | – | No activity in window (last touched 2026-08-25) |

## Highlights

- **CEL `upstreamCalls` — see what the gateway actually sent upstream** ([#4044](https://github.com/agentgateway/agentgateway/pull/4044), merged 10-29). Access logs can now record every outbound call (MCP calls and policy callouts such as token exchange) with kind, subtype, target, request headers, status, duration and error. Demo angle: log the *minted* token audience per MCP target (`unvalidatedJwtPayload(c.request.headers.authorization...)`) — proof of credential brokering, which was previously invisible in logs. Only recorded when an access-log expression references it.
- **Direct-response overhaul — logs finally say *who* answered** ([#4250](https://github.com/agentgateway/agentgateway/pull/4250), merged 11-02). Everything that used to log as `reason=DirectResponse` is now attributed (`ExtAuth`, `ExtProc`, `Authorization`, `InvalidRequest`, `ModelRouting`, `NotFound`…), real failures keep an `error`, and three client-visible bugs are fixed: MCP-backend ext_authz denies now return the real 403/body instead of a 500 JSON-RPC error, inference-routing (EPP) `ImmediateResponse` in the response phase is honored, and buffer-limit/CSRF rejections carry useful body text. Expect dashboards/alerts keyed on `DirectResponse` to break.
- **New pricing dimension: `ultrafast` service tier** ([#4259](https://github.com/agentgateway/agentgateway/pull/4259), merged 11-02). OpenAI/Bedrock GPT models can be costed at their ultrafast rates; catalogs with `"serviceTier": "ultrafast"` previously failed to parse and were billed at standard rates. Exposed in CEL as `llm.serviceTier`; the auto catalog refreshes ([#4278](https://github.com/agentgateway/agentgateway/pull/4278), [#4243](https://github.com/agentgateway/agentgateway/pull/4243), [#4224](https://github.com/agentgateway/agentgateway/pull/4224), [#4197](https://github.com/agentgateway/agentgateway/pull/4197)) now carry those tiers.
- **Nested virtual models** ([#4216](https://github.com/agentgateway/agentgateway/pull/4216), merged 11-02) — virtual models can point at other virtual models, enabling layered routing trees (tier → team → provider) instead of a single hop.
- **Decisions API spreads across providers**: Azure OpenAI Decisions ([#4277](https://github.com/agentgateway/agentgateway/pull/4277), 11-03) and GitHub Copilot `/v1/decisions` + `/v1/systemone` via `microsoft-decision-1` ([#4222](https://github.com/agentgateway/agentgateway/pull/4222), 10-29), plus Copilot `/v1/embeddings` support ([#4240](https://github.com/agentgateway/agentgateway/pull/4240), 11-02). Good new demo verbs beyond chat.
- **HTTP request normalization lands** ([#4253](https://github.com/agentgateway/agentgateway/pull/4253), 11-02) and host-header casing is normalized ([#4233](https://github.com/agentgateway/agentgateway/pull/4233), 10-29) — path-normalization-based route-confusion defenses, worth calling out in any security review.
- **Security hardening**: backend TLS policies now **fail closed** ([#4198](https://github.com/agentgateway/agentgateway/pull/4198), 10-28) — a broken PEM/SAN/kex group no longer leaves the backend with no TLS policy and sending plaintext; it becomes an Invalid TLS source and 503s (note the trade-off: a bad cert update can now cause an outage). Also `go-jose/v4` 4.1.5 → 4.1.6 for GHSA-mgcc-cww5-fp2q ([#4192](https://github.com/agentgateway/agentgateway/pull/4192)) and stricter JWKS key validation for status ([#4291](https://github.com/agentgateway/agentgateway/pull/4291)).
- **Reliability fixes worth quoting in demos**: mid-stream Anthropic/Responses `error` events now reach Chat Completions clients as a proper OpenAI error chunk instead of a silently truncated answer ([#4270](https://github.com/agentgateway/agentgateway/pull/4270)); array-shaped `message.content` from Mistral reasoning models no longer 502s ([#3729](https://github.com/agentgateway/agentgateway/pull/3729)); missing `model` in stream chunks tolerated ([#4265](https://github.com/agentgateway/agentgateway/pull/4265)); a prompt containing `U+0000` no longer rolls back the whole Postgres request-log batch ([#4274](https://github.com/agentgateway/agentgateway/pull/4274)); h2 honors server-advertised limits smaller than expected ([#4273](https://github.com/agentgateway/agentgateway/pull/4273)).
- **Reasoning control correctness for the new model families**: `thinkingLevel` is sent instead of `thinkingBudget` for `gemma-4` and `gemini-4`, with a per-family effort clamp table ([#4151](https://github.com/agentgateway/agentgateway/pull/4151), 10-28) — these models reject budgets outright.
- **New providers/telemetry**: Nebius Token Factory preset ([#4225](https://github.com/agentgateway/agentgateway/pull/4225)) and metrics served over HBONE ([#4118](https://github.com/agentgateway/agentgateway/pull/4118)).
- **API/CRD surface**: `AgentgatewayModel` graduates — `AGW_ENABLE_AGENTGATEWAY_MODELS` is now always on and the flag is dropped as a "stable API" ([#4280](https://github.com/agentgateway/agentgateway/pull/4280), [#4281](https://github.com/agentgateway/agentgateway/pull/4281)); OAuth param validation moves into CEL ([#4286](https://github.com/agentgateway/agentgateway/pull/4286)); Gateway API bumped to v1.7.0 ([#4232](https://github.com/agentgateway/agentgateway/pull/4232)).
- **Contributor velocity signal**: cold `cargo test -p agentgateway --lib` cut from 124s → 81s ([#4207](https://github.com/agentgateway/agentgateway/pull/4207)) and a local UI visual-regression harness (24 pages × 2 themes, GIF/MP4 export) landed ([#3584](https://github.com/agentgateway/agentgateway/pull/3584)).

## Merged PRs by repo

### agentgateway/agentgateway (55 merged)

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [#4272](https://github.com/agentgateway/agentgateway/pull/4272) | llm/discovery: advertise preferred invocation protocols | [howardjohn](https://github.com/howardjohn) | 2026-11-04 |
| agentgateway | [#4291](https://github.com/agentgateway/agentgateway/pull/4291) | jwks: validate keys a bit more for status | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4273](https://github.com/agentgateway/agentgateway/pull/4273) | h2: handle server advertise smaller than expected limits | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4277](https://github.com/agentgateway/agentgateway/pull/4277) | feat: add Azure OpenAI Decisions API | [uc4w6c](https://github.com/uc4w6c) | 2026-11-03 |
| agentgateway | [#4274](https://github.com/agentgateway/agentgateway/pull/4274) | log_store: replace NUL characters when writing to Postgres | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4285](https://github.com/agentgateway/agentgateway/pull/4285) | ext proc: when downstream cancels, cancel callout requests | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4286](https://github.com/agentgateway/agentgateway/pull/4286) | crd: move oauth param validation into CEL | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4290](https://github.com/agentgateway/agentgateway/pull/4290) | gateway: better error messages for not allowed parent kind | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4288](https://github.com/agentgateway/agentgateway/pull/4288) | controller: allow plugins to give their own GetStatus | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4281](https://github.com/agentgateway/agentgateway/pull/4281) | controller: Drop more dead setting | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4280](https://github.com/agentgateway/agentgateway/pull/4280) | controller: AGW_ENABLE_AGENTGATEWAY_MODELS is now always enabled | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4278](https://github.com/agentgateway/agentgateway/pull/4278) | Update model catalog | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-11-03 |
| agentgateway | [#4282](https://github.com/agentgateway/agentgateway/pull/4282) | llm tests: fix merge conflicts | [howardjohn](https://github.com/howardjohn) | 2026-11-03 |
| agentgateway | [#4270](https://github.com/agentgateway/agentgateway/pull/4270) | fix(llm): send mid-stream errors to Chat Completions clients | [liorbenita2](https://github.com/liorbenita2) | 2026-11-03 |
| agentgateway | [#3729](https://github.com/agentgateway/agentgateway/pull/3729) | fix(llm): accept array content parts in chat completion responses | [julien-aubert](https://github.com/julien-aubert) | 2026-11-03 |
| agentgateway | [#4240](https://github.com/agentgateway/agentgateway/pull/4240) | fix(copilot): normalize string embeddings input and reject chat routes | [iandvt](https://github.com/iandvt) | 2026-11-02 |
| agentgateway | [#4259](https://github.com/agentgateway/agentgateway/pull/4259) | feat(llm): support the ultrafast pricing tier for GPT models | [dongjiang1989](https://github.com/dongjiang1989) | 2026-11-02 |
| agentgateway | [#4260](https://github.com/agentgateway/agentgateway/pull/4260) | cel-fork: add missing README.md symlink target | [dongjiang1989](https://github.com/dongjiang1989) | 2026-11-02 |
| agentgateway | [#4216](https://github.com/agentgateway/agentgateway/pull/4216) | llm: support nested virtual models | [howardjohn](https://github.com/howardjohn) | 2026-11-02 |
| agentgateway | [#4253](https://github.com/agentgateway/agentgateway/pull/4253) | feat(http): add normalization | [makavity](https://github.com/makavity) | 2026-11-02 |
| agentgateway | [#4235](https://github.com/agentgateway/agentgateway/pull/4235) | catalog: manually add jev | [howardjohn](https://github.com/howardjohn) | 2026-11-02 |
| agentgateway | [#4265](https://github.com/agentgateway/agentgateway/pull/4265) | fix(llm): tolerate missing model in completion stream chunks | [howardjohn](https://github.com/howardjohn) | 2026-11-02 |
| agentgateway | [#4255](https://github.com/agentgateway/agentgateway/pull/4255) | fix(ui): handle malformed model URL fragments | [pqminh27](https://github.com/pqminh27) | 2026-11-02 |
| agentgateway | [#4252](https://github.com/agentgateway/agentgateway/pull/4252) | [UI] Read-only banner sits above page title with no spacing | [pqminh27](https://github.com/pqminh27) | 2026-11-02 |
| agentgateway | [#4250](https://github.com/agentgateway/agentgateway/pull/4250) | proxy: better direct response handling | [howardjohn](https://github.com/howardjohn) | 2026-11-02 |
| agentgateway | [#4263](https://github.com/agentgateway/agentgateway/pull/4263) | build(deps): bump the cargo-weekly group with 7 updates | [dependabot[bot]](https://github.com/apps/dependabot) | 2026-11-02 |
| agentgateway | [#4232](https://github.com/agentgateway/agentgateway/pull/4232) | gateway api: move to v1.7.0 | [howardjohn](https://github.com/howardjohn) | 2026-10-30 |
| agentgateway | [#4234](https://github.com/agentgateway/agentgateway/pull/4234) | Bump istio dep to latest | [howardjohn](https://github.com/howardjohn) | 2026-10-30 |
| agentgateway | [#4199](https://github.com/agentgateway/agentgateway/pull/4199) | fix(llm): convert OpenAI file parts & Anthropic document blocks to Bedrock documents | [vaibhav1805](https://github.com/vaibhav1805) | 2026-10-30 |
| agentgateway | [#4249](https://github.com/agentgateway/agentgateway/pull/4249) | Fix cache markers for gpt 6 | [howardjohn](https://github.com/howardjohn) | 2026-10-30 |
| agentgateway | [#4241](https://github.com/agentgateway/agentgateway/pull/4241) | llm: expose request context to response CEL on streaming responses | [rajatvig](https://github.com/rajatvig) | 2026-10-30 |
| agentgateway | [#4246](https://github.com/agentgateway/agentgateway/pull/4246) | ext_proc: keep CEL executor out of request future | [markuskobler](https://github.com/markuskobler) | 2026-10-30 |
| agentgateway | [#4243](https://github.com/agentgateway/agentgateway/pull/4243) | Update model catalog | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-30 |
| agentgateway | [#4222](https://github.com/agentgateway/agentgateway/pull/4222) | feat(copilot): support Decisions and SystemOne | [iandvt](https://github.com/iandvt) | 2026-10-29 |
| agentgateway | [#4215](https://github.com/agentgateway/agentgateway/pull/4215) | llm: add original model as first class concept | [howardjohn](https://github.com/howardjohn) | 2026-10-29 |
| agentgateway | [#4237](https://github.com/agentgateway/agentgateway/pull/4237) | move future size limits into consts | [markuskobler](https://github.com/markuskobler) | 2026-10-29 |
| agentgateway | [#4233](https://github.com/agentgateway/agentgateway/pull/4233) | http: normalize host header casing | [howardjohn](https://github.com/howardjohn) | 2026-10-29 |
| agentgateway | [#4118](https://github.com/agentgateway/agentgateway/pull/4118) | feat(hbone): serve the metrics endpoint over HBONE | [rajatvig](https://github.com/rajatvig) | 2026-10-29 |
| agentgateway | [#4230](https://github.com/agentgateway/agentgateway/pull/4230) | ui: trim model names on save | [cjlg-soloio](https://github.com/cjlg-soloio) | 2026-10-29 |
| agentgateway | [#4225](https://github.com/agentgateway/agentgateway/pull/4225) | llm: add nebius provider | [mgoldsborough](https://github.com/mgoldsborough) | 2026-10-29 |
| agentgateway | [#4224](https://github.com/agentgateway/agentgateway/pull/4224) | Update model catalog | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-29 |
| agentgateway | [#4044](https://github.com/agentgateway/agentgateway/pull/4044) | CEL: Expose outbound calls as upstreamCalls | [ymesika](https://github.com/ymesika) | 2026-10-29 |
| agentgateway | [#4151](https://github.com/agentgateway/agentgateway/pull/4151) | llm: send thinkingLevel instead of thinkingBudget for gemma-4 and gemini-4 | [dongjiang1989](https://github.com/dongjiang1989) | 2026-10-28 |
| agentgateway | [#4211](https://github.com/agentgateway/agentgateway/pull/4211) | fix(llm): forward thinking.display to Bedrock | [matheus-meneses](https://github.com/matheus-meneses) | 2026-10-28 |
| agentgateway | [#4192](https://github.com/agentgateway/agentgateway/pull/4192) | build(deps): bump github.com/go-jose/go-jose/v4 from 4.1.5 to 4.1.6 | [dependabot[bot]](https://github.com/apps/dependabot) | 2026-10-28 |
| agentgateway | [#4200](https://github.com/agentgateway/agentgateway/pull/4200) | feat(mcp): make the ui:// URI target delimiter configurable | [alexoakland](https://github.com/alexoakland) | 2026-10-28 |
| agentgateway | [#4207](https://github.com/agentgateway/agentgateway/pull/4207) | Improve build times, especially for tests | [howardjohn](https://github.com/howardjohn) | 2026-10-28 |
| agentgateway | [#4204](https://github.com/agentgateway/agentgateway/pull/4204) | ui: tell apart models that share a match pattern | [Charlesthebird](https://github.com/Charlesthebird) | 2026-10-28 |
| agentgateway | [#3753](https://github.com/agentgateway/agentgateway/pull/3753) | feat: expose late extension status finalization inputs | [jbohanon](https://github.com/jbohanon) | 2026-10-28 |
| agentgateway | [#4194](https://github.com/agentgateway/agentgateway/pull/4194) | tests: fix various flakes in rust unit tests | [howardjohn](https://github.com/howardjohn) | 2026-10-28 |
| agentgateway | [#3584](https://github.com/agentgateway/agentgateway/pull/3584) | test(ui): add visual regression coverage | [iandvt](https://github.com/iandvt) | 2026-10-28 |
| agentgateway | [#4203](https://github.com/agentgateway/agentgateway/pull/4203) | ui: show full model names in the model combobox | [Charlesthebird](https://github.com/Charlesthebird) | 2026-10-28 |
| agentgateway | [#4195](https://github.com/agentgateway/agentgateway/pull/4195) | controller: avoid panic when mesh config changes | [howardjohn](https://github.com/howardjohn) | 2026-10-28 |
| agentgateway | [#4198](https://github.com/agentgateway/agentgateway/pull/4198) | Fail closed when a backend TLS policy can't be built | [ymesika](https://github.com/ymesika) | 2026-10-28 |
| agentgateway | [#4197](https://github.com/agentgateway/agentgateway/pull/4197) | Update model catalog | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-28 |

### agentgateway/website (27 merged)

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| website | [#1467](https://github.com/agentgateway/website/pull/1467) | [Automated] Refresh product-UI screenshots | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-11-04 |
| website | [#1463](https://github.com/agentgateway/website/pull/1463) | docs: retries follow the frontend maxBufferSize limit | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-11-04 |
| website | [#1464](https://github.com/agentgateway/website/pull/1464) | docs: add credential brokering part 3 | [christian-posta](https://github.com/christian-posta) | 2026-11-02 |
| website | [#1461](https://github.com/agentgateway/website/pull/1461) | Added host to arize and axiom | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-11-02 |
| website | [#1458](https://github.com/agentgateway/website/pull/1458) | Link fixes | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-11-02 |
| website | [#1430](https://github.com/agentgateway/website/pull/1430) | Fix metrics labels | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-11-02 |
| website | [#1432](https://github.com/agentgateway/website/pull/1432) | [Automated] Update reference docs (latest) from agentgateway v1.7.0 | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-11-02 |
| website | [#1431](https://github.com/agentgateway/website/pull/1431) | [Automated] Update reference docs (main) from agentgateway main | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-11-02 |
| website | [#1433](https://github.com/agentgateway/website/pull/1433) | [Automated] Refresh product-UI screenshots | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-11-02 |
| website | [#1449](https://github.com/agentgateway/website/pull/1449) | Fix doc tests that failed in a nightly run | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-11-02 |
| website | [#1421](https://github.com/agentgateway/website/pull/1421) | docs(copilot): document Decisions and SystemOne support | [iandvt](https://github.com/iandvt) | 2026-11-02 |
| website | [#1434](https://github.com/agentgateway/website/pull/1434) | Fix doc tests that failed in a nightly run | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-31 |
| website | [#1429](https://github.com/agentgateway/website/pull/1429) | Doc test fixes for tailscale and phoenix | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-10-30 |
| website | [#1425](https://github.com/agentgateway/website/pull/1425) | [Automated] Update reference docs (main) from agentgateway main | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-30 |
| website | [#1426](https://github.com/agentgateway/website/pull/1426) | [Automated] Update reference docs (latest) from agentgateway v1.7.0 | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-30 |
| website | [#1418](https://github.com/agentgateway/website/pull/1418) | Doc test fixes for Devin, Phoenix, and more | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-10-30 |
| website | [#1423](https://github.com/agentgateway/website/pull/1423) | Use snippets for API group, GatewayClass, and fix Argo Rollouts install | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-10-30 |
| website | [#1427](https://github.com/agentgateway/website/pull/1427) | [Automated] Refresh product-UI screenshots | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-30 |
| website | [#1428](https://github.com/agentgateway/website/pull/1428) | Fix doc tests that failed in a nightly run | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-30 |
| website | [#1422](https://github.com/agentgateway/website/pull/1422) | [Automated] Refresh product-UI screenshots | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-29 |
| website | [#1420](https://github.com/agentgateway/website/pull/1420) | [Automated] Update reference docs (main) from agentgateway main | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-29 |
| website | [#1424](https://github.com/agentgateway/website/pull/1424) | Search heading fix for apostrophes | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-10-29 |
| website | [#1417](https://github.com/agentgateway/website/pull/1417) | Added mac and windows tests | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-10-29 |
| website | [#1419](https://github.com/agentgateway/website/pull/1419) | Removed inline doc tests | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-10-29 |
| website | [#1416](https://github.com/agentgateway/website/pull/1416) | docs: replace announcement with project survey | [danehans](https://github.com/danehans) | 2026-10-28 |
| website | [#1414](https://github.com/agentgateway/website/pull/1414) | Test fixes for traces, jaegar, a2a, mcp observability... | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-10-28 |
| website | [#1413](https://github.com/agentgateway/website/pull/1413) | [Automated] Refresh product-UI screenshots | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-10-28 |

### agentgateway/benchmarks (1 merged)

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| benchmarks | [#8](https://github.com/agentgateway/benchmarks/pull/8) | Fix missing-gcloud harness check on hosted runners | [danehans](https://github.com/danehans) | 2026-10-28 |

The only benchmark commit in the window is the corresponding [Isolate the missing-gcloud harness test (#8)](https://github.com/agentgateway/benchmarks/commit/d53aaaab52cccefd8d220f727bccf3cb53a85b65) (2026-10-28) — CI hygiene, not a scoring change.

### agentgateway/community

None in window.

## Watch list

- **No 1.8 RC is open yet.** Everything below is on `main` post-v1.7.0 and only reachable via nightly/`latest` images — worth pinning on omni-viper if you want to demo it before it releases.
- **Log/dashboard breakage from `reason=DirectResponse` removal** ([#4250](https://github.com/agentgateway/agentgateway/pull/4250)). Re-check any access-log filter, dashboard or alert that matches `DirectResponse`; it is now `ExtAuth` / `ExtProc` / `Authorization` / `InvalidRequest` / `ModelRouting` / `NotFound`. Also: MCP-backend ext_authz denies change from 500 → 403, and inference-routing response-phase `ImmediateResponse` is now honored (was silently ignored). Test on omni-viper with the EPP + ext_authz-on-MCP topologies.
- **TLS fail-closed behavior change** ([#4198](https://github.com/agentgateway/agentgateway/pull/4198)). Bad cert/CA/kex in a backend TLS policy update now 503s instead of keeping the last good config or falling back to plaintext. Genuinely safer, but a bad update can now take a backend down — validate the demo's BackendTLSPolicy YAML before a live run.
- **CRD/API changes to test**: `AgentgatewayModel` is now unconditionally enabled and treated as a stable API ([#4280](https://github.com/agentgateway/agentgateway/pull/4280), [#4281](https://github.com/agentgateway/agentgateway/pull/4281)) — drop the env flag from any install values you still carry; OAuth param validation moved into CEL ([#4286](https://github.com/agentgateway/agentgateway/pull/4286)); Gateway API dependency at v1.7.0 ([#4232](https://github.com/agentgateway/agentgateway/pull/4232)) — confirm your Gateway/ReferenceGrant CRDs are current.
- **Retries now follow `maxBufferSize`** ([website#1463](https://github.com/agentgateway/website/pull/1463) documenting agentgateway #4140): the old hardcoded 64 KiB retry body limit is gone (default 2 MiB, 32 MiB for LLM). Anything you previously said about "large requests aren't retried" is stale — re-shoot that demo.
- **CEL `upstreamCalls`** ([#4044](https://github.com/agentgateway/agentgateway/pull/4044)) — new access-log surface, only materialized when referenced; fan-out produces one entry per outbound call (targets can repeat, stdio targets excluded). Good candidate for a credential-brokering access-log recipe.
- **`ultrafast` service tier** ([#4259](https://github.com/agentgateway/agentgateway/pull/4259)) — `llm.serviceTier` now includes it for OpenAI/Bedrock; verify cost dashboards against the refreshed catalog tiers.
- **Deprecations / catalog churn**: `gemini-2.5-flash-image` and `stepfun-ai/Step-5-Preview` marked `deprecated`, `gemini-3-pro-image-preview` and `gemini-3.1-flash-image-preview` removed from the catalog ([#4224](https://github.com/agentgateway/agentgateway/pull/4224)) — don't hardcode those model IDs in demo configs.
- **Docs process change**: doc-tests moved out of the markdown into a separate doc-tests repo ([website#1419](https://github.com/agentgateway/website/pull/1419)); reference docs + screenshots are regenerated automatically per release ([#1432](https://github.com/agentgateway/website/pull/1432), [#1467](https://github.com/agentgateway/website/pull/1467)). Several automated PRs carry a "capture or sync failed for `latest main`" warning, so don't treat them as a complete refresh.
