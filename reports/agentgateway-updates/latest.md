# agentgateway-updates - 2026-11-07

Protocol-compatibility day: the OpenAPI MCP bridge finally negotiates `initialize` (unblocks Copilot Studio), a2a agent-card paths stop doubling, gRPC local-500s become retryable — **18 PRs merged across 2 repos on Nov 6, still no release since v1.7.0**.

_Window: merged `>=2026-11-06` (since the 2026-11-06 report). Active: [agentgateway](https://github.com/agentgateway/agentgateway), [website](https://github.com/agentgateway/website); `benchmarks`/`community` idle._

## Releases

**None published in the window:**

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (2026-10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (2026-10-26) | No 1.8 tag; everything below is nightly/`latest` only |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (2026-10-27) | – | No docs cut; ref-docs refreshed vs v1.7.0 ([#1476](https://github.com/agentgateway/website/pull/1476)) |
| benchmarks · community | None | – | Idle |

## Highlights

- **MCP behind Copilot Studio works** — [#4328](https://github.com/agentgateway/agentgateway/pull/4328) (fixes #4321): the OpenAPI→MCP adapter answered `initialize` with rmcp's default `ProtocolVersion::LATEST` (`2026-07-28`), which has *no* initialize handshake; clients asking for an initialize-era version (`2025-11-25`) got a modern-only protocol and failed on routing headers / `_meta`. The requested version is now echoed when known, else it falls back to `LATEST_WITH_INITIALIZE`.
- **a2a agent cards stop doubling paths** — [#4329](https://github.com/agentgateway/agentgateway/pull/4329) (fixes #3500): backends advertising interface URLs that already carry the public gateway path produced `/a2a/agents/supervisor/a2a/agents/supervisor/rpc`; the gateway's own agent path is stripped first.
- **gRPC failures become retryable** — [#4226](https://github.com/agentgateway/agentgateway/pull/4226): `ServiceNotFound`, `InvalidBackendType`, `BackendDoesNotExist` mapped to `Unknown` (clients don't retry); now `Unavailable`. Matters for agent-to-agent gRPC mid-rollout.
- **Observability** — [#4316](https://github.com/agentgateway/agentgateway/pull/4316) always includes standard log attributes (cleaner Loki/OTLP); [#4334](https://github.com/agentgateway/agentgateway/pull/4334) adds `config.logging.database.enabled` (default `true`) to switch DB log persistence off, clarified by [#4337](https://github.com/agentgateway/agentgateway/pull/4337) — handy for "no database on the demo laptop".
- **UI** — [#4322](https://github.com/agentgateway/agentgateway/pull/4322) (fixes #2658): the XDS Routes table/drawer counted only *inline* policies, so `targetRef`-attached `AgentgatewayPolicy` showed "Policies: 0"; [#4332](https://github.com/agentgateway/agentgateway/pull/4332) serializes the API-key policy as `apiKey` (was `aPIKey` in `/config_dump`).
- **1.8 contents mapped** — [website#1482](https://github.com/agentgateway/website/pull/1482) folds every pr-tracker release note into one PR: 1.8 breaking changes (upstream #4198, #4185/#4188), features (MCP outcome metric #4182, `upstreamCalls` CEL #4044) and held-for-cut entries — best preview of 1.8 so far.
- **Docs pipeline repaired** — [website#1479](https://github.com/agentgateway/website/pull/1479): the upstream CRD move to `files/crds/` broke nightly doc-tests (34 schema failures) and the kubespec widget; workflows now read `files/crds/` with a `templates/` fallback for pre-1.8 cuts. Theme → `docs-theme-extras` v0.4.1 ([#1483](https://github.com/agentgateway/website/pull/1483)).
- **Build + catalog** — [#4338](https://github.com/agentgateway/agentgateway/pull/4338) fixes the FIPS build (bindgen locked to syn 3 but paired with syn-2-based `prettyplease` 0.2.37 → 0.3.0); [#4331](https://github.com/agentgateway/agentgateway/pull/4331) models.dev refresh ("Nemotron 3 Ultra", pricing added) — don't hardcode model IDs.

## Merged PRs by repo

### agentgateway (11)

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [#4328](https://github.com/agentgateway/agentgateway/pull/4328) | fix(mcp): negotiate OpenAPI initialize protocol version | WillXing | 2026-11-06 |
| agentgateway | [#4329](https://github.com/agentgateway/agentgateway/pull/4329) | fix(a2a): don't double already-public agent card interface paths | jamesilse | 2026-11-06 |
| agentgateway | [#4226](https://github.com/agentgateway/agentgateway/pull/4226) | fix(proxy): answer a local 500 of a gRPC call as Unavailable | fiunchinho | 2026-11-06 |
| agentgateway | [#4322](https://github.com/agentgateway/agentgateway/pull/4322) | fix(ui): count targetRef policies on dump routes | jamesilse | 2026-11-06 |
| agentgateway | [#4334](https://github.com/agentgateway/agentgateway/pull/4334) | add ability to disable log persistence to a db | conradhanson | 2026-11-06 |
| agentgateway | [#4316](https://github.com/agentgateway/agentgateway/pull/4316) | log: always include standard attributes | howardjohn | 2026-11-06 |
| agentgateway | [#4332](https://github.com/agentgateway/agentgateway/pull/4332) | fix: serialize API key policy name correctly | warjiang | 2026-11-06 |
| agentgateway | [#4338](https://github.com/agentgateway/agentgateway/pull/4338) | build: give bindgen a prettyplease on its own syn (FIPS) | markuskobler | 2026-11-06 |

+3 more: [#4336](https://github.com/agentgateway/agentgateway/pull/4336) fuzz build fix, [#4337](https://github.com/agentgateway/agentgateway/pull/4337) logging-db doc clarify, [#4331](https://github.com/agentgateway/agentgateway/pull/4331) model catalog.

### website (7)

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| website | [#1482](https://github.com/agentgateway/website/pull/1482) | Release notes: combine pr-tracker entries for 1.8 and 1.7 | kristin-kronstain-brown | 2026-11-06 |
| website | [#1479](https://github.com/agentgateway/website/pull/1479) | Read agentgateway CRDs from files/crds after upstream move | kristin-kronstain-brown | 2026-11-06 |
| website | [#1483](https://github.com/agentgateway/website/pull/1483) | Bump docs-theme-extras to v0.4.1 | kristin-kronstain-brown | 2026-11-06 |
| website | [#1437](https://github.com/agentgateway/website/pull/1437) | [Automated] Draft docs: CEL outbound calls as `upstreamCalls` | github-actions[bot] | 2026-11-06 |

+3 automated: [#1447](https://github.com/agentgateway/website/pull/1447) MCP-outcome docs, [#1476](https://github.com/agentgateway/website/pull/1476) ref docs from v1.7.0, [#1477](https://github.com/agentgateway/website/pull/1477) UI screenshots.

### benchmarks · community

None in window.

## Watch list

- **Breaking, still open** — [#4300](https://github.com/agentgateway/agentgateway/pull/4300) `(breaking) llm: default to native endpoints on AWS` (off `converse` → `/chat/completions`, `/messages`, `/responses`; **different IAM role**), [#4213](https://github.com/agentgateway/agentgateway/pull/4213) `(breaking) llm: custom provider formats configure endpoints instead of gating them`. Re-test Bedrock streaming + tool-calling on omni-viper before either lands.
- **Guardrails** — [#4345](https://github.com/agentgateway/agentgateway/pull/4345) run guardrails on errors and server-initiated MCP messages; [#4341](https://github.com/agentgateway/agentgateway/pull/4341) webhook writeback without clobbering non-text content. Security-demo relevant.
- **Auth/budget** — [#4343](https://github.com/agentgateway/agentgateway/pull/4343) keep API-key budget across key regeneration; [#4142](https://github.com/agentgateway/agentgateway/pull/4142) max trust age for remote JWT signing keys.
- **LLM/CRD** — [#4344](https://github.com/agentgateway/agentgateway/pull/4344) missing `finish_reason` = failure, not stop; [#4346](https://github.com/agentgateway/agentgateway/pull/4346) reject duplicate model names per router; [#4339](https://github.com/agentgateway/agentgateway/pull/4339) CRD fix for `authorization.matchExpressions` (regenerate ref docs).
- **Docs incoming** — [website#1484](https://github.com/agentgateway/website/pull/1484) SDK guide (Go + LiteLLM migration), [#1487](https://github.com/agentgateway/website/pull/1487)–[#1490](https://github.com/agentgateway/website/pull/1490) docs-vs-code fixes, [#1455](https://github.com/agentgateway/website/pull/1455) `ui://` delimiter.
- **Version drift** — ~30 PRs since v1.7.0 (h2 pool fixes, GCP `backendAuth`, OTLP body, compression, `uiBasePath`, MCP/a2a) are nightly/`latest` only; pin `:latest` and note the build date. [Open PRs](https://github.com/pulls?q=is%3Apr+is%3Aopen+org%3Aagentgateway).
