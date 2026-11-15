# agentgateway-updates - 2026-11-15

**Quiet Sunday: 1 PR merged (agentgateway only), no releases — still v1.7.0 from Oct 27, no 1.8 tag. The action is in flight: a 3-PR LiteLLM importer expansion, a duplicate-model-name controller fix, a Vertex `anthropic-beta` filter, and the first release-notes PR for the next cut.**

_Window: merged/updated `>=2026-11-14` (last report covered `>=2026-11-13`). Active: [agentgateway](https://github.com/agentgateway/agentgateway) (1 merge); `website`, `community` and `benchmarks` had no commits._

## Releases

**None in window.** Newest artifact is still the Oct 27 cut — everything since (LLM-IR, Bedrock fixes, webhook-guardrail writeback, catalog refreshes) is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) "Docs for v1.7.0 release" (10-27) | – |
| benchmarks · community | None | – |

## Highlights

- **LiteLLM migration built out in earnest** — three stacked PRs opened 11-14 against #2607: [4413](https://github.com/agentgateway/agentgateway/pull/4413) maps inbound API-key settings, [4414](https://github.com/agentgateway/agentgateway/pull/4414) covers retry/timeout/order/pass-through/guardrails/MCP (109 files, +4,645), [4415](https://github.com/agentgateway/agentgateway/pull/4415) covers DB-managed keys, budgets and MCP grants. **Demo impact:** the "migrate off LiteLLM" story goes from diagnostics-only (yesterday's #3663) to real conversion — re-run an import before quoting parity.
- **Duplicate model names per router about to become a config error** — [4419](https://github.com/agentgateway/agentgateway/pull/4419), opened today. Today two same-named models on one listener coexist and either can answer; the fix keeps one (oldest, then namespace/name) and marks the loser `Accepted=False, Conflicted`. Good caveat slide for canary/shadow setups — not merged yet.
- **Vertex + Anthropic beta flags (open)** — [4416](https://github.com/agentgateway/agentgateway/pull/4416): Vertex rejects unsupported `anthropic-beta` values; keeps only allowed ones, with `AGENTGATEWAY_VERTEX_ANTHROPIC_BETA_HEADERS` override. Fixes #4396. Matters for Claude-via-Vertex with caching/thinking beta headers.
- **Windsurf/Devin Desktop recipe corrected (open)** — [4417](https://github.com/agentgateway/agentgateway/pull/4417): the old recipe set `Http: Proxy` to the LLM gateway URL, so `CONNECT` returned 405 and Devin lost network; new path runs Claude Agent over ACP with per-agent env vars at agentgateway. Windsurf noted as effectively EOL.
- **1.8 release notes being assembled** — website [1519](https://github.com/agentgateway/website/pull/1519) "Release notes from the PR tracker" updated today, plus draft-docs PRs [1542](https://github.com/agentgateway/website/pull/1542)–[1545](https://github.com/agentgateway/website/pull/1545) and reference regens [1526](https://github.com/agentgateway/website/pull/1526)/[1527](https://github.com/agentgateway/website/pull/1527). A cut looks nearer than the Oct cadence suggested.
- **The only merge is CI hygiene** — [4373](https://github.com/agentgateway/agentgateway/pull/4373) makes the `cel-derive` expand test build its own fixtures; no behaviour change.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4373](https://github.com/agentgateway/agentgateway/pull/4373) | fix(cel-derive): make expand test build its fixtures | markuskobler | 11-14 |

Complete in-window set: 1 merge in `agentgateway`, 0 in `website`/`community`/`benchmarks`. Full list: [merged PRs `>=2026-11-14`](https://github.com/pulls?q=is%3Apr+is%3Amerged+org%3Aagentgateway+merged%3A%3E%3D2026-11-14).

## Watch list

| Repo | PR | Why |
| --- | --- | --- |
| agentgateway | [4419](https://github.com/agentgateway/agentgateway/pull/4419) | **API/status change**: duplicate model names → `Accepted=False/Conflicted`; blocked on review today |
| agentgateway | [4413](https://github.com/agentgateway/agentgateway/pull/4413) · [4414](https://github.com/agentgateway/agentgateway/pull/4414) · [4415](https://github.com/agentgateway/agentgateway/pull/4415) | LiteLLM importer expansion (keys, budgets, MCP, guardrails, timeouts) — the migration pitch |
| agentgateway | [4416](https://github.com/agentgateway/agentgateway/pull/4416) | Filter unsupported `anthropic-beta` on Vertex; new env override |
| agentgateway | [4417](https://github.com/agentgateway/agentgateway/pull/4417) | Devin Desktop / Claude Agent ACP recipe (replaces broken Windsurf proxy recipe) |
| agentgateway | [4392](https://github.com/agentgateway/agentgateway/pull/4392) | Open+blocked, updated 11-15: `bedrockRuntimeApi: native \| converse` per provider — Codex losing `custom` tools on Bedrock Runtime; **no xDS field yet** |
| agentgateway | [3350](https://github.com/agentgateway/agentgateway/pull/3350) · [3327](https://github.com/agentgateway/agentgateway/pull/3327) | Limit in-flight requests per CEL key (per-tenant bulkhead); MCP Entra AS metadata RFC 8414 alignment |
| agentgateway | [4407](https://github.com/agentgateway/agentgateway/pull/4407) · [4409](https://github.com/agentgateway/agentgateway/pull/4409) | UI fixes (admin-port warning, playground CORS, provider aliases, file-mode banner) + draft antd migration |
| website | [1519](https://github.com/agentgateway/website/pull/1519) | Release notes from the PR tracker — watch for the 1.8 cut; [4418](https://github.com/agentgateway/agentgateway/pull/4418) is today's pending catalog refresh |

Nothing to re-test on omni-viper today (the one merge is a test-fixture fix). Best `:latest-dev` spins while waiting on a tag: **duplicate model names per router** (4419, if it lands), **LiteLLM import diff** (4413–4415), **Claude-on-Vertex beta headers** (4416).
