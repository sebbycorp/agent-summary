# agentgateway-updates - 2026-11-17

**The freeze broke: 11 PRs merged into `agentgateway` main on Sun 11-16 (malformed-Host 400, HBONE deadlock fixes, MCP multi-replica sessions, Vertex beta-header filter), and the PR-tracker docs backlog landed in `website` overnight (36 merges, incl. the 1.8 release notes). Still no 1.8 tag.**

_Window: merged/pushed `>=2026-11-16` (last report covered `>=2026-11-15`). `benchmarks` and `community` idle._

## Releases

**None published in window.** Newest artifact is still the Oct 27 cut; nothing past `v1.7.0`, so everything below is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0][ag-170] (10-27) | [v1.7.0-rc.1][ag-170rc1] (10-26) |
| website | [v1.7.0 "Docs for v1.7.0"][ws-170] (10-27) | – |
| benchmarks · community | None | – |

## Highlights

- **Malformed `Host` gets a `400`, not a route miss** — [agw#4439][pr-4439] (howardjohn, 11-16, fixes #4361). Rejected client-side before routing; same family as the duplicate-Host fix in the 1.8 notes. Pair with path normalization in the security slide.
- **MCP streams work behind N replicas** — [#4437][pr-4437] (howardjohn, 11-16, fixes #4428). `GET /mcp` checked only the local session map, so a `GET` landing on a replica that never handled `initialize` returned **404**; now uses `get_or_resume_session` like POST/DELETE. This was the "works on my laptop, breaks on Kind" MCP demo failure.
- **HBONE deadlock fixes ported from ztunnel** — [#4436][pr-4436] (howardjohn, 11-16, cherry-picks ztunnel `043ea989`). With the h2 waiter/deadlock entries in the 1.8 notes, HBONE + h2 stability is the theme of this batch.
- **Vertex stops 400-ing on unknown `anthropic-beta` values** — [#4416][pr-4416] (jamesilse, 11-16, fixes #4396). Unsupported values filtered; `AGENTGATEWAY_VERTEX_ANTHROPIC_BETA_HEADERS` overrides the allowlist. The Bedrock twin [#4420][pr-4420] is still open.
- **Anthropic wire conformance on Bedrock** — [#4432][pr-4432] (ividalATSnyk, 11-16). Emits both Anthropic cache-duration counters when Bedrock supplies cache-creation details (missing duration defaults to 0), and adds the **required `error` discriminator** in translated Anthropic error envelopes — strict Anthropic SDKs reject the envelope without it.
- **`retry` in the simplified standalone LLM config** — [#4424][pr-4424] (dimetron, 11-16, fixes #4423). `llm.policies` took `timeout` but not `retry`; `retry: {attempts, codes, backoff}` now flows via `split_policies` into `TrafficPolicy::Retry`; schema regenerated. Kills a quickstart footgun.
- **Model catalog refreshed, `flex` tier everywhere** — [#4418][pr-4418] (bot, 11-16). Adds `serviceTier: flex` to gpt-5.4/5.5/5.6-\*/6\*/6.1\* plus new models (`gemini-4-hydrogen`, `Kimi K3.1 Pro`, `Inkling 2 Small`), drops Azure/OpenAI `gpt-image-2*`, cuts Gemini image output rates ~10x. Cost/budget demo numbers change.
- **OIDC bearer scope documented** — [#4357][pr-4357] (ymesika, 11-16). `sessionOrBearer` accepts **any** issuer-signed JWT carrying the client in `aud`; restrict with authorization rules.
- **1.8 release notes finally exist** — website [#1519][pr-1519] merged 11-17 01:48 with ~20 auto-drafted doc PRs (path normalization, `ui.basePath`, `config.dns.maxTtl`, nested virtual models, `AGW_ENABLE_AGENTGATEWAY_MODELS` removal, OTLP log body/timestamp).
- **No CVEs** in window; 4439/4431/4369/4394 are the security-adjacent items.

## Merged PRs by repo

`org:agentgateway is:pr is:merged merged:>=2026-11-16` → **47** (11 `agentgateway`, 36 `website`).

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4439][pr-4439] | fix: return 400 for malformed Host headers | howardjohn | 11-16 |
| agentgateway | [4437][pr-4437] | mcp: resume sessions on GET (any replica serves stream) | howardjohn | 11-16 |
| agentgateway | [4436][pr-4436] | hbone: port deadlock fixes over | howardjohn | 11-16 |
| agentgateway | [4432][pr-4432] | fix: preserve Anthropic response conformance on Bedrock | ividalATSnyk | 11-16 |
| agentgateway | [4424][pr-4424] | feat(llm): support retry in standalone llm.policies | dimetron | 11-16 |
| agentgateway | [4418][pr-4418] | Update model catalog | github-actions[bot] | 11-16 |
| agentgateway | [4416][pr-4416] | fix(llm): filter unsupported anthropic-beta on Vertex | jamesilse | 11-16 |
| agentgateway | [4357][pr-4357] | oidc: clarify bearer tokens sessionOrBearer accepts | ymesika | 11-16 |
| agentgateway | [4444][pr-4444] | document request log api and budget status types | jenshu | 11-16 |
| website | [1519][pr-1519] | Release notes from the PR tracker (the 1.8 notes) | github-actions[bot] | 11-17 |
| website | [1551][pr-1551] | misc doc fixes: MCP tasks, normalization, connection limits | artberger | 11-17 |
| website | [1550][pr-1550] | docs: private multi-LLM routing on GKE blog post | danehans | 11-16 |
| website | [1549][pr-1549] | docs: pin GKE guide to Cluster Toolkit v1.109.0 | danehans | 11-16 |
| website | [1399][pr-1399] | docs: shared provider TLS defaults | pqminh27 | 11-17 |

+22 more (incl. [4340][pr-4340] ui multiselect, [4440][pr-4440] ci, [1553][pr-1553] reference docs, and ~19 `[Automated] Draft docs` regens): [all 47 merged][merged].

## Watch list

| Repo | PR | Why |
| --- | --- | --- |
| agentgateway | [4430][pr-4430] | **(breaking)** default `agentgateway.user` = `jwt.email` → `jwt.sub` → API-key metadata, dropping `apiKey.name`: splits request-log history per person, changes UI API-key ownership. Pin `standardAttributes.user` for old behaviour. Blocked |
| agentgateway | [4431][pr-4431] | Security hardening: deny/allowlist files referenced from config (`/dev/…` risk). Blocked |
| agentgateway | [4421][pr-4421] · [4420][pr-4420] | Yesterday's CRD/API items still open: CEL `replace` on header transforms in xDS + `AgentgatewayPolicy`; Bedrock `advanced-tool-use` beta mapping |
| agentgateway | [4387][pr-4387] · [4369][pr-4369] · [4394][pr-4394] | **CRD shape change**: prompt-guard policies move to top level; `privateKeyJwt` assertion capped at 300s; isolate remote JWKS fetch failures |
| agentgateway | [4447][pr-4447] · [4402][pr-4402] · [4413][pr-4413] · [4419][pr-4419] | Drop catalog tiers with an unrecognized `serviceTier`; Gemini Decisions API routing; rest of the LiteLLM importer series + duplicate-model guard (**no window activity**) |

Re-test on omni-viper with `:latest-dev`: MCP `GET /mcp` over 3 replicas (4437), malformed Host → 400 (4439), Vertex beta filter + env override (4416), Bedrock cache counters + `error` discriminator (4432), standalone `llm.policies.retry` (4424).

[merged]: https://github.com/pulls?q=is%3Apr+org%3Aagentgateway+is%3Amerged+merged%3A%3E%3D2026-11-16
[ag-170]: https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0
[ag-170rc1]: https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1
[ws-170]: https://github.com/agentgateway/website/releases/tag/v1.7.0
[pr-4439]: https://github.com/agentgateway/agentgateway/pull/4439
[pr-4437]: https://github.com/agentgateway/agentgateway/pull/4437
[pr-4436]: https://github.com/agentgateway/agentgateway/pull/4436
[pr-4432]: https://github.com/agentgateway/agentgateway/pull/4432
[pr-4424]: https://github.com/agentgateway/agentgateway/pull/4424
[pr-4418]: https://github.com/agentgateway/agentgateway/pull/4418
[pr-4416]: https://github.com/agentgateway/agentgateway/pull/4416
[pr-4357]: https://github.com/agentgateway/agentgateway/pull/4357
[pr-4340]: https://github.com/agentgateway/agentgateway/pull/4340
[pr-4444]: https://github.com/agentgateway/agentgateway/pull/4444
[pr-4440]: https://github.com/agentgateway/agentgateway/pull/4440
[pr-4430]: https://github.com/agentgateway/agentgateway/pull/4430
[pr-4431]: https://github.com/agentgateway/agentgateway/pull/4431
[pr-4421]: https://github.com/agentgateway/agentgateway/pull/4421
[pr-4420]: https://github.com/agentgateway/agentgateway/pull/4420
[pr-4447]: https://github.com/agentgateway/agentgateway/pull/4447
[pr-4402]: https://github.com/agentgateway/agentgateway/pull/4402
[pr-4369]: https://github.com/agentgateway/agentgateway/pull/4369
[pr-4394]: https://github.com/agentgateway/agentgateway/pull/4394
[pr-4387]: https://github.com/agentgateway/agentgateway/pull/4387
[pr-4413]: https://github.com/agentgateway/agentgateway/pull/4413
[pr-4419]: https://github.com/agentgateway/agentgateway/pull/4419
[pr-1519]: https://github.com/agentgateway/website/pull/1519
[pr-1551]: https://github.com/agentgateway/website/pull/1551
[pr-1550]: https://github.com/agentgateway/website/pull/1550
[pr-1549]: https://github.com/agentgateway/website/pull/1549
[pr-1399]: https://github.com/agentgateway/website/pull/1399
[pr-1553]: https://github.com/agentgateway/website/pull/1553
