# agentgateway-updates - 2026-11-14

**19 PRs merged across core + docs: webhook guardrails stop destroying tool calls, the proxy rejects duplicate `Host` headers, Helm/Prometheus-Operator installs unblocked, docs caught up with compression + OpenMetrics; still no 1.8 tag.**

_Window: merged/published `>=2026-11-13`. Active: [agentgateway](https://github.com/agentgateway/agentgateway) (14 merges), [website](https://github.com/agentgateway/website) (5). `benchmarks` idle since 10-28; `community` no merges._

## Releases

**None in window.** Newest artifact is the Oct 27 cut — everything since (LLM-IR, Bedrock fixes, catalog refreshes, today's work) is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) "Docs for v1.7.0" (10-27) | – |
| benchmarks · community | None | – |

## Highlights

- **Webhook guardrails no longer wreck the request they inspect** — [4341](https://github.com/agentgateway/agentgateway/pull/4341). A webhook only sees `{role, content}` text; on a mask the gateway **rebuilt the request from that text**, dropping tool calls, tool-call IDs, images and `cache_control`. Edits now write back in place; fixes post-mask 400s from OpenAI and Anthropic. **Demo impact:** "mask PII then keep using tools" was fragile — use `:latest-dev`.
- **Silent file-drop fixed on Messages → Responses** — [4127](https://github.com/agentgateway/agentgateway/pull/4127). An Anthropic `document` bound for an OpenAI Responses backend was ignored, so the model answered as if no file was attached. Now maps to `input_file`/`input_text`; unsupported sources error rather than drop.
- **Helm on Prometheus Operator < v0.88 unblocked** — [4370](https://github.com/agentgateway/agentgateway/pull/4370). The chart always rendered `scrapeNativeHistograms` (a v0.88 field) so `helm upgrade` died with `field not declared in schema`; now emitted only when `true`.
- **Duplicate `Host` headers rejected** — [4401](https://github.com/agentgateway/agentgateway/pull/4401). Protocol hardening of the sort that surfaces in security review.
- **Delegated child routes report the truth** — [4353](https://github.com/agentgateway/agentgateway/pull/4353). `ResolvedRefs` was hard-coded `True` even when the child's backend failed to resolve — route dropped, status green.
- **FIPS TLS misconfig surfaces at config time** — [4379](https://github.com/agentgateway/agentgateway/pull/4379). Non-approved cipher suites now warn at xDS conversion, not at first handshake.
- **`matchExpressions` was documented backwards** — [4339](https://github.com/agentgateway/agentgateway/pull/4339). Lines are independent: Allow/Deny match on **any**, Require on **all**. Re-check agent-generated policies.
- **Admin UI: LLM settings finally persist** — [4374](https://github.com/agentgateway/agentgateway/pull/4374). Saves to the `llm.settings` DB row; hybrid-mode Save no longer permanently disabled.
- **Unreleased features got docs** — [1521](https://github.com/agentgateway/website/pull/1521) covers the #4178 compression policy (`main` only); [1480](https://github.com/agentgateway/website/pull/1480) covers 1.7's `/metrics` shift to `application/openmetrics-text` — a Datadog Agent without `use_latest_spec: true` silently dropped *every* counter on 1.6.
- **API reference missing fields on 31 types** — [1538](https://github.com/agentgateway/website/pull/1538). Embedded-struct fields were skipped, so `OAuthTokenExchange`, `ExtAuth`, `ExtProc`, `Tracing`, `RemoteJWKS` were documented **without** `backendRef`/`url`.
- **Model catalog refreshed** — [4377](https://github.com/agentgateway/agentgateway/pull/4377). Adds Claude Fable 5.2, Gemini 4 Carbon/Hydrogen, Gemma 4, `gpt-image-1.5`; deprecates Claude Sonnet 4.5; big `temperature` false→true flip across Bedrock/Azure/OpenAI/Copilot. Check pinned models before a cost demo.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4341](https://github.com/agentgateway/agentgateway/pull/4341) | webhook guardrails: writeback without clobbering non-text content | stevenctl | 11-13 |
| agentgateway | [4127](https://github.com/agentgateway/agentgateway/pull/4127) | fix(llm): convert Messages documents to Responses input files | onhate | 11-13 |
| agentgateway | [4370](https://github.com/agentgateway/agentgateway/pull/4370) | fix(helm): only render `scrapeNativeHistograms` when enabled | AlecGoncharow | 11-13 |
| agentgateway | [4401](https://github.com/agentgateway/agentgateway/pull/4401) | proxy: reject duplicate Host header | howardjohn | 11-13 |
| agentgateway | [4353](https://github.com/agentgateway/agentgateway/pull/4353) | fix: report real ResolvedRefs on delegated child routes | dongjiang1989 | 11-13 |
| agentgateway | [4379](https://github.com/agentgateway/agentgateway/pull/4379) | Warn on FIPS-rejected frontend TLS policy options | ymesika | 11-13 |
| agentgateway | [4339](https://github.com/agentgateway/agentgateway/pull/4339) | fix(api): correct authorization matchExpressions description | jamesilse | 11-13 |
| agentgateway | [4374](https://github.com/agentgateway/agentgateway/pull/4374) | ui: make llm settings save to db | stevenctl | 11-13 |
| agentgateway | [3663](https://github.com/agentgateway/agentgateway/pull/3663) | fix(import): report LiteLLM cache migration requirements | ManateeBen | 11-13 |
| agentgateway | [4377](https://github.com/agentgateway/agentgateway/pull/4377) · [4399](https://github.com/agentgateway/agentgateway/pull/4399) · [4373](https://github.com/agentgateway/agentgateway/pull/4373) · [4404](https://github.com/agentgateway/agentgateway/pull/4404) · [4403](https://github.com/agentgateway/agentgateway/pull/4403) | chores: model catalog refresh, rust 1.100, cel-derive test fixtures, OTel → 1.47 | bots, howardjohn, markuskobler | 11-13/14 |
| website | [1521](https://github.com/agentgateway/website/pull/1521) | docs: compression policy guides for `main` | kristin-kronstain-brown | 11-13 |
| website | [1480](https://github.com/agentgateway/website/pull/1480) | Metrics format (1.7 OpenMetrics Content-Type) | kristin-kronstain-brown | 11-13 |
| website | [1538](https://github.com/agentgateway/website/pull/1538) | Document embedded-struct fields in shared types (31 fixed) | kristin-kronstain-brown | 11-13 |
| website | [1046](https://github.com/agentgateway/website/pull/1046) · [1537](https://github.com/agentgateway/website/pull/1537) | GKE Cluster Toolkit install guide; docs-theme-extras v0.5.0 | danehans, kristin-kronstain-brown | 11-13 |

All 19 in-window merges covered (agentgateway 14, website 5); full list at [merged PRs `>=2026-11-13`](https://github.com/pulls?q=is%3Apr+is%3Amerged+org%3Aagentgateway+merged%3A%3E%3D2026-11-13).

## Watch list

| Repo | PR | Why | Author |
| --- | --- | --- | --- |
| agentgateway | [4387](https://github.com/agentgateway/agentgateway/pull/4387) | **API/CRD change**, open+blocked: prompt-guard `policies` hoisted to top level + per-type CEL validation; CRD 1.23→1.08 MB. Breaks saved guardrail manifests | jgreeer |
| agentgateway | [4392](https://github.com/agentgateway/agentgateway/pull/4392) | Open+blocked: `bedrockRuntimeApi: native \| converse` per provider — fixes Codex losing `custom` tools on Bedrock Runtime; no xDS field yet | Quentin-M |
| agentgateway | [4402](https://github.com/agentgateway/agentgateway/pull/4402) · [4405](https://github.com/agentgateway/agentgateway/pull/4405) · [4398](https://github.com/agentgateway/agentgateway/pull/4398) | Gemini Decisions API routing; `basicAuth` bcrypt-cost cap (DoS); non-blocking db runtime | markuskobler, howardjohn |
| agentgateway | [4408](https://github.com/agentgateway/agentgateway/pull/4408) · [4406](https://github.com/agentgateway/agentgateway/pull/4406) | Azure response model-name normalization; provider catalogs keyed by the gateway's provider key — both hit routing demos | howardjohn |
| agentgateway | [4409](https://github.com/agentgateway/agentgateway/pull/4409) · [4407](https://github.com/agentgateway/agentgateway/pull/4407) | Draft antd UI migration; UI fixes (admin-port warning, playground CORS, provider aliases) | Charlesthebird |
| agentgateway | [4394](https://github.com/agentgateway/agentgateway/pull/4394) · [4391](https://github.com/agentgateway/agentgateway/pull/4391) | Isolate remote JWKS fetch failures (IdP outage ≠ total authN failure); draft MCP **Skills extension** | shubhamojha1, bdubs00 |
| agentgateway | [4410](https://github.com/agentgateway/agentgateway/pull/4410) · [3637](https://github.com/agentgateway/agentgateway/pull/3637) | New Cognito JWT example; Keycloak RFC 9207 fix **open since 09-23** still gates token-exchange demos | jamesilse, Tazmainiandevil |
| website | [1539](https://github.com/agentgateway/website/pull/1539) · [1540](https://github.com/agentgateway/website/pull/1540) | "Previous releases" in the version dropdown; doc tests schema-check only the PR's pages | kristin-kronstain-brown |

Nothing to re-test on omni-viper for a tag. Best `:latest-dev` spins today: **webhook guardrails with tool history** (4341), **Helm against an older Prometheus Operator** (4370), and the blocked API-shape PRs **4387 / 4392** ahead of a 1.8 cut.
