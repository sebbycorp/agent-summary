# kagent-updates - 2026-11-12

**kagent v1.0.1 shipped overnight — the first patch since GA is not quiet: the install/onboarding feature was backported to 1.0.x, so chart defaults move to `gpt-5-mini` + `apiFormat: responses`, gRPC reflection now requires auth, and `kagent install` creates default Harnesses. doc2vec v2.17.5 closed 4 CVEs. 13 PRs merged org-wide (11 new); substrate: zero commits; tools finally moved.**

Window: 2026-11-11 → 11-12 (since `latest.md` @ 11-11); 11 new since yesterday.

## Releases

- **[kagent v1.0.1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.1)** — 11-12; three commits over v1.0.0, all behaviour-changing backports: [#3611](https://github.com/kagent-dev/kagent/pull/3611) telemetry honors runtime content-capture settings, [#3612](https://github.com/kagent-dev/kagent/pull/3612) session identity preserved on outbound MCP requests, [#3629](https://github.com/kagent-dev/kagent/pull/3629) install / first-agent onboarding.
- **[doc2vec v2.17.5](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.5)** — 11-12; express 5.2.1→5.3.0 (`qs` ^6.16.0 / `proxy-addr` ^2.0.8 fix CVE-2026-2391, -82417, -82562, -90711), pg 8.23.1, turndown 7.2.4 ([#143](https://github.com/kagent-dev/doc2vec/pull/143)).
- **Unchanged:** [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) (11-08), [tools v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1) (11-04; fixes untagged). No pre-releases; kagent and substrate still list `rc4` as newest RC.

## Highlights

- **v1.0.1 is a chart-default change, not a no-op patch** ([#3629](https://github.com/kagent-dev/kagent/pull/3629)). Defaults become OpenAI `gpt-5-mini` + `apiFormat: responses` (was `gpt-4.1-mini`), Azure OpenAI `gpt-5-mini` / `2024-10-21`; `codex-default` needs `responses` to work at all. Pin old values via `providers.openAI`/`azureOpenAI` `model`+`config`.
- **Same PR, two customer-visible breaks:** gRPC reflection left `AccessPublic`, so anonymous `grpcurl` dies once auth is on (update runbooks); `kagent install` now creates `kagent-default`/`codex-default`/`claude-default` Harnesses — a same-named Harness of yours can break Helm on ownership.
- **Postgres brownouts stop looking like controller bugs:** failed logins retry with backoff up to `database.pool.connectRetryTimeout` (default `90s`; `0s` = old behaviour) and return `Unavailable "database unavailable"` with the cause logged; auth rejections (`28P01`/`28000`/`42501`) still fail fast as `Internal`. Blast radius: every store call; validated on live Aurora Serverless ([#3550](https://github.com/kagent-dev/kagent/pull/3550)).
- **Anthropic parallel tool calls were non-deterministic:** `runAnthropicStreaming` ranged a `map[int]`, so Go's random map order reordered function calls in the response *and* in history back to the model; indices now sorted, as in the OpenAI adapter ([#3568](https://github.com/kagent-dev/kagent/pull/3568)).
- **Breaking for one config:** `azureAdToken` removed from `ModelConfig` — the runtime never consumed `AZURE_AD_TOKEN` and it bypassed v1 gateway credential injection ([#3651](https://github.com/kagent-dev/kagent/pull/3651)).
- **tools: an MCP server that was wholly unavailable is listable again** — `kubescape_get_vulnerability_details` returned an array-rooted `outputSchema` (`["null","array"]`) — object-rooted clients reject it, killing *all* that server's tools in `tools/list`; now `{"matches": [...]}` ([tools#101](https://github.com/kagent-dev/tools/pull/101)). `LOG_LEVEL` also works — `logger.Init` had forced `slog.LevelInfo` ([tools#100](https://github.com/kagent-dev/tools/pull/100)).
- **API surface:** the App Extensions example emitted `kagent.dev/v1alpha2`, a group kagent no longer serves; now built from `KAGENT_API_VERSION`, plus a lint rule banning hand-written `apiVersion` strings ([#3640](https://github.com/kagent-dev/kagent/pull/3640)). The user-set `kagent.dev/prompt-library` label is now `v1alpha3.PromptLibraryLabel` (reviewed API) ([#3638](https://github.com/kagent-dev/kagent/pull/3638)).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3629](https://github.com/kagent-dev/kagent/pull/3629) | [backport] feat: improve install and first-agent onboarding (→ v1.0.1) | EItanya | 11-11 |
| kagent | [#3550](https://github.com/kagent-dev/kagent/pull/3550) | fix(database): retry failed PostgreSQL logins, report `Unavailable` | danielle-mustillo | 11-11 |
| kagent | [#3568](https://github.com/kagent-dev/kagent/pull/3568) | fix(go/adk): Anthropic streaming tool calls in block order | pcbeingused333 | 11-12 |
| kagent | [#3651](https://github.com/kagent-dev/kagent/pull/3651) | chore: remove unsupported Azure OpenAI inline token config | supreme-gg-gg | 11-11 |
| kagent | [#3640](https://github.com/kagent-dev/kagent/pull/3640) | fix(ui): App Extensions example uses `KAGENT_API_VERSION` | EvannDev | 11-11 |
| tools | [#101](https://github.com/kagent-dev/tools/pull/101) | fix: object output schema for Kubescape vuln details | benjaminhuo | 11-11 |
| tools | [#100](https://github.com/kagent-dev/tools/pull/100) | fix(logger): honor `LOG_LEVEL` | lbarrosby | 11-11 |
| doc2vec | [#143](https://github.com/kagent-dev/doc2vec/pull/143) | chore(deps): express 5.3.0 (4 CVEs), pg, turndown (2.17.5) | djannot | 11-12 |

+3 more: prompt-library label, attachment-size, dev container — [all PRs merged in window](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3A%3E%3D2026-11-11&type=pullrequests). **substrate: zero commits, zero merges**; nothing in website, kmcp, khook, mockllm, mockmcp, community, byo-ext.

## Watch list (kagent Enterprise alpha12 / Agent Substrate 0.6 on omni-viper)

- **Pin images before the next refresh.** Substrate components are tag-only today; open `charts:` `image.digests` appends `@sha256:…` per component for `substrate`, `-podcert`, `-atelet` ([#176](https://github.com/kagent-dev/substrate/pull/176)); also open `gvisorReleaseLocation` (air-gap demos, [#175](https://github.com/kagent-dev/substrate/pull/175)) and `networkPolicies.enabled` ([#174](https://github.com/kagent-dev/substrate/pull/174)).
- **Security fix to track, not merged — [tools#107](https://github.com/kagent-dev/tools/pull/107) (open/blocked).** Under `TOKEN_PASSTHROUGH=true` the k8s tools handed the caller's bearer token to kubectl **on argv**: it leaked into the tool result (→ agent context, model requests, session history), span status, the process list and a logged cache key. Worse, `--token` doesn't override a kubeconfig client cert, so the API server could authenticate as the *server's* identity — passthrough did not constrain the caller. Fix: per-call `0600` `tokenFile` kubeconfig + redaction. Until then, keep raw tool errors off-screen on v0.3.1.
- **Session/cancel/recovery in review** (open — test on omni-viper): resumed Claude turn stops on cancel [#3647](https://github.com/kagent-dev/kagent/pull/3647), unrecorded suspension reconciled [#3654](https://github.com/kagent-dev/kagent/pull/3654), cancel a task whose turn can't be replayed [#3652](https://github.com/kagent-dev/kagent/pull/3652) (draft), recover/suspend actors after abandoned dispatches [#3635](https://github.com/kagent-dev/kagent/pull/3635), crash recovery on send [#3399](https://github.com/kagent-dev/kagent/pull/3399), session/push recovery telemetry [#3593](https://github.com/kagent-dev/kagent/pull/3593).
- **Harness/model consistency open:** one ask-user answer rule across harnesses [#3656](https://github.com/kagent-dev/kagent/pull/3656), report a sub-agent's 2nd approval instead of inviting a new run [#3655](https://github.com/kagent-dev/kagent/pull/3655), reject malformed tool-call args from OpenAI chat [#3069](https://github.com/kagent-dev/kagent/pull/3069).
- **UI to re-screenshot after upgrading:** single Create modal [#3633](https://github.com/kagent-dev/kagent/pull/3633), multi-line `KAGENT_UI_EXTENSION_*` kept in `env-config.js` [#3393](https://github.com/kagent-dev/kagent/pull/3393); Agent-Substrate-on-any-cluster blog ([website#722](https://github.com/kagent-dev/website/pull/722)).
- Carried forward: kagent 1.0.x pairs with substrate 1.0.1/1.0.2, not "Agent Substrate 0.6"; substrate `upgrade` still doesn't roll WorkerPool images and needs the install `-c` config.
