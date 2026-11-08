# agentgateway-updates - 2026-11-08

Zero PRs merged org-wide since the Nov 7 report and still no tag past **v1.7.0** — but ~20 new PRs opened in the last 36h, headlined by MCP guardrails closing a text-leak gap, aborted LLM streams becoming real errors, and a first shot at prompt tokenization.

_Window: merged/published `>=2026-11-07` (since the 2026-11-07 report). Active: [agentgateway](https://github.com/agentgateway/agentgateway), [website](https://github.com/agentgateway/website); `benchmarks`/`community` idle._

## Releases

**None published in the window** — the newest artifact is still the Oct 27 cut:

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (2026-10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (2026-10-26) | No 1.8 tag; post-v1.7.0 work is nightly/`:latest` only |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) "Docs for v1.7.0" (2026-10-27) | – | No docs cut; ref-docs refresh open as [#1485](https://github.com/agentgateway/website/pull/1485) |
| benchmarks · community | None | – | Idle |

## Highlights

Everything below is **open**, not merged — useful as the leading edge to test on nightly, not to quote as shipped.

- **MCP guardrails had a bypass** — [#4345](https://github.com/agentgateway/agentgateway/pull/4345) (fixes #4245): `mcpGuardrails` inspected only JSON-RPC *results*; protocol errors, progress notifications and server-initiated requests (e.g. elicitation) on the same call skipped the response hook, so their text reached the client **and the access log** unfiltered. Security-demo relevant.
- **Truncated generations reported as success** — [#4344](https://github.com/agentgateway/agentgateway/pull/4344) (fixes #4220): a `chat.completions` stream ending with `[DONE]` but a null `finish_reason` (Copilot Gemini after long reasoning) mapped to `end_turn` / `response.completed`; proposed: `502 api_error` / `response.failed`. Explains agents that "finish" mid-thought.
- **Claude Code + Bedrock broken by `thinking: disabled`** — [#4330](https://github.com/agentgateway/agentgateway/pull/4330): Opus 5.5 / Fable 5 / Fable 5.1 400 on `{"type":"disabled"}`, Sonnet 5.5 wants `between_tools`, and the gateway forwarded the 400 — Claude Code's default auto-mode classifier denied every action. New catalog tags (`always_on_thinking`, `between_tools_thinking`) pick the rewrite per model.
- **API-key budgets reset on rotation** — [#4343](https://github.com/agentgateway/agentgateway/pull/4343) (fixes #4283): the budget key was the SHA-256 of the secret, so regenerating a key opened a fresh counter. Anchors on the stable `agentgateway.dev/id` instead.
- **Duplicate model names ambiguous** — [#4346](https://github.com/agentgateway/agentgateway/pull/4346) (fixes #4303): two `AgentgatewayModel`s with the same `match.model` on one listener/route now go oldest-wins, newer left unprogrammed with `Accepted=False` / `Conflicted`; wildcards out of scope.
- **Webhook guardrail masks broke requests** — [#4341](https://github.com/agentgateway/agentgateway/pull/4341): a mask rebuilt the request from text-only `{role, content}`, dropping tool calls/ids, images and `cache_control` (→ provider 400s); edits now write in place.
- **New security surface proposed** — [#4349](https://github.com/agentgateway/agentgateway/pull/4349): prompt tokenization / pseudonymization (email first) as a built-in guardrail, positioned against LiteLLM's Presidio integration.
- **Provider parity churn** — [#4350](https://github.com/agentgateway/agentgateway/pull/4350) (fixes #4193) anchors Copilot Responses `output_index` to its first `item.id`; [#4339](https://github.com/agentgateway/agentgateway/pull/4339) fixes the `authorization.matchExpressions` CRD description (Allow/Deny match *any* row, Require all) — regenerate ref docs after it lands.
- **Docs pipeline** — [#1484](https://github.com/agentgateway/website/pull/1484) turns the OpenAI SDK pages into an SDK guide (Go via `openai-go/v3`, `openai/*` + `anthropic/*` prefixes, LiteLLM migration, JWT-as-api-key) and fixes the Kubernetes `base_url` to end in `/v1`; [#1480](https://github.com/agentgateway/website/pull/1480) explains the 1.7 `/metrics` Content-Type change — why a Datadog Agent without `use_latest_spec: true` silently drops every counter; [#1478](https://github.com/agentgateway/website/pull/1478) adds Elastic guides.

## Merged PRs by repo

**None in window.** No PR merged and no commit landed on `main` in either repo since 2026-11-06; the last merge was [agentgateway#4328](https://github.com/agentgateway/agentgateway/pull/4328) at 20:47Z Nov 6, and the newest `main` commits ([#4310](https://github.com/agentgateway/agentgateway/pull/4310), [#4312](https://github.com/agentgateway/agentgateway/pull/4312), [#4313](https://github.com/agentgateway/agentgateway/pull/4313), [#4305](https://github.com/agentgateway/agentgateway/pull/4305)) are Nov 5–6, already covered on Nov 7. [Open PRs](https://github.com/pulls?q=is%3Apr+is%3Aopen+org%3Aagentgateway).

## Watch list

Open work opened/updated in the window — worth a nightly pull on omni-viper:

| Repo | PR # | Title | Author |
| --- | --- | --- | --- |
| agentgateway | [#4350](https://github.com/agentgateway/agentgateway/pull/4350) | fix: stabilize item IDs in native Copilot Responses streams | NotSleeply |
| agentgateway | [#4349](https://github.com/agentgateway/agentgateway/pull/4349) | feat(tokenization): streaming token reassembly (new guardrail) | deem0n |
| agentgateway | [#4345](https://github.com/agentgateway/agentgateway/pull/4345) · [#4341](https://github.com/agentgateway/agentgateway/pull/4341) | guardrails on MCP errors/server messages · webhook writeback keeps non-text | ZYZ666-RGB, stevenctl |
| agentgateway | [#4344](https://github.com/agentgateway/agentgateway/pull/4344) | fix: treat missing `finish_reason` as failure, not stop | ZYZ666-RGB |
| agentgateway | [#4346](https://github.com/agentgateway/agentgateway/pull/4346) | fix(controller): reject duplicate model names on one router | ZYZ666-RGB |
| agentgateway | [#4343](https://github.com/agentgateway/agentgateway/pull/4343) | fix: keep API key budget across key regeneration | ZYZ666-RGB |
| agentgateway | [#4330](https://github.com/agentgateway/agentgateway/pull/4330) | fix(llm): translate `disabled` thinking for always-on Bedrock Claude | Quentin-M |
| agentgateway | [#4339](https://github.com/agentgateway/agentgateway/pull/4339) · [#4347](https://github.com/agentgateway/agentgateway/pull/4347) | `matchExpressions` CRD description · pending model catalog | jamesilse, gha[bot] |
| website | [#1484](https://github.com/agentgateway/website/pull/1484) · [#1480](https://github.com/agentgateway/website/pull/1480) · [#1481](https://github.com/agentgateway/website/pull/1481) · [#1478](https://github.com/agentgateway/website/pull/1478) | SDK guide · metrics format · Datadog · Elastic | jamesilse-solo, kristin-kronstain-brown, haowu1234 |
| website | [#1487](https://github.com/agentgateway/website/pull/1487)–[#1490](https://github.com/agentgateway/website/pull/1490) | Docs-vs-code fixes (JWT `preserveToken`, sessionAffinity, ListenerSet precedence, dynamic backend) | github-actions[bot] |

Also open: UI-only [#4342](https://github.com/agentgateway/agentgateway/pull/4342), [#4340](https://github.com/agentgateway/agentgateway/pull/4340) (stevenctl).

- **Model-catalog churn to test before demoing** — pending [#4347](https://github.com/agentgateway/agentgateway/pull/4347) adds Bedrock `moonshotai.kimi-k3.1`, Fireworks `kimi-k3p1-pro` and a `deepseek-v4p1-flash-1017-ultrafast` router, **deletes** Fireworks `gpt-oss-120b`, `inkling` and `nemotron-3-ultra-nvfp4`, and drops `reasoning.budgetTokens` from the Kimi rows. Don't hardcode model IDs in a deck.
- **1.8 is staged, not cut** — the combined release-note PR [website#1482](https://github.com/agentgateway/website/pull/1482) keeps entries marked "held for the 1.8 release cut"; expect a 1.8 window and re-check those held items when the RC appears.
- **Docs automation gaps** — nightly screenshot refresh [#1486](https://github.com/agentgateway/website/pull/1486) warns "capture or sync failed for `latest main`", so the committed UI images are incomplete; ref-docs refresh [#1485](https://github.com/agentgateway/website/pull/1485) is unmerged, meaning published CRD/schema docs lag `main`.
- **Version drift unchanged** — ~30 merged PRs since v1.7.0 remain nightly/`:latest`-only; pin `:latest` and note the build date when demoing.
