# agentgateway-updates - 2026-11-11

Still **no new tag — v1.7.0 (Oct 27) remains the newest artifact**, but core landed the fix that makes **Anthropic fast-mode billing correct (2x undercount)** and a **Go 1.27.4** bump, website finally documented the **Docker `localhost` tracing trap**, and the catalog picked up **Gemini 4 Carbon** + **Meta Muse Ember** — **6 PRs merged since the 11-10 report (4 core, 2 website)**.

_Window: merged/published `>=2026-11-10` (since the 2026-11-10 report). Active: [agentgateway](https://github.com/agentgateway/agentgateway), [website](https://github.com/agentgateway/website); `benchmarks`/`community` idle. [4362](https://github.com/agentgateway/agentgateway/pull/4362) and [1497](https://github.com/agentgateway/website/pull/1497) merged 11-10 but were already covered yesterday._

## Releases

**None published in the window** — newest artifact is still the Oct 27 cut; everything post-v1.7.0 (including this week's cost-pricing fix) is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) | No 1.8 tag yet; 1.8 release-notes page is still an open PR |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) "Docs for v1.7.0 release" (10-27) | – | Ref docs regenerate continuously from `main`/`v1.7.0` via open bot PRs |
| benchmarks · community | None | – | Idle |

## Highlights

- **Cost dashboards were lying about Anthropic fast mode — fixed** — [4363](https://github.com/agentgateway/agentgateway/pull/4363): Anthropic reports fast mode in `usage.speed`, not `service_tier`, so fast requests were billed at standard rates (stated as a **2x undercount**). Adds `PricingTier::Fast`, surfaces "fast" service tier in buffered/streaming/detect modes, reads `message.usage.service_tier` from streaming `message_start`, imports models.dev `speed: "fast"` modes in `agctl`, and adds fast tiers for claude-fable-5-2, claude-opus-5-5, claude-opus-5, claude-opus-4-8. **Demo-relevant**: any cost claim/CEL assertion you wrote against pre-fix numbers needs re-checking.
- **Catalog churn continues — don't hardcode model IDs** — [4372](https://github.com/agentgateway/agentgateway/pull/4372) added **Gemini 4 Carbon** (1M ctx, multimodal incl. video/audio, $2/$10) to both `gcp.gemini` and `gcp.vertex_ai`, and **Meta Muse Ember 1.0** (1M ctx, tiered $0.10/$0.50 → $0.30/$1.50 above 200k, six reasoning efforts). Same PR repairs the OpenAI pricing scraper: section headings on the pricing page changed to "Realtime & Audio" / "Transcription", so `controller/pkg/cli/catalog/source_openai.go` was retargeted — a silent failure class for `agctl` catalog refresh.
- **Docker + observability was silently broken in the docs** — [1494](https://github.com/agentgateway/website/pull/1494): the OTel Collector, Jaeger and OTLP access-log guides told you to use `localhost:4317`, which is the agentgateway container itself under Docker — spans/logs never arrived. Now documents `host.docker.internal:4317` (+ `--add-host=host.docker.internal:host-gateway` on Linux) or a shared Docker network, as a shared snippet reused across the guides on `main`/`latest`/`1.6.x`/`1.5.x`. The author's test matrix (`localhost` → `Connect: Connection refused`; the two working paths → spans delivered) is a good pre-demo sanity check.
- **Toolchain refresh** — [4371](https://github.com/agentgateway/agentgateway/pull/4371) bumps Go to **1.27.4** (controller/CLI builds).
- **Supply-chain posture** — [4360](https://github.com/agentgateway/agentgateway/pull/4360) (cargo-weekly, 12 crates) is more than routine: **spiffe 0.19.0** is a security-hardening release (Workload API response validation, stricter offline JWT verification incl. exclusive `exp`, JWK algorithm/use + ≥2048-bit RSA checks) with **breaking error-type changes**; **aws-lc-rs 1.19.0** moves to AWS-LC 5.12.0 and lowers the minimum *system* AWS-LC to 4.2.0 (FIPS 4.x system library now usable); **prometheus-client 0.25.2** escapes text-format label values and fixes the duplicate `le="+Inf"` series — worth knowing if you scrape metrics into a dashboard in demos.
- **Docs hygiene** — [1529](https://github.com/agentgateway/website/pull/1529) extracts the cost-dashboard "Group by / Measure" section into `assets/agw-docs/snippets/cost-dashboard-group-by-measure.md` (no wording change) so downstream sites can relabel UI without copying the page.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4372](https://github.com/agentgateway/agentgateway/pull/4372) | update catalog (Gemini 4 Carbon, Muse Ember; OpenAI pricing scraper fix) | howardjohn | 11-10 |
| agentgateway | [4363](https://github.com/agentgateway/agentgateway/pull/4363) | feat(llm): price Anthropic fast mode at fast rates | howardjohn | 11-10 |
| agentgateway | [4371](https://github.com/agentgateway/agentgateway/pull/4371) | bump go to 1.27.4 | jenshu | 11-10 |
| agentgateway | [4360](https://github.com/agentgateway/agentgateway/pull/4360) | build(deps): bump the cargo-weekly group with 12 updates | dependabot[bot] | 11-10 |
| website | [1494](https://github.com/agentgateway/website/pull/1494) | Docker container note (collector host for OTel/Jaeger/access logs) | kristin-kronstain-brown | 11-10 |
| website | [1529](https://github.com/agentgateway/website/pull/1529) | Move cost dashboard Group by/Measure text into a snippet | kristin-kronstain-brown | 11-10 |

All 6 in-window PRs listed. [1494](https://github.com/agentgateway/website/pull/1494) and [1497](https://github.com/agentgateway/website/pull/1497)/[4362](https://github.com/agentgateway/agentgateway/pull/4362) straddled yesterday's publish time — see the [full merged list](https://github.com/pulls?q=is%3Apr+is%3Amerged+org%3Aagentgateway+merged%3A%3E%3D2026-11-10). No merges so far on 11-11; all activity is in open PRs below.

## Watch list

Test on omni-viper with `:latest-dev`; nothing tagged.

| Repo | PR | Why | Author |
| --- | --- | --- | --- |
| agentgateway | [4356](https://github.com/agentgateway/agentgateway/pull/4356) | **Breaking, still open**: standalone `jwtAuth`/`apiKey`/`basicAuth` default `strict` — anonymous traffic needs `mode: optional` | ymesika |
| agentgateway | [4357](https://github.com/agentgateway/agentgateway/pull/4357) | OIDC `azp` verification on bearer ID tokens (Okta/Keycloak multi-audience) | ymesika |
| agentgateway | [4378](https://github.com/agentgateway/agentgateway/pull/4378) | Accept HMAC-signed (`HS*`) ID tokens verified with the client secret — new IdP interop path | wzlee |
| agentgateway | [4379](https://github.com/agentgateway/agentgateway/pull/4379) | Warn on FIPS-rejected frontend TLS policy options — pairs with the aws-lc-rs FIPS bump above | ymesika |
| agentgateway | [4365](https://github.com/agentgateway/agentgateway/pull/4365) · [4364](https://github.com/agentgateway/agentgateway/pull/4364) | CEL `recurse()` for nested fields + UI editing of CEL MCP guardrails — new guardrail demo beats | ymesika, stevenctl |
| agentgateway | [4369](https://github.com/agentgateway/agentgateway/pull/4369) | Cap `privateKeyJwt` client-assertion lifetime at 300s — replay hardening | schuhu |
| agentgateway | [4353](https://github.com/agentgateway/agentgateway/pull/4353) · [4354](https://github.com/agentgateway/agentgateway/pull/4354) | Real `ResolvedRefs` / `status.parents` on delegated child routes — Gateway API conformance surface | dongjiang1989 |
| agentgateway | [4368](https://github.com/agentgateway/agentgateway/pull/4368) · [4370](https://github.com/agentgateway/agentgateway/pull/4370) | Postgres DDL in one transaction with an advisory lock; Helm renders `scrapeNativeHistograms` only when enabled | conradhanson, AlecGoncharow |
| website | [1519](https://github.com/agentgateway/website/pull/1519) | Consolidated **1.8 release notes** page from the PR tracker — usually lands just before the tag | github-actions[bot] |
| website | [1518](https://github.com/agentgateway/website/pull/1518) | Pending 1.8 docs: **nested virtual models** (+2 related) — new feature to rehearse | github-actions[bot] |
| website | [1532](https://github.com/agentgateway/website/pull/1532) · [1484](https://github.com/agentgateway/website/pull/1484) · [1478](https://github.com/agentgateway/website/pull/1478) | More backend token-exchange IdPs; Go SDK + LiteLLM migration guide; Elastic observability guides | artberger, jamesilse-solo, haowu1234 |
