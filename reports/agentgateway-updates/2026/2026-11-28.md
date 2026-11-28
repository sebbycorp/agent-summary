# agentgateway-updates - 2026-11-28

**Third straight day with zero merges and zero commits to `main` — HEAD is still `134cbf02` (#4556, 11-24T23:22Z), so the demo build remains the 11-24 nightly. No releases (v1.7.0, Oct 27 still current). The action is all in the queue: 4 brand-new PRs since yesterday, every one LLM-billing or auth — Realtime/GPT-Live usage accounting (#4589), LiteLLM Vertex credential import (#4590), per-model tokenizers (#4586), automated docs fixes (#1625).**

_Window: since the 2026-11-27 digest. `is:pr is:merged merged:>=2026-11-27` → [0 results](https://github.com/search?q=org%3Aagentgateway+is%3Apr+is%3Amerged+merged%3A%3E%3D2026-11-27&type=pullrequests); `list_commits since=2026-11-27T00:00Z` → empty on `agentgateway`, `website`, `benchmarks`._

## Releases

**None published in window** — the cut is still Oct 27; anything newer is nightly / `:latest-dev`.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27T17:18Z) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26T19:06Z) |
| website | [Docs for v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27T23:37Z) | – |

Still no `v1.8.0-alpha.1`. Rolling release-note PR [website#1586](https://github.com/agentgateway/website/pull/1586) is open and `behind`, last touched 11-23T15:11Z, still warning that the 11 reference URLs it adds were never seen in the docs — including the breaking Bedrock native-endpoint note (#4300), file-backed mTLS (#3872) and MCP Skills (#4214).

## Highlights

- **Realtime / "GPT-Live" audio was invisible to billing** ([#4589](https://github.com/agentgateway/agentgateway/pull/4589), PrashantRaghuwanshi90, 11-28T05:18Z, against #4588). Adds `/v1/live/sessions` with `session.start`/`session.started`, forwards audio/control frames through the existing WebSocket proxy, aggregates usage across multi-turn Realtime responses, and takes duration from the cumulative snapshot plus the final `session.closed`, de-duplicating response usage by response id. Makes the voice/audio-agent demo cost-attributable; issues 4–5 deferred, so partial.
- **LiteLLM → agentgateway migrations could silently lose the Vertex credential** ([#4590](https://github.com/agentgateway/agentgateway/pull/4590), KrishnaRMaddikara, 11-28T07:29Z, part of #2607). The importer dropped the configured path from `vertex_credentials` / `vertex_ai_credentials`, so a working LiteLLM config returned `403 PERMISSION_DENIED` post-migration by falling back to pod identity. Now maps a supported path to `auth.gcp.credential.file`, skips ambiguous models with a reason, still allows ADC when none is set. GKE evidence: patched 200, base 403, authorized-but-different SA 200 — good migration-war story.
- **Token estimates were wrong for Chinese-model traffic** ([#4586](https://github.com/agentgateway/agentgateway/pull/4586), xixi-box, 11-27T17:48Z, fixes #4585). `o200k` was used for *all* local input estimates — a Chinese Chat Completions call to DeepSeek V4 counted 28 tokens where the DeepSeek vocabulary gives 23. Selects bundled tiktoken vocabularies for DeepSeek, GLM, Kimi, Qwen, MiniMax; others keep `o200k` with the Claude adjustments; provider-reported usage still wins. UI cost/rate-limit attribution will shift when this lands.
- **Docs corrections touching demo-critical semantics** ([website#1625](https://github.com/agentgateway/website/pull/1625), github-actions[bot], 11-28T10:22Z): JWT `Optional` vs `Permissive` + default Basic-auth realm; backend-TLS `hostname` overriding SNI; route **hostname matched before path/header/method/query**; buffered response webhooks see only message text while streamed ones see reasoning; Bedrock guardrails default scope now includes tool outputs (inputs stay opt-in). CI is held `action_required` until someone presses **Approve and run** — no failures means untested, not green.
- **Carried forward, still unreleased**: yesterday's queue is unchanged — [MCP discovery deadlines #4576](https://github.com/agentgateway/agentgateway/pull/4576), [OIDC `forwardAccessToken` #4575](https://github.com/agentgateway/agentgateway/pull/4575), [AgentCore MCP by ARN #4579](https://github.com/agentgateway/agentgateway/pull/4579), [`probe` #4581](https://github.com/agentgateway/agentgateway/pull/4581), [#4578](https://github.com/agentgateway/agentgateway/pull/4578), [Vertex pricing #4582](https://github.com/agentgateway/agentgateway/pull/4582), [CEL-error counter #4584](https://github.com/agentgateway/agentgateway/pull/4584) — all open, last touched 11-26/11-27.
- **No new CVEs or advisories.** Auth hardening parked: [#4545 `jwks.maxAge`](https://github.com/agentgateway/agentgateway/pull/4545) (expired keys → 503), [#4394 JWKS failure isolation](https://github.com/agentgateway/agentgateway/pull/4394) — both change IdP-outage fail-open/closed behaviour; re-run the JWT demo when either merges.
- **Nothing to re-test on omni-viper**: `website` last commit [`59af5f8b`](https://github.com/agentgateway/website/commit/59af5f8b6e9eb6175c252713f5d123711482c8bd) (11-24, merge of #1578, the 1.8 LLM draft); `community` [`0bd822d6`](https://github.com/agentgateway/community/commit/0bd822d6ee2cc52c46fe6e13dfbd94f7c651074c) (08-25); `benchmarks` quiet since 11-20.

## Merged PRs by repo

**None in window** — nothing merged in `agentgateway`, `website`, `community` or `benchmarks` since 2026-11-27. Newly **opened** PRs instead (all unmerged):

| Repo | PR | Title | Author | Opened |
| --- | --- | --- | --- | --- |
| agentgateway | [#4590](https://github.com/agentgateway/agentgateway/pull/4590) | Map LiteLLM Vertex AI credentials to GCP auth | KrishnaRMaddikara | 11-28T07:29Z |
| agentgateway | [#4589](https://github.com/agentgateway/agentgateway/pull/4589) | fix: realtime usage accounting across session turns + GPT-Live websocket endpoint | PrashantRaghuwanshi90 | 11-28T05:18Z |
| agentgateway | [#4586](https://github.com/agentgateway/agentgateway/pull/4586) | fix(llm): use model vocabularies for token estimates | xixi-box | 11-27T17:48Z |
| website | [#1625](https://github.com/agentgateway/website/pull/1625) | [Automated] Docs fixes: basic-auth, jwt-setup, guardrails overview, and 4 more | github-actions[bot] | 11-28T10:22Z |

Also *updated* (not opened): [#4563 model catalog](https://github.com/agentgateway/agentgateway/pull/4563), [website#1621 reference docs](https://github.com/agentgateway/website/pull/1621), [#3930 Vertex Gemini system messages](https://github.com/agentgateway/agentgateway/pull/3930), [#4349 streaming token reassembly](https://github.com/agentgateway/agentgateway/pull/4349).

## Watch list

| Item | Why |
| --- | --- |
| [#4589](https://github.com/agentgateway/agentgateway/pull/4589) | Realtime/GPT-Live `/v1/live/sessions` + usage accounting — partial; test audio cost attribution once merged. |
| [#4590](https://github.com/agentgateway/agentgateway/pull/4590) | `auth.gcp.credential.file` import — re-run a LiteLLM Vertex migration on GKE/ADC before promising a clean migration. |
| [#4586](https://github.com/agentgateway/agentgateway/pull/4586) · [#4582](https://github.com/agentgateway/agentgateway/pull/4582) · [#4563](https://github.com/agentgateway/agentgateway/pull/4563) | Tokenizers + Vertex regional pricing + nightly catalog — expected vs. actual cost moves together. |
| [#4576](https://github.com/agentgateway/agentgateway/pull/4576) | New `discoveryTimeout` MCP field (failOpen skip vs failClosed 504) — config-schema change; test with a hung server. |
| [#4579](https://github.com/agentgateway/agentgateway/pull/4579) | `static.agentCore` on `AgentgatewayBackend` — **CRD change**; regenerate CRDs, re-check AgentCore MCP (SigV4, Streamable HTTP). |
| [#4575](https://github.com/agentgateway/agentgateway/pull/4575) · [#2939](https://github.com/agentgateway/agentgateway/pull/2939) | `forwardAccessToken` changes cookie contents/session lifetime; session-store refactor open — regression-test login + exchange. |
| [#4581](https://github.com/agentgateway/agentgateway/pull/4581) · [#4584](https://github.com/agentgateway/agentgateway/pull/4584) | `agentgateway probe` (update lab readiness manifests); CEL-error counter exposes silent Deny allows. |
| [#4545](https://github.com/agentgateway/agentgateway/pull/4545) · [#4394](https://github.com/agentgateway/agentgateway/pull/4394) | `jwks.maxAge` fail-closed, JWKS failure isolation — JWT/auth demo behaviour shifts on IdP outage. |
| [website#1586](https://github.com/agentgateway/website/pull/1586) · [#1625](https://github.com/agentgateway/website/pull/1625) | #1586 gates any `v1.8.0-alpha.1` (`behind`, unverified links, breaking Bedrock note); #1625 docs fixes untested until CI approved. |
