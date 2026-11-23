# agentdesktop-updates - 2026-11-23

Second consecutive quiet day upstream: **no merged PRs, no new commits to `main`, no releases** — `main` still sits at `b57329c` (#267, 11-20) and v0.6.0 is still the only shipped version; the only movement today is two of @peterj's open PRs rebasing this morning, with Copilot resolving the merge conflicts in the database-backed config editor (#280).

Window: 2026-11-22 → 2026-11-23 (from `latest.md`). Repos checked: `agentdesktop`, `website` (both active, neither archived).

## Releases

**None in window.** No new tags or releases in either repo; no pre-releases exist at all.

| Repo | Latest stable | Latest pre-release | Published | Link |
|---|---|---|---|---|
| agentdesktop | v0.6.0 | none | 2026-11-06 | [v0.6.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0) |
| website | n/a | n/a | – | – |

The whole v0.7.0 breaking set is still `main`-only: #240 (gateway JWKS listener), #256 (supply-chain hardening), #265 (jsonwebtoken v12), #261 (fleet limits, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/1e565ed9ec1b1819d698be467788e0bcf2ea4b25)), #267 (enrollment tokens, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/b57329c32ff564e771dce98baa129a00a8b8c123)). Anything you demo from `main` today is a nightly, not a release.

## Highlights

- **The database-backed config editor (#280) is the one to watch — it moved this morning.** 34 files, +3296/−203: opt-in DB storage with one-time seeding, validated saves, conflict detection, revision history + restore, a web YAML editor/builder with review and rollout status, file mode read-only by default, plus Helm/deployment support ([PR](https://github.com/agentdesktop-dev/agentdesktop/pull/280)). At 06:21 ET @peterj commented `@copilot resolve the merge conflicts in this pull request`, and Copilot merged `main` and resolved the conflict in `crates/core/src/config.rs`, preserving both the daemon config changes and the enrollment-token types (merge commit `f081157`, [comment](https://github.com/agentdesktop-dev/agentdesktop/pull/280#issuecomment-7093357090)). Demo impact: expect `daemon.yaml`/Helm `checksum/config` churn and two config "modes" — and note it implements only part of design PR #244, with mode switching tracked separately in issue #279.
- **#260 also rebased this morning** (06:20 ET, still `blocked` on review, 5 files / +343): device-usage requests and the admin session lifecycle move to **explicit states**, plus Storybook tests ([PR](https://github.com/agentdesktop-dev/agentdesktop/pull/260)). Small, but it's the shape of the admin-console state machine your demos click through, so re-record any console walkthroughs after it lands.
- **agentgateway: the sign-in-tab storm fix (#288) is still a draft and unchanged since 11-22** ([PR](https://github.com/agentdesktop-dev/agentdesktop/pull/288), 3 files / +514, `mergeable_state: blocked`). Today every gateway credential request with no usable token runs its **own** OIDC browser sign-in (new tab, 10-min wait, teardown, fail); background agents (Claude Code, Codex) queue on the global `LOGIN` lock, so an unattended machine collects dozens of tabs and only the newest works. #288 makes it one sign-in per account: first requester starts it, later ones wait on the same `watch` result and get `BrowserSignInPending` (HTTP 401) naming the open page; reopen only after 1h→8h doubling backoff; GitHub step failure no longer fails the credential; identity-only prompts skip the checklist and go straight to the IdP. That is what makes "sign in once overnight, Copilot CLI / Claude Code / Pi / OpenCode work in the morning" demoable.
- **Zero-touch fleets still can't get a gateway credential.** #267's token-enrolled devices do `Connect` / `RenewDeviceCertificate` / `GetDeviceUsage` on the certificate alone, but `GetLlmGatewayCredential` returns `FAILED_PRECONDITION` ("sign in to use the LLM gateway") ([commit](https://github.com/agentdesktop-dev/agentdesktop/commit/b57329c32ff564e771dce98baa129a00a8b8c123)). #287 (`AttachUser`/`DetachUser`) is the unblock; no movement since 11-21.
- **No security-relevant changes in window.** Nothing new in the advisory set; #256's `cargo-deny` + SHA/digest pinning posture and the known ignores (bundled-SQLite CVE blocked on `sqlx`, unmaintained-but-required `zbus`/`zvariant`, Tauri GTK build deps) are unchanged on `main`.
- **Controller reliability at fleet scale** remains #261 (still the newest controller work): zero limits rejected at config validation, LRU-correct eviction after #240, per-device token-bucket locks, throttled IdP-outage logging. Documented gap for tight `maxConnections` demos: a dead `Connect` stream holds its connection at a full cap for ~50 s.

## Merged PRs by repo

**None in window.** `is:pr is:merged merged:>=2026-11-22` returns 0 results for the org, and `list_commits` on `agentdesktop` since 11-22 returns nothing (newest `main` commit is #267 at 2026-11-20T19:52Z).

| Repo | PR | Title | Author | Merged |
|---|---|---|---|---|
| agentdesktop | — | none in window | – | – |
| website | — | none in window (no commits since 2026-11-15) | – | – |

For context, the last merge burst (already reported): [#267](https://github.com/agentdesktop-dev/agentdesktop/pull/267) and [#261](https://github.com/agentdesktop-dev/agentdesktop/pull/261), both merged 11-20.

## Watch list

- **[#280](https://github.com/agentdesktop-dev/agentdesktop/pull/280) — test on omni-viper next.** Freshly conflict-resolved onto `main`; check the DB-mode seeding and the read-only-file default against your existing `daemon.yaml`, and watch for Helm `checksum/config` churn during a rollout demo.
- **[#288](https://github.com/agentdesktop-dev/agentdesktop/pull/288) (draft)** — decides whether `BrowserSignInPending` (401 + open sign-in URL) becomes behaviour agents must handle: a new expectation for anyone scripting Claude Code / Codex against the gateway. Provider integration tests were not run locally (needs a Linux-built daemon binary).
- **[#260](https://github.com/agentdesktop-dev/agentdesktop/pull/260)** — explicit usage/admin-session states; re-validate console screenshots and Storybook-based demos after merge.
- **[#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287) (open, unchanged)** — `AttachUser`/`DetachUser`, `POST /v1/login` / `/v1/logout`, `GET /v1/enrollment` gains `user: signedOut | awaitingAuthentication | signedIn`; unblocks gateway traffic on token-enrolled devices; `enrollmentTokenPath` becomes combinable with `deviceAuthorization`.
- **[#285](https://github.com/agentdesktop-dev/agentdesktop/pull/285) (controller removes inactive device records) and [#286](https://github.com/agentdesktop-dev/agentdesktop/pull/286) (docs: "deleting a token-enrolled device does not keep it out")** — read #286 before promising device-block behaviour on zero-touch fleets; #285 changes what a stale fleet table looks like mid-demo.
- **[#180](https://github.com/agentdesktop-dev/agentdesktop/pull/180)** — fleet metrics move to the **OpenTelemetry SDK** (was a Prometheus `/metrics` scrape); don't build demo dashboards on a scrape endpoint that may not be the shape that ships. Carried over: repoint every agentgateway JWKS URL at `gatewayListener` (`127.0.0.1:8444`, `tls: true`) **before** upgrading past #240, and run migrations `0005_enrollment_receipts` **and** `0006` (#267).
