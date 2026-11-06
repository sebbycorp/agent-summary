# agentgateway-updates - 2026-11-06

Hardening day, no shipping: three HTTP/2 connection-pool deadlock/stall fixes landed inside ~90 minutes, GCP `backendAuth` tokens and OTLP access-log bodies merged, Bedrock Mantle got two param fixes — **20 PRs merged across 2 repos, still no release since v1.7.0**.

_Window: merged `>=2026-11-05T11:00Z` (since the 2026-11-05 report). Active: [agentgateway](https://github.com/agentgateway/agentgateway), [website](https://github.com/agentgateway/website); `benchmarks`/`community` idle._

## Releases

**None published in the window:**

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (2026-10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (2026-10-26) | No 1.8 tag — all PRs below are nightly/`latest` only |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (2026-10-27) | – | No docs cut; ref-docs refresh [#1476](https://github.com/agentgateway/website/pull/1476) open |
| benchmarks · community | None | – | Idle |

## Highlights

- **HTTP/2 pool stabilized** — three fixes on pool code merged 2 days ago (from #4273; author: "not impacting any release"): deadlock when a connection dies during the capacity check [#4325](https://github.com/agentgateway/agentgateway/pull/4325), deadlock when a handed-off connection's request dies at once [#4323](https://github.com/agentgateway/agentgateway/pull/4323) (closes #4317), waiters left asleep when the peer stream limit dips then recovers [#4326](https://github.com/agentgateway/agentgateway/pull/4326). Re-test long-lived LLM/MCP streams on nightly — likely the "occasionally wedged upstream" cause.
- **GCP identity end-to-end** — [#4279](https://github.com/agentgateway/agentgateway/pull/4279) (+1359/−487, 23 files): `backendAuth` can send the GCP ID token in `Proxy-Authorization`, so IAP authorizes the gateway while passing the caller's `Authorization` header through untouched. Unblocks "MCP behind IAP without losing end-user identity".
- **OTLP access logs usable** — [#4312](https://github.com/agentgateway/agentgateway/pull/4312): records went out with `body: {}` and no `timeUnixNano` (blank events at ingest); now `body = "POST /v1/messages 200"` + emit time. Update the OTEL slide.
- **Failover virtual models described** — [#4313](https://github.com/agentgateway/agentgateway/pull/4313): `GET /v1/models` now reports `context_window`/`max_tokens`/`input_modalities` for failover models (were `null`), so auto-trimming clients behave the same for every virtual-model type.
- **Bedrock / Mantle** — [#4310](https://github.com/agentgateway/agentgateway/pull/4310) rewrites `max_tokens`→`max_completion_tokens` for gpt models on Mantle (was `400 unsupported_parameter`); open follow-ups on local `CountTokens` fallback and always-on-Claude thinking (see watch list).
- **Helm CRD size fix** — [#4319](https://github.com/agentgateway/agentgateway/pull/4319) renders CRDs as compact JSON at packaging (926→767 KB) after Helm's secret-storage limit broke CI in #4205. Caveat: repo-local `helm install` breaks once CRDs cross the limit again.
- **Yesterday's loose ends** — stdio MCP requests honor timeouts [#4305](https://github.com/agentgateway/agentgateway/pull/4305); `dns.maxTTL` exposed, closes #2810 [#4304](https://github.com/agentgateway/agentgateway/pull/4304); rmcp 3.9 [#4318](https://github.com/agentgateway/agentgateway/pull/4318).
- **K8s + UI** — ReferenceGrant honored for CA lookup [#4315](https://github.com/agentgateway/agentgateway/pull/4315) (+318/−1): cross-namespace cert refs need the grant; UI errors now show in the editor drawer [#4324](https://github.com/agentgateway/agentgateway/pull/4324).
- **Catalog churn** — [#4306](https://github.com/agentgateway/agentgateway/pull/4306) merged, next bump [#4331](https://github.com/agentgateway/agentgateway/pull/4331) up, new provider "Fortytwo" [#4333](https://github.com/agentgateway/agentgateway/pull/4333). Don't hardcode model IDs.

## Merged PRs by repo

### agentgateway (15) — most significant first

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [#4325](https://github.com/agentgateway/agentgateway/pull/4325) | h2: avoid deadlock when connection dies during capacity check | howardjohn | 2026-11-05 |
| agentgateway | [#4323](https://github.com/agentgateway/agentgateway/pull/4323) | h2: avoid deadlock when giving connection while request dies | howardjohn | 2026-11-05 |
| agentgateway | [#4326](https://github.com/agentgateway/agentgateway/pull/4326) | h2: wake waiters left behind when the peer limit dips and recovers | howardjohn | 2026-11-05 |
| agentgateway | [#4279](https://github.com/agentgateway/agentgateway/pull/4279) | auth: allow gcp tokens in backendAuth credentials | onematchfox | 2026-11-05 |
| agentgateway | [#4312](https://github.com/agentgateway/agentgateway/pull/4312) | feat: give OTLP access log records a body and a timestamp | Quentin-M | 2026-11-05 |
| agentgateway | [#4313](https://github.com/agentgateway/agentgateway/pull/4313) | feat(llm): describe failover virtual models in the model list | Quentin-M | 2026-11-05 |
| agentgateway | [#4310](https://github.com/agentgateway/agentgateway/pull/4310) | fix(bedrock): Mantle max_tokens → max_completion_tokens | Quentin-M | 2026-11-05 |
| agentgateway | [#4319](https://github.com/agentgateway/agentgateway/pull/4319) | helm: render CRDs as compact JSON | jgreeer | 2026-11-05 |
| agentgateway | [#4315](https://github.com/agentgateway/agentgateway/pull/4315) | Fix refgrant for CA lookup | howardjohn | 2026-11-05 |
| agentgateway | [#4305](https://github.com/agentgateway/agentgateway/pull/4305) | mcp: respect timeout on stdio requests | howardjohn | 2026-11-05 |

+5 more: [#4320](https://github.com/agentgateway/agentgateway/pull/4320) stale condition, [#4318](https://github.com/agentgateway/agentgateway/pull/4318) rmcp 3.9, [#4304](https://github.com/agentgateway/agentgateway/pull/4304) dns maxTTL, [#4324](https://github.com/agentgateway/agentgateway/pull/4324) UI drawer errors, [#4306](https://github.com/agentgateway/agentgateway/pull/4306) catalog.

### website (5)

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| website | [#1474](https://github.com/agentgateway/website/pull/1474) | Check Helm values in schema doc tests | kristin-kronstain-brown | 2026-11-05 |
| website | [#1471](https://github.com/agentgateway/website/pull/1471) | Document hostRewrite None for transformation-set hosts | artberger | 2026-11-05 |
| website | [#1470](https://github.com/agentgateway/website/pull/1470) | Streaming guardrail latency note | artberger | 2026-11-05 |
| website | [#1465](https://github.com/agentgateway/website/pull/1465) | [Automated] Update reference docs (main) | github-actions[bot] | 2026-11-05 |
| website | [#1473](https://github.com/agentgateway/website/pull/1473) | [Automated] Refresh product-UI screenshots | github-actions[bot] | 2026-11-05 |

### benchmarks · community

None in window.

## Watch list

- **Breaking, still blocked** — [#4300](https://github.com/agentgateway/agentgateway/pull/4300) `(breaking) llm: default to native endpoints on AWS`: off `converse` to `/chat/completions`, `/messages`, `/responses`. Flag unchanged, **different IAM role** — re-test Bedrock streaming + tool-calling on omni-viper with the native-endpoint policy.
- **Cost/metrics follow-ups open** — [#4311](https://github.com/agentgateway/agentgateway/pull/4311), [#4330](https://github.com/agentgateway/agentgateway/pull/4330).
- **MCP / a2a open** — [#4328](https://github.com/agentgateway/agentgateway/pull/4328) OpenAPI-bridge `initialize` negotiation, [#4329](https://github.com/agentgateway/agentgateway/pull/4329) double-public agent-card paths.
- **API-key surface** — shared budget via `metadata.budgetScope` [#4314](https://github.com/agentgateway/agentgateway/pull/4314), policy-name serialization [#4332](https://github.com/agentgateway/agentgateway/pull/4332): re-export demo YAML after upgrading nightly.
- **Preview** — [website#1478](https://github.com/agentgateway/website/pull/1478) Elastic observability guides, [#4316](https://github.com/agentgateway/agentgateway/pull/4316) always include standard log attributes (nicer Loki/OTLP demos).
- **Version drift** — 20+ PRs since v1.7.0 (compression, custom model sources, `uiBasePath`, Bedrock native-first, h2 pool) are nightly/`latest` only; pin `:latest`, note build date. [Open PRs](https://github.com/pulls?q=is%3Apr+is%3Aopen+org%3Aagentgateway).
