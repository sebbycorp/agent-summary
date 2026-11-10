# agentgateway-updates - 2026-11-10

Quiet on the release front (**no new tag — still v1.7.0 from Oct 27**), but the brotli **CVE-2026-111497** fix finally merged, the model catalog gained **Claude Fable 5.2**, and website did a same-day cleanup pass on yesterday's breaking-change docs — **13 PRs merged (2 core, 11 website)**.

_Window: merged/published `>=2026-11-09` (since the 2026-11-09 report). Active: [agentgateway](https://github.com/agentgateway/agentgateway), [website](https://github.com/agentgateway/website); `benchmarks`/`community` idle._

## Releases

**None published in the window** — newest artifact is still the Oct 27 cut; post-v1.7.0 work is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) | No 1.8 tag; docs already carry a 1.8 section |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | – | Ref docs + 1.8 notes live in the `main` trees ahead of the tag |
| benchmarks · community | None | – | Idle |

## Highlights

- **Security fix landed** — [4358](https://github.com/agentgateway/agentgateway/pull/4358) bumps `brotli` to **9.1.1** for **CVE-2026-111497** (closed upstream in 9.1.0). Yesterday it was blocked on review; merged 11-09 13:23, so `:latest-dev` nightlies are clean and the next tag will carry it. Shipped v1.7.0 binaries still bundle the vulnerable version — relevant for supply-chain questions.
- **Catalog churn = don't hardcode model IDs** — [4362](https://github.com/agentgateway/agentgateway/pull/4362) and [4347](https://github.com/agentgateway/agentgateway/pull/4347) added **Claude Fable 5.2** (Anthropic / Bedrock us+global / Azure / Vertex, 1M ctx, $8/$40, adaptive thinking), **Kimi K3.1 Pro/Highspeed** (Bedrock, Fireworks, DeepInfra) and Fireworks **DeepSeek V4.1 Flash ultrafast**; they **deleted** Fireworks `gpt-oss-120b`, `inkling`, `nemotron-3-ultra-nvfp4`, `nemotron-lightning-3p5-30b-a3b`, and re-scored the Gemini `deep-research` previews 131k → 1M context. Re-check decks naming those IDs.
- **Yesterday's breaking docs corrected the same day** — [1500](https://github.com/agentgateway/website/pull/1500) reworks passthrough precedence + the Bedrock passthrough example (follow-up to [1441](https://github.com/agentgateway/website/pull/1441)); [1499](https://github.com/agentgateway/website/pull/1499) reworks the backend-TLS fail-closed page (follow-up to [1459](https://github.com/agentgateway/website/pull/1459)). Behaviour unchanged (bad backend TLS → `503`, never plaintext; `transformation.model` rewrites provider URL paths) — the published wording moved.
- **New provider pages to demo** — [1496](https://github.com/agentgateway/website/pull/1496) gives **Nebius Token Factory** and **Perplexity** dedicated pages in both modes, so the Kubernetes provider list matches standalone.
- **MCP behaviour note shipped** — [1435](https://github.com/agentgateway/website/pull/1435): an **expired JWT-shaped OAuth subject token now gets `401 invalid_token` locally**, before token exchange calls the token endpoint, and MCP `FailOpen` no longer hides bearer-token rejections. Good for demoing refresh/retry.
- **Two demo-breaking doc bugs fixed** — [1497](https://github.com/agentgateway/website/pull/1497): the Claude Desktop K8s guide's buffer policy targeted an HTTPRoute, which the API server rejects (`frontend` can only target a Gateway); it now targets the `agentgateway-proxy` Gateway and `anthropic-beta` moves to its own HTTPRoute policy. [1495](https://github.com/agentgateway/website/pull/1495): two `AgentgatewayBackend` examples set only `spec.policies` and were rejected (`exactly one of [ai static …] must be set`); they now set a `static` backend. Both caught by a new dry-run-against-API-server doc test.
- **Docs pipeline** — [1492](https://github.com/agentgateway/website/pull/1492) + [1498](https://github.com/agentgateway/website/pull/1498): pr-tracker previews build only changed version trees (plus `main` on shared-asset changes) and skip the search index. [1486](https://github.com/agentgateway/website/pull/1486) still merged with "capture or sync failed for `latest main`" — committed UI screenshots incomplete.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4358](https://github.com/agentgateway/agentgateway/pull/4358) | fix(compression): bump `brotli` to 9.1.1 (CVE-2026-111497) | makavity | 11-09 |
| agentgateway | [4362](https://github.com/agentgateway/agentgateway/pull/4362) | Update model catalog (Claude Fable 5.2, Kimi K3.1) | bot | 11-10 |
| agentgateway | [4347](https://github.com/agentgateway/agentgateway/pull/4347) | Update model catalog (drops 4 Fireworks models) | bot | 11-09 |
| website | [1497](https://github.com/agentgateway/website/pull/1497) | Claude Desktop: buffer policy HTTPRoute → Gateway | kristin-kronstain-brown | 11-10 |
| website | [1522](https://github.com/agentgateway/website/pull/1522) | Fix stale comment on removed controller setting | kristin-kronstain-brown | 11-09 |
| website | [1500](https://github.com/agentgateway/website/pull/1500) | Rework passthrough precedence + Bedrock example | artberger | 11-09 |
| website | [1499](https://github.com/agentgateway/website/pull/1499) | Rework backend TLS fail-closed docs | artberger | 11-09 |
| website | [1496](https://github.com/agentgateway/website/pull/1496) | Dedicated Nebius and Perplexity provider pages | artberger | 11-09 |
| website | [1475](https://github.com/agentgateway/website/pull/1475) | DeepKeep adapter links → integrations monorepo | gilarel | 11-09 |
| website | [1435](https://github.com/agentgateway/website/pull/1435) | Draft docs: MCP `401` for expired OAuth subject tokens | bot | 11-09 |
| website | [1495](https://github.com/agentgateway/website/pull/1495) | Add missing `static` backend-type example | kristin-kronstain-brown | 11-09 |
| website | [1498](https://github.com/agentgateway/website/pull/1498) | Build shared-file fallback trees in pr-tracker previews | kristin-kronstain-brown | 11-09 |
| website | [1492](https://github.com/agentgateway/website/pull/1492) | Shorten build times for pr-tracker branches | kristin-kronstain-brown | 11-09 |

All 13 in-window PRs listed; yesterday's morning 11-09 batch ([1485](https://github.com/agentgateway/website/pull/1485), [1486](https://github.com/agentgateway/website/pull/1486), [1493](https://github.com/agentgateway/website/pull/1493) + the 1.8 draft-docs set) was in the 2026-11-09 report.

## Watch list

For omni-viper on `:latest-dev`; none tagged.

| Repo | PR | Why | Author |
| --- | --- | --- | --- |
| agentgateway | [4356](https://github.com/agentgateway/agentgateway/pull/4356) | **Breaking, open/blocked**: standalone `jwtAuth`/`apiKey`/`basicAuth` default `strict`; anonymous traffic needs `mode: optional` (incl. `oidc` + `credentials: permissive`) | ymesika |
| agentgateway | [4357](https://github.com/agentgateway/agentgateway/pull/4357) | OIDC `azp` + token-type checks on bearer ID tokens — Okta/Keycloak multi-audience | ymesika |
| agentgateway | [4363](https://github.com/agentgateway/agentgateway/pull/4363) | Anthropic **fast mode** priced at fast rates — changes cost dashboards/CEL claims | howardjohn |
| agentgateway | [4364](https://github.com/agentgateway/agentgateway/pull/4364) · [4365](https://github.com/agentgateway/agentgateway/pull/4365) | UI editing of CEL MCP guardrails; new `actors()` CEL helper for act chains — new demo beats | stevenctl, ymesika |
| website | [1519](https://github.com/agentgateway/website/pull/1519) | Consolidated 1.8 release-notes page — usually precedes the tag | bot |
| website | [1518](https://github.com/agentgateway/website/pull/1518) · [1516](https://github.com/agentgateway/website/pull/1516) · [1515](https://github.com/agentgateway/website/pull/1515) | Pending 1.8 docs: **nested virtual models**, UI under any path prefix, HTTP path normalization | bot |
| website | [1521](https://github.com/agentgateway/website/pull/1521) · [1523](https://github.com/agentgateway/website/pull/1523) · [1525](https://github.com/agentgateway/website/pull/1525) | Compression policy guides, OSS assistant-skills guides, K8s↔standalone cross-links | kristin-kronstain-brown, jamesilse-solo, uc4w6c |
