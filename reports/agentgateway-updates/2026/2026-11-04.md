# agentgateway-updates - 2026-11-04

Quiet day: **no new release and no new merged PRs since the 09:46 ET run earlier today** — agentgateway is still v1.7.0 (2026-10-27) with no 1.8 RC open; the only activity since then is in-flight open work (extensible model sources #4292, budget-keyed-by-id fix #4284, GCP backendAuth tokens #4279).

## Window

This is the same-day delta for **2026-11-04** (America/Toronto). An earlier `agentgateway-updates` run today covered **2026-10-28 → 2026-11-04** (83 PRs across 3 repos); that content is superseded by this file, and the still-live items are recapped below so nothing actionable is lost.

Verified this session:
- Repos pushed since 2026-11-01: only [agentgateway/agentgateway](https://github.com/agentgateway/agentgateway) and [agentgateway/website](https://github.com/agentgateway/website) (`benchmarks` and `community` had no pushes in the window).
- Merged PRs with `merged:>=2026-11-04`: **3 total**, all merged *before* the 09:46 ET run and all already reported then.
- Commits on `agentgateway/agentgateway` main since 2026-11-04T00:00Z: **1**.
- Commits on `agentgateway/website` main since 2026-11-04T12:10Z: **0**.

## Releases

**None published in the window.** Current released state (checked directly):

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway/agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (2026-10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (2026-10-26) | No new tag since; next release cut has not started (no v1.8.0-alpha.1). Model catalog + service-tier cost accounting + Decisions/SystemOne + `agctl standalone` + raw-YAML UI are the headline 1.7.0 features ([release notes](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0)). |
| [agentgateway/website](https://github.com/agentgateway/website/releases) | [v1.7.0 – "Docs for v1.7.0 release"](https://github.com/agentgateway/website/releases/tag/v1.7.0) (2026-10-27) | – | No newer docs cut. |
| [agentgateway/benchmarks](https://github.com/agentgateway/benchmarks) | No releases | – | No pushes in window. |
| [agentgateway/community](https://github.com/agentgateway/community) | No releases | – | No pushes in window. |

Because no RC exists yet, everything below v1.7.0 is only reachable via nightly/`latest` images.

## Highlights

**New since the last report: none.** No features, breaking changes or security fixes landed in the gap.

Carried forward from the 2026-10-28 → 2026-11-04 run — still unreleased, still demo-relevant (all on `main` post-v1.7.0):

- **CEL `upstreamCalls`** ([#4044](https://github.com/agentgateway/agentgateway/pull/4044)) — access logs can now record every outbound call (MCP calls, token-exchange callouts) with target, headers, status and duration. Best credential-brokering proof point we have: log the *minted* token audience per MCP target.
- **Direct-response attribution overhaul** ([#4250](https://github.com/agentgateway/agentgateway/pull/4250)) — `reason=DirectResponse` is now split into `ExtAuth` / `ExtProc` / `Authorization` / `InvalidRequest` / `ModelRouting` / `NotFound`; MCP-backend ext_authz denies return a real 403 instead of a 500 JSON-RPC error. Breaks any dashboard or alert keyed on `DirectResponse`.
- **`ultrafast` service tier** ([#4259](https://github.com/agentgateway/agentgateway/pull/4259)) — OpenAI/Bedrock GPT models can now be costed at ultrafast rates; exposed in CEL as `llm.serviceTier`.
- **Nested virtual models** ([#4216](https://github.com/agentgateway/agentgateway/pull/4216)) — virtual models can point at other virtual models, so tier → team → provider routing trees are one config, not a hop chain.
- **Decisions API spreading**: Azure OpenAI Decisions ([#4277](https://github.com/agentgateway/agentgateway/pull/4277)) and Copilot `/v1/decisions` + `/v1/systemone` ([#4222](https://github.com/agentgateway/agentgateway/pull/4222)).
- **Security**: backend TLS policies now **fail closed** — a broken PEM/SAN/kex group 503s instead of silently sending plaintext ([#4198](https://github.com/agentgateway/agentgateway/pull/4198)); HTTP request normalization ([#4253](https://github.com/agentgateway/agentgateway/pull/4253)) and host-header casing ([#4233](https://github.com/agentgateway/agentgateway/pull/4233)) close route-confusion paths; stricter JWKS key validation ([#4291](https://github.com/agentgateway/agentgateway/pull/4291)).
- **API surface**: `AgentgatewayModel` is unconditionally enabled and treated as stable — the `AGW_ENABLE_AGENTGATEWAY_MODELS` flag is dropped ([#4280](https://github.com/agentgateway/agentgateway/pull/4280), [#4281](https://github.com/agentgateway/agentgateway/pull/4281)).

## Merged PRs by repo

### agentgateway/agentgateway

None in window (nothing merged after the 09:46 ET run). The only merged PR dated 2026-11-04, already in the morning report:

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [#4272](https://github.com/agentgateway/agentgateway/pull/4272) | llm/discovery: advertise preferred invocation protocols | [howardjohn](https://github.com/howardjohn) | 2026-11-04 (00:01 UTC) |

Its merge commit is the single commit on `main` today: [`134897e`](https://github.com/agentgateway/agentgateway/commit/134897e1975fa26fa5c5b8ba45d720d05d3f7fb8) (co-authored with Shashank Ram).

### agentgateway/website

None new. Both 2026-11-04 merges were already reported this morning:

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| website | [#1467](https://github.com/agentgateway/website/pull/1467) | [Automated] Refresh product-UI screenshots | [github-actions[bot]](https://github.com/apps/github-actions) | 2026-11-04 (12:02 UTC) |
| website | [#1463](https://github.com/agentgateway/website/pull/1463) | docs: retries follow the frontend maxBufferSize limit | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 2026-11-04 (11:45 UTC) |

Note the screenshot refresh PR carries a *"capture or sync failed for `latest main`"* warning, so it is not a complete refresh.

### agentgateway/benchmarks

None in window.

### agentgateway/community

None in window.

## Watch list

Open, actively-moving work — worth pre-testing on omni-viper before it lands:

- **Extensible model source / plugin points** — [agentgateway#4292](https://github.com/agentgateway/agentgateway/pull/4292), opened today by [howardjohn](https://github.com/howardjohn), 46 files (+1014/−248). Adds controller plugin points for a **custom model source** plus a "preferred endpoint" type in the model catalog, on both standalone and Kubernetes. This is the API-surface item to watch: if it merges, internal model resolution becomes pluggable, which changes how we'd wire an in-house model catalog into a demo. Currently `mergeable_state: blocked`.
- **Budgets keyed by `agentgateway.dev/id` instead of key hash** — [agentgateway#4284](https://github.com/agentgateway/agentgateway/pull/4284) fixes [#4283](https://github.com/agentgateway/agentgateway/issues/4283). Notable for standalone cost-control demos: API-key budgets would follow the key's stable id across rehashes, with a temporary merge-and-delete migration at flush time. Two documented caveats: hand-written YAML keys without `agentgateway.dev/id` stay hash-linked, and during a rolling upgrade old replicas can hand out up to one full extra budget per key. Also: **budgeted keys sharing an id become invalid**. Watch for that validation landing.
- **GCP tokens in `backendAuth` credentials** — [agentgateway#4279](https://github.com/agentgateway/agentgateway/pull/4279) ([onematchfox](https://github.com/onematchfox)) — would add a new backend-auth credential option; relevant to Vertex demos.
- **gRPC local failures reported as `Unavailable`** — [agentgateway#4226](https://github.com/agentgateway/agentgateway/pull/4226) ([fiunchinho](https://github.com/fiunchinho)) — pairs with the merged v1.7.0 change on gRPC policy-call statuses; affects how retries/health treat local errors.
- **Empty translated Chat Completions should fail** — [agentgateway#4128](https://github.com/agentgateway/agentgateway/pull/4128) ([iandvt](https://github.com/iandvt)) — closes a silent-empty-answer path in format conversion; useful to retest after it lands.
- **Windows home-directory resolution** — [agentgateway#4264](https://github.com/agentgateway/agentgateway/pull/4264) ([xixi-box](https://github.com/xixi-box)) — only matters for `agctl`/standalone on Windows laptops.
- **Docs in flight**: Honeycomb standalone + Kubernetes observability guides ([website#1466](https://github.com/agentgateway/website/pull/1466)) and the automated reference-docs refresh for `main` ([website#1465](https://github.com/agentgateway/website/pull/1465)) are both still open; a fresh model catalog refresh is queued ([agentgateway#4293](https://github.com/agentgateway/agentgateway/pull/4293)) — don't hardcode model IDs that a catalog bump may drop.

Carried forward from the previous window (still un-triaged on our side):

- **No 1.8 RC exists yet** — everything post-v1.7.0 (CEL `upstreamCalls`, direct-response split, `ultrafast` tier, nested virtual models, TLS fail-closed, `AgentgatewayModel` stabilization) needs nightly/`latest` images. Pin a nightly on omni-viper if you want to demo it early.
- **Breakage to pre-empt**: access-log filters/alerts keyed on `reason=DirectResponse` ([#4250](https://github.com/agentgateway/agentgateway/pull/4250)); MCP-backend ext_authz deny 500 → 403; backend TLS policy updates now 503 on bad cert material ([#4198](https://github.com/agentgateway/agentgateway/pull/4198)) — validate the demo's BackendTLSPolicy YAML before going live; drop the `AGW_ENABLE_AGENTGATEWAY_MODELS` env flag from any values file you still carry ([#4280](https://github.com/agentgateway/agentgateway/pull/4280)).
- **Stale demo script**: retries now follow `spec.frontend.http.maxBufferSize` (default 2 MiB, 32 MiB for LLM) rather than the old hardcoded 64 KiB — documented in [website#1463](https://github.com/agentgateway/website/pull/1463). Anything we said about "large requests aren't retried" is wrong now; re-shoot that segment.
