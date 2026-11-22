# agentgateway-updates - 2026-11-22

**Quiet weekend: zero merges and zero commits in `agentgateway`, `website`, `benchmarks` and `community` since 2026-11-20T22:04Z - `main` is still at [#4501](https://github.com/agentgateway/agentgateway/commit/888badbbe91f2a57ec8e7bf5a5ef3acda34b1fe5). No releases. All the signal is forward-looking: 8 new open PRs, headlined by request-log retention ([4512](https://github.com/agentgateway/agentgateway/pull/4512)), JWT key `maxAge` ([4142](https://github.com/agentgateway/agentgateway/pull/4142)) and the Substrate router/egress rewrite ([4505](https://github.com/agentgateway/agentgateway/pull/4505)); 1.8 doc drafts now at six.**

_Window: since the 2026-11-21 digest (closed at 2026-11-20T22:04Z). Nothing new merged, so yesterday's breaking trio - #4300 AWS native endpoints, #4430 JWT-email user, #4205 azureContentSafety - is still the newest behaviour change on `:latest-dev`._

## Releases

**None published** in window. Cut remains Oct 27; everything since is nightly / `:latest-dev`.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | - |

Still no `v1.8.0-alpha.1` tag - the release is being staged in docs, not in tags.

## Highlights

- **Request-log retention is coming** ([4512](https://github.com/agentgateway/agentgateway/pull/4512), dongjiang1989, opened 11-22, fixes #4166, +603/-110, 22 files). New optional `config.logging.databaseRetention` prunes `request_logs` / `request_log_payloads` at startup then hourly: 10,000-row chunks, 100 ms between chunks, 5-minute wall-clock budget per round, abort after 3 failed chunks, payloads via `ON DELETE CASCADE`. Unset = keep forever (today's behaviour). **Demo-relevant**: long-running omni-viper instances have an unbounded DB right now - and `90d` is *not* a valid value, use `2160h`.
- **JWT key trust age, still unmerged after a month** ([4142](https://github.com/agentgateway/agentgateway/pull/4142), benjamin-di, draft, updated 11-22). Adds `jwks: {url, maxAge}` for `jwtAuth` and `mcpAuthentication`; aged-out keys return **503 with no `WWW-Authenticate`** - deliberately not 401, since the caller's token isn't at fault. Also quietly changes the default for *all* remote JWKS: a non-2xx is now a failed fetch that keeps cached keys, instead of the error body replacing the key document. Security-review talking point; state is `blocked`.
- **Fail-fast for MCP OpenAPI targets** ([4511](https://github.com/agentgateway/agentgateway/pull/4511), jamesilse, open). An operation with no `operationId`, or with a cookie param, loaded clean and only blew up at first `initialize`; now rejected at config load with the target name. Kills the "mysteriously empty tool list mid-demo" class of bug.
- **Substrate being realigned** ([4505](https://github.com/agentgateway/agentgateway/pull/4505), keithmattix, draft, +793/-111, 11 files). Router outcome handling matched to atenet resume semantics, generic SSRF protection on dynamically resolved backends (denied vs unavailable distinguished), WebSocket egress upgrades, credential injection by longest safe path prefix, RFC 8693 egress token exchange over the shared OAuth transport. Unit-tested (36 substrate + 152 oauth) but **live e2e lanes not run** - don't put Substrate egress in a customer-facing flow yet.
- **1.8 docs are now six drafts deep** (website, artberger): [1576](https://github.com/agentgateway/website/pull/1576) guardrails · [1577](https://github.com/agentgateway/website/pull/1577) Gateway API **v1.7 move** · [1578](https://github.com/agentgateway/website/pull/1578) LLM providers/APIs/catalog · [1579](https://github.com/agentgateway/website/pull/1579) data-plane metrics generator · [1580](https://github.com/agentgateway/website/pull/1580) observability + UI text · [1581](https://github.com/agentgateway/website/pull/1581) MCP backends, sessions, authz, clients - plus the bot's doc sync for the #4300 AWS breaking default ([1582](https://github.com/agentgateway/website/pull/1582)) and regenerated reference docs ([1575](https://github.com/agentgateway/website/pull/1575)). 1577 is the one implying CRD churn.
- **Housekeeping, low risk**: [4509](https://github.com/agentgateway/agentgateway/pull/4509) model catalog refresh, [4510](https://github.com/agentgateway/agentgateway/pull/4510) duplicate-model-names warning now says how to fix it, [4506](https://github.com/agentgateway/agentgateway/pull/4506) honour configured Copilot client headers, [4507](https://github.com/agentgateway/agentgateway/pull/4507) LLMTR as an OpenAI-compatible preset, [4504](https://github.com/agentgateway/agentgateway/pull/4504) **(draft)** preserve Responses images / 400 on file-ID-only input. No CVEs against org-owned code in window; no new security merges.

## Merged PRs by repo

**None in window.** No merged PR and no direct commit in any of the four repos between 2026-11-21T00:00Z and 2026-11-22T13:00Z. Last merge anywhere was [#4501](https://github.com/agentgateway/agentgateway/pull/4501) at 2026-11-20T22:04Z (already covered yesterday). `benchmarks` and `community` stay idle (last activity 10-28 / 11-13).

## Watch list

| Item | Why |
| --- | --- |
| website [1577](https://github.com/agentgateway/website/pull/1577) | Gateway API **v1.7 move** = CRD churn. Regenerate + smoke manifests on omni-viper before the 1.8 RC. |
| [4512](https://github.com/agentgateway/agentgateway/pull/4512) | First retention knob for the request-log DB (`mergeable_state: dirty`, needs rebase). Test `2160h`, verify prune can't starve UI queries. |
| [4142](https://github.com/agentgateway/agentgateway/pull/4142) | Alters *default* remote-JWKS error handling. If merged, re-run the IdP-outage auth demo. |
| [4505](https://github.com/agentgateway/agentgateway/pull/4505) · [4490](https://github.com/agentgateway/agentgateway/pull/4490) · [4473](https://github.com/agentgateway/agentgateway/pull/4473) | Substrate router/egress rewrite (draft, no live e2e); MCP Skills multiplexing; multiple MCP endpoints in standalone. |

Nothing new to re-test on `:latest-dev` today; yesterday's list (#4300 Bedrock IAM role, #4430 user attribution split, #4205 azureContentSafety CRD guards, #4497 fallback during rollout, #4483 OTel `gen_ai.response.id`, #4501 MCP stateful default) is unchanged and still outstanding.
