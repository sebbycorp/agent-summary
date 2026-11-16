# agentgateway-updates - 2026-11-16

**A frozen weekend: zero merges, zero releases — `agentgateway` main is still at the 11-14 commit. But 4 new PRs opened Sun/Mon, headlined by a CRD/xDS API change (CEL `replace` for header transforms, #4421) and the Claude-Code-on-Bedrock beta-header fix (#4420). Still no 1.8 tag.**

_Window: merged/pushed/updated `>=2026-11-15` (last report covered `>=2026-11-14`). All four repos have **no commits and no merges** in the window; activity is new/updated open PRs in `agentgateway` plus automated docs in `website`. `community`/`benchmarks` idle. [PRs updated in window][updated]._

## Releases

**None in window.** Newest artifact is still the Oct 27 cut; no tags past `v1.7.0`. Everything merged since (LLM-IR, Bedrock fixes, webhook-guardrail writeback, catalog refreshes) is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0][ag-170] (10-27) | [v1.7.0-rc.1][ag-170rc1] (10-26) |
| website | [v1.7.0 "Docs for v1.7.0"][ws-170] (10-27) | – |
| benchmarks · community | None | – |

## Highlights

- **CRD/API change coming: CEL `replace` reaches Kubernetes** — [#4421][pr-4421] (dongjiang1989, 11-16). `replace` for header transforms existed since #2589 but only in **local file config** — the xDS proto and `AgentgatewayPolicy` CRD never carried it, so on K8s you cannot express a header allowlist. Adds `optional string replace = 6` to `TransformationPolicy.Transform` and `Replace *CELExpression` to the CRD `Transform` (same `isCEL` validation), regenerating deepcopy + Helm CRD YAMLs; fixes #4384 by raising the replace-failure log debug→warn, since a failed replace **fails open** and keeps the original headers. **Demo impact:** the K8s customer currently has to fall back to standalone config.
- **Claude Code on Bedrock stops 400-ing on beta headers** — [#4420][pr-4420] (Quentin-M, 11-16). Claude Code sends `advanced-tool-use-2025-11-20`, which Bedrock has no name for and rejects; this rewrites it to Bedrock's equivalents (`tool-search-tool-2025-10-19` + `tool-examples-2025-10-29`), deduped against client-sent names, on Converse and native, and adds 8 accepted-but-missing betas (`compact-2026-09-04`, `computer-use-2024-10-22`, `inline-tools-2026-09-15`, two `thinking-*`, …), excluding `compact-2026-09-04` from Converse. Author probed every beta live on Opus 5.5 / Sonnet 5.5 / Haiku 4.5 / Fable 5.1–5.2: no 400s in Claude Code auto mode. Caveat: native requests still **skip** the allowlist unless `AGENTGATEWAY_BEDROCK_ANTHROPIC_BETA_HEADERS` is set.
- **Retry in the simplified standalone LLM config** — [#4424][pr-4424] (dimetron, 11-16, fixes #4423). `llm.policies` accepted `timeout` but not `retry`, so standalone LLM routes could not retry provider 429s/5xx. Adds `retry` to `LocalLLMPolicy` through `split_policies` into `TrafficPolicy::Retry`; schema regenerated. Kills a quickstart footgun.
- **Duplicate model names per router — still pending** — [#4419][pr-4419] (mangopep, 11-15, +2,800/−10, 11 files). Two same-named models on one listener coexist and either can answer; the fix keeps the winner (oldest, then namespace/name) and marks the loser `Accepted=False, Conflicted`, so a new model can't silently take over live traffic. **Blocked on review** since it opened — good canary/shadow caveat slide.
- **1.8 notes keep assembling, no cut** — website [#1519][pr-1519] "Release notes from the PR tracker" (touched 11-15), plus four automated draft-docs PRs refreshed this morning: [1542][pr-1542] fast-mode pricing, [1543][pr-1543] helm histograms, [1544][pr-1544] guardrail writeback, [1545][pr-1545] UI LLM settings.
- **The LiteLLM importer stalled** — [4413][pr-4413] · [4414][pr-4414] · [4415][pr-4415] unchanged since 11-14 19:50 UTC, as are [4416][pr-4416] and [4417][pr-4417]. Yesterday's "migration lands any minute" call needs patience.
- **No security fixes or CVEs** merged or updated in the window.

## Merged PRs by repo

**None in window.** `org:agentgateway is:pr is:merged merged:>=2026-11-15` → 0 results; agentgateway main HEAD still [`18f8370`][c-ag] (11-14 00:08), website main HEAD still [`33cf4cd`][c-ws] (11-13 22:08). New PRs opened in the window instead:

| Repo | PR | Title | Author | Opened |
| --- | --- | --- | --- | --- |
| agentgateway | [#4421][pr-4421] | feat(transformation): expose CEL replace mode in xDS proto and AgentgatewayPolicy CRD | dongjiang1989 | 11-16 |
| agentgateway | [#4420][pr-4420] | bedrock: map advanced-tool-use and refresh the Anthropic beta allowlist | Quentin-M | 11-16 |
| agentgateway | [#4424][pr-4424] | feat(llm): support retry in standalone llm.policies | dimetron | 11-16 |
| agentgateway | [#4419][pr-4419] | fix(controller): reject duplicate exact model names per router | mangopep | 11-15 |
| website | [#1547][pr-1547] | [Automated] Refresh product-UI screenshots | github-actions[bot] | 11-16 |
| website | [1542][pr-1542] · [1543][pr-1543] · [1544][pr-1544] · [1545][pr-1545] | [Automated] Draft docs (fast-mode pricing, helm histograms, guardrail writeback, UI LLM settings) | github-actions[bot] | 11-15 |

## Watch list

| Repo | PR | Why |
| --- | --- | --- |
| agentgateway | [#4421][pr-4421] · [#4419][pr-4419] | **xDS proto + CRD / status changes**: `replace` on header transforms (regenerate CRDs, then test a header allowlist on omni-viper); duplicate model names → `Accepted=False/Conflicted`. Both blocked |
| agentgateway | [#4420][pr-4420] · [#4424][pr-4424] | Bedrock beta allowlist (native-mode default undecided); `llm.policies.retry` + regenerated schema |
| agentgateway | [4413][pr-4413] · [4414][pr-4414] · [4415][pr-4415] · [4416][pr-4416] · [4417][pr-4417] | LiteLLM importer expansion (keys, budgets, MCP, guardrails, timeouts); Vertex `anthropic-beta` filter + env override; Devin Desktop ACP recipe (replaces the broken Windsurf proxy one) — all frozen since 11-14 |
| agentgateway | [4392][pr-4392] · [4330][pr-4330] · [4184][pr-4184] | Updated in window: `bedrockRuntimeApi: native\|converse` per provider (Codex losing `custom` tools, **no xDS field yet**); disabled-thinking on always-on Bedrock Claude; mid-conversation system messages |
| website | [1519][pr-1519] · [1526][pr-1526] · [1527][pr-1527] · [4418][pr-4418] | The 1.8 release-notes PR, reference-doc regens and the model-catalog refresh — watch these for the cut |
| website | [1478][pr-1478] · [1523][pr-1523] | Elastic observability guides; OSS assistant skills — day-2 demo material |

Nothing to re-test on omni-viper today (no merged behaviour change). Best `:latest-dev` spins while waiting on 1.8: **header-transform `replace` on K8s** (4421, post-merge with new CRDs), **Claude Code → Bedrock beta headers** (4420), **standalone `llm.policies.retry`** (4424).

[updated]: https://github.com/pulls?q=is%3Apr+org%3Aagentgateway+updated%3A%3E%3D2026-11-15
[ag-170]: https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0
[ag-170rc1]: https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1
[ws-170]: https://github.com/agentgateway/website/releases/tag/v1.7.0
[c-ag]: https://github.com/agentgateway/agentgateway/commit/18f83707118b3e2df500cc63299486958a0e86f5
[c-ws]: https://github.com/agentgateway/website/commit/33cf4cdf4b4c1d02119b7cef788fcf2eca953ac9
[pr-4421]: https://github.com/agentgateway/agentgateway/pull/4421
[pr-4420]: https://github.com/agentgateway/agentgateway/pull/4420
[pr-4424]: https://github.com/agentgateway/agentgateway/pull/4424
[pr-4419]: https://github.com/agentgateway/agentgateway/pull/4419
[pr-4418]: https://github.com/agentgateway/agentgateway/pull/4418
[pr-4417]: https://github.com/agentgateway/agentgateway/pull/4417
[pr-4416]: https://github.com/agentgateway/agentgateway/pull/4416
[pr-4415]: https://github.com/agentgateway/agentgateway/pull/4415
[pr-4414]: https://github.com/agentgateway/agentgateway/pull/4414
[pr-4413]: https://github.com/agentgateway/agentgateway/pull/4413
[pr-4392]: https://github.com/agentgateway/agentgateway/pull/4392
[pr-4330]: https://github.com/agentgateway/agentgateway/pull/4330
[pr-4184]: https://github.com/agentgateway/agentgateway/pull/4184
[pr-1547]: https://github.com/agentgateway/website/pull/1547
[pr-1545]: https://github.com/agentgateway/website/pull/1545
[pr-1544]: https://github.com/agentgateway/website/pull/1544
[pr-1543]: https://github.com/agentgateway/website/pull/1543
[pr-1542]: https://github.com/agentgateway/website/pull/1542
[pr-1527]: https://github.com/agentgateway/website/pull/1527
[pr-1526]: https://github.com/agentgateway/website/pull/1526
[pr-1523]: https://github.com/agentgateway/website/pull/1523
[pr-1519]: https://github.com/agentgateway/website/pull/1519
[pr-1478]: https://github.com/agentgateway/website/pull/1478
