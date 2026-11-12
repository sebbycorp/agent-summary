# agentgateway-updates - 2026-11-12

**Quiet day upstream: no new tag and zero merged PRs in the core repo since 11-10** — all four in-window merges are website docs (Go/LiteLLM SDK guide, Okta + Keycloak token exchange, authn reorg, schema shortcode), while the real story is a brand-new **open 68k-line LLM-IR POC** in core that rewrites protocol conversion behind an env flag.

_Window: merged/published `>=2026-11-11` (since the 2026-11-11 report). Active: [website](https://github.com/agentgateway/website); [agentgateway](https://github.com/agentgateway/agentgateway) core idle (no commits since 11-10); `benchmarks`/`community` idle.

## Releases

**None published in the window.** Newest artifact is still the Oct 27 cut; everything after v1.7.0 — including last week's Anthropic fast-mode pricing fix and the Go 1.27.4 bump — is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) | No 1.8 tag; release-notes page still open as [1519](https://github.com/agentgateway/website/pull/1519) |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) "Docs for v1.7.0 release" (10-27) | – | Ref docs regenerate continuously from `main`/`v1.7.0` via bot PRs ([1526](https://github.com/agentgateway/website/pull/1526), [1527](https://github.com/agentgateway/website/pull/1527), [1534](https://github.com/agentgateway/website/pull/1534)) |
| benchmarks · community | None | – | Idle |

## Highlights

- **The OpenAI SDK page was wrong about `base_url` — now fixed** — [1484](https://github.com/agentgateway/website/pull/1484) (merged 11-11): the Kubernetes page told you to leave `/v1` off `base_url`; the SDK appends only `/chat/completions`, so the page's own config returned **404** on both the default route and a `/openai` custom route. Now it says to end the base URL with `/v1`. Same PR adds a **Go** example (`openai-go/v3`), a "call more than one provider" section using the `openai/*` / `anthropic/*` prefix pattern, a **LiteLLM SDK migration** section (same `provider/model` strings), and JWT-as-API-key auth. Also a useful demo gotcha: once a listener serves `AgentgatewayModel`, an unprefixed model name that an HTTPRoute used to own returns *model not found*.
- **Backend token-exchange IdP list grew to Okta + Keycloak** — [1532](https://github.com/agentgateway/website/pull/1532) (merged 11-12): joins the existing Google Cloud IdP guide. **Demo caveat:** the core-side Keycloak fix for the RFC 9207 issuer mismatch ([3637](https://github.com/agentgateway/agentgateway/pull/3637)) is **still open since 09-23**, so test Keycloak token exchange on `:latest-dev` before putting it in a customer deck.
- **Authn docs restructured** — [1533](https://github.com/agentgateway/website/pull/1533) (merged 11-12): security/authentication topics reorganized ("Based on feedback"), plus a schema **shortcode** in [1531](https://github.com/agentgateway/website/pull/1531) so config reference tables render from the generated schema instead of being copy-pasted. Expect doc URLs to move — re-check any deep links in your demo scripts.
- **Biggest thing to watch in core: a shared LLM intermediate representation** — [4383](https://github.com/agentgateway/agentgateway/pull/4383) opened 11-12, **+67,779 / -114 lines across 383 files**, a new `agent-llm-ir` crate converting Messages, Responses, Chat Completions and Gemini `generateContent` through one IR. Explicitly **experimental and off by default** (`AGENTGATEWAY_LLM_IR_ENABLED=true`). Every format-conversion bug you have worked around (reasoning carry-over, citations, tool-call fragments) would be re-based on this — worth following before 1.8.
- **New core fix proposal in flight** — [4381](https://github.com/agentgateway/agentgateway/pull/4381) `fix(extproc): preserve END_STREAM for bodyless responses` (AlecGoncharow, open 11-11): the class of ext_proc bug that hangs or double-terminates responses; useful to know if a demo uses an external-processing policy.
- **Core has been idle since 11-10** — `list_commits` for the core repo since 2026-11-11 returns nothing; last week's items (fast-mode pricing 4363, catalog 4372, Go 1.27.4 4371, cargo-weekly 4360) are still the newest core work, and all remain untagged.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| website | [1484](https://github.com/agentgateway/website/pull/1484) | docs: SDK guide with Go examples and LiteLLM SDK migration (incl. `base_url` `/v1` fix) | jamesilse-solo | 11-11 |
| website | [1531](https://github.com/agentgateway/website/pull/1531) | [docs] schema shortcode | artberger | 11-11 |
| website | [1532](https://github.com/agentgateway/website/pull/1532) | [docs] add more backend token exchange IdPs (Okta, Keycloak) | artberger | 11-12 |
| website | [1533](https://github.com/agentgateway/website/pull/1533) | authn reorg | artberger | 11-12 |

All 4 in-window merges listed. **agentgateway (core): none in window** — no merged PRs and no commits after 11-10. Full list: [merged PRs `>=2026-11-11`](https://github.com/pulls?q=is%3Apr+is%3Amerged+org%3Aagentgateway+merged%3A%3E%3D2026-11-11).

## Watch list

Nothing new to test on omni-viper beyond `:latest-dev` — no tag in window.

| Repo | PR | Why | Author |
| --- | --- | --- | --- |
| agentgateway | [4383](https://github.com/agentgateway/agentgateway/pull/4383) | **LLM-IR POC** (+67.8k lines, 383 files), env-gated `AGENTGATEWAY_LLM_IR_ENABLED=true` — future home of every format conversion | markuskobler |
| agentgateway | [4356](https://github.com/agentgateway/agentgateway/pull/4356) | **Breaking, still open**: standalone `jwtAuth`/`apiKey`/`basicAuth` default `strict` — anonymous traffic needs `mode: optional` | ymesika |
| agentgateway | [4378](https://github.com/agentgateway/agentgateway/pull/4378) | Accept HMAC-signed (`HS*`) ID tokens verified with the client secret — new IdP interop path | wzlee |
| agentgateway | [4357](https://github.com/agentgateway/agentgateway/pull/4357) | OIDC `azp` verification on bearer ID tokens (Okta/Keycloak multi-audience) — pairs with the new IdP docs | ymesika |
| agentgateway | [3637](https://github.com/agentgateway/agentgateway/pull/3637) | Keycloak RFC 9207 issuer mismatch fix, open since 09-23 — blocks clean Keycloak token-exchange demos | Tazmainiandevil |
| agentgateway | [4381](https://github.com/agentgateway/agentgateway/pull/4381) | ext_proc: preserve `END_STREAM` for bodyless responses | AlecGoncharow |
| agentgateway | [4365](https://github.com/agentgateway/agentgateway/pull/4365) · [4364](https://github.com/agentgateway/agentgateway/pull/4364) | CEL `recurse()` for nested fields + UI editing of CEL MCP guardrails — good extra guardrail beats | ymesika, stevenctl |
| agentgateway | [4341](https://github.com/agentgateway/agentgateway/pull/4341) | Webhook guardrails: writeback without clobbering non-text content | stevenctl |
| agentgateway | [4368](https://github.com/agentgateway/agentgateway/pull/4368) · [4370](https://github.com/agentgateway/agentgateway/pull/4370) · [4369](https://github.com/agentgateway/agentgateway/pull/4369) | Postgres DDL in one txn + advisory lock; Helm `scrapeNativeHistograms` gating; 300s cap on `privateKeyJwt` assertions | conradhanson, AlecGoncharow, schuhu |
| website | [1519](https://github.com/agentgateway/website/pull/1519) · [1518](https://github.com/agentgateway/website/pull/1518) | Consolidated **1.8 release notes** page and pending 1.8 docs (**nested virtual models**) — usually land just before the tag | github-actions[bot] |
| website | [1478](https://github.com/agentgateway/website/pull/1478) · [1530](https://github.com/agentgateway/website/pull/1530) | Elastic observability guides; relative-alias fix for generated doc links | haowu1234, kristin-kronstain-brown |
