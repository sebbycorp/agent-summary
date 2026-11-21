# agentgateway-updates - 2026-11-21

**Pre-1.8 backfill day: the long-tail breaking changes landed at once - AWS native endpoints by default (#4300), JWT-email default user (#4430), Azure Content Safety guards in Kubernetes mode (#4205), initial MCP "Skills" (#4214). 23 merges (21 `agentgateway`, 2 `website`); still no 1.8 tag, but four "1.8" doc drafts open.**

_Window: merged `>=2026-11-20T01:03Z`; yesterday's digest covered #3872, #4431, #4480, #4477, #4463, #4468, #4479. `benchmarks`/`community` idle._

## Releases

**None published** - everything after the Oct 27 cut is nightly / `:latest-dev`.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | - |

## Highlights

- **AWS native LLM endpoints by default** ([4300](https://github.com/agentgateway/agentgateway/pull/4300), **breaking**, fixes #4098). Bedrock moves from translated `converse` to `/chat/completions`, `/messages`, `/responses` (AWS's abstraction over `invoke`). Flag remains, only the default flipped - but it needs a **different IAM role**. Re-check demo policy before `:latest-dev`.
- **Default `agentgateway.user` prefers JWT email** ([4430](https://github.com/agentgateway/agentgateway/pull/4430), **breaking**). `jwt.email` -> `jwt.sub` -> API-key metadata; `apiKey.name` dropped as a key label. Log/analytics history splits across the upgrade; UI-created keys become owned by creator email. Pin `standardAttributes.user` for old behaviour.
- **Azure Content Safety guards in Kubernetes mode** ([4205](https://github.com/agentgateway/agentgateway/pull/4205), fixes #3569, +2867/-446, 20 files). New CRD definitions, fail-closed/fail-open from #3618, proto lets `detectJailbreak` stand alone, user policies ordered ahead of implicit auth. **Biggest CRD/API change of the window** - regenerate manifests.
- **MCP "Skills" extension, initial support** ([4214](https://github.com/agentgateway/agentgateway/pull/4214), #3579). No multiplexing yet - [4490](https://github.com/agentgateway/agentgateway/pull/4490) still open; don't promise it in the roadmap slide.
- **GenAI telemetry: `gen_ai.response.id` + `gen_ai.response.finish_reasons`** ([4483](https://github.com/agentgateway/agentgateway/pull/4483), part of #4482, 169 files). Also CEL `llm.responseId`/`llm.finishReasons`; all chat routes, buffered and streaming; Responses status mapped `completed`->`stop`, `failed`->`error`; string lists export as OTel arrays. Fresh observability-demo material.
- **LB fallback correctness** ([4497](https://github.com/agentgateway/agentgateway/pull/4497)). `select_fallback` now prefers any viable *active* endpoint across all buckets before rejected ones, matching `select_affinity`; previously an all-rejected bucket could beat a later healthy pod - the source of rollout/drain flakiness.
- **Hardening**: stable config-store UIDs ([4464](https://github.com/agentgateway/agentgateway/pull/4464)); `basicAuth` bcrypt cost capped ([4405](https://github.com/agentgateway/agentgateway/pull/4405), closes a CPU-DoS from crafted high-cost hashes). No CVEs against org-owned code in window.
- **Also demo-visible**: Gemini structured output via `responseJsonSchema` ([4261](https://github.com/agentgateway/agentgateway/pull/4261)); `buffer` back under normal policy hierarchy ([4491](https://github.com/agentgateway/agentgateway/pull/4491)); settings drawers stop writing unchanged fields, incl. the `stateless`-vs-`stateful` default bug ([4501](https://github.com/agentgateway/agentgateway/pull/4501)); PowerShell installer for Windows ([4478](https://github.com/agentgateway/agentgateway/pull/4478)).

## Merged PRs by repo

**23 merges** (21 `agentgateway`, 2 `website`); none in `benchmarks`/`community`. All merged 11-20.

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4300](https://github.com/agentgateway/agentgateway/pull/4300) | **(breaking)** llm: native endpoints by default on AWS | howardjohn | 11-20 |
| agentgateway | [4430](https://github.com/agentgateway/agentgateway/pull/4430) | **(breaking)** prefer JWT email for the default user | howardjohn | 11-20 |
| agentgateway | [4205](https://github.com/agentgateway/agentgateway/pull/4205) | controller: azureContentSafety prompt guards | jgreeer | 11-20 |
| agentgateway | [4214](https://github.com/agentgateway/agentgateway/pull/4214) | mcp: initial "Skills" extension support | howardjohn | 11-20 |
| agentgateway | [4483](https://github.com/agentgateway/agentgateway/pull/4483) | telemetry: gen_ai.response.id + finish_reasons | krisztianfekete | 11-20 |
| agentgateway | [4497](https://github.com/agentgateway/agentgateway/pull/4497) | lb: prefer active endpoints in fallback | howardjohn | 11-20 |
| agentgateway | [4464](https://github.com/agentgateway/agentgateway/pull/4464) · [4405](https://github.com/agentgateway/agentgateway/pull/4405) | config store: stable UIDs; basicAuth bcrypt cost cap | howardjohn | 11-20 |
| agentgateway | [4261](https://github.com/agentgateway/agentgateway/pull/4261) | llm: Gemini structured-output schemas | dongjiang1989 | 11-20 |
| agentgateway | [4408](https://github.com/agentgateway/agentgateway/pull/4408) | llm: normalize response model lookup on azure | howardjohn | 11-20 |
| agentgateway | [4501](https://github.com/agentgateway/agentgateway/pull/4501) · [4492](https://github.com/agentgateway/agentgateway/pull/4492) | ui: drawers write only edits; llm policy needs no settings row | howardjohn, stevenctl | 11-20 |
| agentgateway | [4491](https://github.com/agentgateway/agentgateway/pull/4491) | policy: buffer respects policy hierarchy | howardjohn | 11-20 |
| agentgateway | [4487](https://github.com/agentgateway/agentgateway/pull/4487) · [4406](https://github.com/agentgateway/agentgateway/pull/4406) · [4398](https://github.com/agentgateway/agentgateway/pull/4398) | mcp/llm settings from file config; provider-key catalogs; unblock db runtime | howardjohn | 11-20 |
| agentgateway | [4500](https://github.com/agentgateway/agentgateway/pull/4500) · [4499](https://github.com/agentgateway/agentgateway/pull/4499) · [4481](https://github.com/agentgateway/agentgateway/pull/4481) · [4460](https://github.com/agentgateway/agentgateway/pull/4460) · [4478](https://github.com/agentgateway/agentgateway/pull/4478) | model-discovery logs; copilot integrator id; catalog; build fixes; PowerShell installer | howardjohn, bot, sebbycorp | 11-20 |
| website | [1570](https://github.com/agentgateway/website/pull/1570) · [1561](https://github.com/agentgateway/website/pull/1561) | automated reference docs; params.llms.rootNote | bot, artberger | 11-20 |

## Watch list

| Item | Why |
| --- | --- |
| website [1576](https://github.com/agentgateway/website/pull/1576)·[1577](https://github.com/agentgateway/website/pull/1577)·[1578](https://github.com/agentgateway/website/pull/1578)·[1579](https://github.com/agentgateway/website/pull/1579) | **Strongest 1.8 signal yet**: draft "1.8" docs for guardrails, Gateway API **v1.7 move**, LLM providers/catalog, data-plane metrics. v1.7 = CRD churn; refresh omni-viper manifests. |
| [4490](https://github.com/agentgateway/agentgateway/pull/4490) · [4345](https://github.com/agentgateway/agentgateway/pull/4345) · [4493](https://github.com/agentgateway/agentgateway/pull/4493) | MCP Skills multiplexing; guardrails on errors/server messages and request-phase `tasks/*`. |
| [4488](https://github.com/agentgateway/agentgateway/pull/4488) · [4473](https://github.com/agentgateway/agentgateway/pull/4473) · [4505](https://github.com/agentgateway/agentgateway/pull/4505) | Weaver telemetry registry (draft); multiple MCP endpoints in standalone; **Substrate** router/egress alignment (draft). |
| [4469](https://github.com/agentgateway/agentgateway/pull/4469) · [4503](https://github.com/agentgateway/agentgateway/pull/4503) · [4502](https://github.com/agentgateway/agentgateway/pull/4502) | Delegated-provider/health reporting, budgets stale-write fix, in-tree SSE decoder (streaming regression risk). |

Re-test on omni-viper with `:latest-dev`: **#4300** Bedrock IAM role, **#4430** user attribution/log split, **#4205** azureContentSafety CRD guards, **#4497** fallback during rollout, **#4483** OTel `gen_ai.response.id`, **#4501** MCP stateless/stateful default.
