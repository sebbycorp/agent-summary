# agentgateway-updates - 2026-11-09

Quiet day upstream: **1 core PR merged** (UI dropdown fix) and **no new tag** (still v1.7.0), but the docs bot landed 13 draft-docs PRs publishing the **1.8 release notes** — two of them breaking — and two security fixes opened today (OIDC `azp`, brotli CVE).

_Window: merged/published `>=2026-11-08` (since the 2026-11-08 report). Active: [agentgateway](https://github.com/agentgateway/agentgateway), [website](https://github.com/agentgateway/website); `benchmarks`/`community` idle._

## Releases

**None published in the window** — newest artifact is still the Oct 27 cut.

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) | No 1.8 tag; post-v1.7.0 work is nightly/`:latest-dev` only |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | – | Docs run ahead of the tag: ref docs + 1.8 notes live in the `main` trees |
| benchmarks · community | None | – | Idle |

## Highlights

- **1.8 documented before it's tagged** — 13 automated docs PRs merged today into the `main` trees under a **new 1.8 section**; good deck material, not shipped until a tag exists.
- **Breaking: model transformations rewrite provider URL paths** — [website 1441](https://github.com/agentgateway/website/pull/1441) (code [4185](https://github.com/agentgateway/agentgateway/pull/4185), [4188](https://github.com/agentgateway/agentgateway/pull/4188)): Bedrock/Vertex/Gemini requests carrying the model **in the path** are now rewritten by `transformation.model`, and `passthrough` means "always pass through" again, not "handle unknown paths". Re-check Bedrock/Vertex passthrough demos.
- **Breaking: invalid backend TLS fails closed** — [website 1459](https://github.com/agentgateway/website/pull/1459): a bad PEM/hostname/SAN/kx-group used to drop the policy and send **plaintext** upstream; now requests get `503` plus an "invalid backend TLS policy" warning.
- **Breaking, pending — hits standalone demos** — [4356](https://github.com/agentgateway/agentgateway/pull/4356): `jwtAuth`/`apiKey`/`basicAuth` silently defaulted to `mode: optional`, letting anonymous traffic through; the PR defaults all three to `strict` (401). K8s already defaulted strict; configs relying on unauthenticated requests must set `mode: optional`.
- **OIDC hardening (pending)** — [4357](https://github.com/agentgateway/agentgateway/pull/4357): bearer ID tokens must name the client in `azp` (required for multi-value `aud`), access-typed tokens rejected. Matters for Okta/Keycloak multi-audience.
- **Supply chain (pending)** — [4358](https://github.com/agentgateway/agentgateway/pull/4358) bumps `brotli` to 9.1.1 for **CVE-2026-111497**; still blocked on review, so shipped binaries carry the vulnerable version.
- **New demo material in published docs**: Nebius Token Factory preset (`provider: nebius`, [1448](https://github.com/agentgateway/website/pull/1448)); `llm.originalModel` CEL + response→request→original cost fallback ([1442](https://github.com/agentgateway/website/pull/1442)); `/metrics` over **HBONE** when stats binds non-loopback ([1452](https://github.com/agentgateway/website/pull/1452)); `AGENTGATEWAY_MCP_UI_URI_DELIMITER` for MCP Apps hosts that reject `+` ([1455](https://github.com/agentgateway/website/pull/1455)).
- **SDK-visible behaviour**: Chat Completions streams now **always** append a usage chunk before `[DONE]`, even with `stream_options.include_usage: false` ([1438](https://github.com/agentgateway/website/pull/1438)); streamed response CEL sees `llm.provider`/`llm.requestModel`/`llm.streaming`, still not `llm.responseModel` ([1453](https://github.com/agentgateway/website/pull/1453)). Duplicate MCP target names rejected standalone, deterministic in K8s ([1446](https://github.com/agentgateway/website/pull/1446)).
- **Only core change landed**: UI dropdowns size to the longest option ([4342](https://github.com/agentgateway/agentgateway/pull/4342)) — fixes the model picker truncating long model IDs mid-demo.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4342](https://github.com/agentgateway/agentgateway/pull/4342) | ui: make dropdowns wide enough to see all options | stevenctl | 11-08 |
| website | [1493](https://github.com/agentgateway/website/pull/1493) | Link fixes (weekly) | kristin-kronstain-brown | 11-09 |
| website | [1485](https://github.com/agentgateway/website/pull/1485) | Update reference docs (main) — API/CRD, Helm, agctl | bot | 11-09 |
| website | [1486](https://github.com/agentgateway/website/pull/1486) | Refresh product-UI screenshots | bot | 11-09 |
| website | [1459](https://github.com/agentgateway/website/pull/1459) | Draft docs: fail closed on unbuildable backend TLS | bot | 11-09 |
| website | [1441](https://github.com/agentgateway/website/pull/1441) | Draft docs: passthrough + path-based transformations | bot | 11-09 |
| website | [1448](https://github.com/agentgateway/website/pull/1448) | Draft docs: Nebius Token Factory provider | bot | 11-09 |
| website | [1438](https://github.com/agentgateway/website/pull/1438) | Draft docs: `stream_options` usage chunk | bot | 11-09 |

+9 more, all `[Automated] Draft docs` on website: HBONE metrics [1452](https://github.com/agentgateway/website/pull/1452), `llm.originalModel` [1442](https://github.com/agentgateway/website/pull/1442), MCP `ui://` delimiter [1455](https://github.com/agentgateway/website/pull/1455), streamed response CEL [1453](https://github.com/agentgateway/website/pull/1453), model combobox [1460](https://github.com/agentgateway/website/pull/1460), `thinkingLevel` [1451](https://github.com/agentgateway/website/pull/1451), retry `attempts: 0` [1445](https://github.com/agentgateway/website/pull/1445), duplicate MCP targets [1446](https://github.com/agentgateway/website/pull/1446), host casing [1457](https://github.com/agentgateway/website/pull/1457).

## Watch list

For omni-viper on `:latest-dev`; none tagged.

| Repo | PR | Why | Author |
| --- | --- | --- | --- |
| agentgateway | [4356](https://github.com/agentgateway/agentgateway/pull/4356) | **Breaking**: standalone auth defaults `strict`; anonymous traffic needs `mode: optional` | ymesika |
| agentgateway | [4357](https://github.com/agentgateway/agentgateway/pull/4357) | OIDC `azp` + token-type checks on bearer ID tokens | ymesika |
| agentgateway | [4358](https://github.com/agentgateway/agentgateway/pull/4358) | brotli 9.1.1 / CVE-2026-111497 — still blocked; ask about a patch window | makavity |
| agentgateway | [4353](https://github.com/agentgateway/agentgateway/pull/4353) · [4354](https://github.com/agentgateway/agentgateway/pull/4354) | Real `ResolvedRefs` / `status.parents` on delegated child routes — fixes "route looks healthy but isn't" | dongjiang1989 |
| agentgateway | [4350](https://github.com/agentgateway/agentgateway/pull/4350) | Stable `item.id` in native Copilot Responses streams | NotSleeply |
| agentgateway | [4347](https://github.com/agentgateway/agentgateway/pull/4347) | Catalog churn: adds Kimi 3.1 / DeepSeek v4.1, **deletes** Fireworks `gpt-oss-120b`, `inkling`, `nemotron-3-ultra-nvfp4` — don't hardcode model IDs | bot |
| agentgateway | [4345](https://github.com/agentgateway/agentgateway/pull/4345) (+4344, 4343, 4346, 4349) | Yesterday's queue, no merge in window: MCP guardrail bypass · missing `finish_reason` · API-key budget · duplicate models · tokenization | ZYZ666-RGB, deem0n, Quentin-M |
| website | [1484](https://github.com/agentgateway/website/pull/1484) | SDK guide (Go `openai-go/v3`, LiteLLM migration, JWT-as-api-key) — fits the "swap the base URL" pitch | jamesilse-solo |
| website | [1487](https://github.com/agentgateway/website/pull/1487)–[1490](https://github.com/agentgateway/website/pull/1490), [1492](https://github.com/agentgateway/website/pull/1492) | Doc-fix batch + faster pr-tracker builds; [1486](https://github.com/agentgateway/website/pull/1486) still warns "capture or sync failed for `latest main`" — committed UI images incomplete | bot, kristin-kronstain-brown |
