# agentdesktop-updates - 2026-11-22

Quiet day: **no merges, no commits to `main`, no releases** since the 11-21 report — `main` still sits at `b57329c` (#267, 11-20) and v0.6.0 is still the only shipped version; the only new upstream activity is draft **PR #288**, which fixes the agentgateway browser sign-in storm.

Window: 2026-11-21 → 2026-11-22 (from `latest.md`). Repos checked: `agentdesktop`, `website` (both active, neither archived).

## Releases

**None in window.** No new tags or releases in either repo; no pre-releases exist at all.

| Repo | Latest stable | Latest pre-release | Published | Link |
|---|---|---|---|---|
| agentdesktop | v0.6.0 | none | 2026-11-06 | [v0.6.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0) |
| website | n/a | n/a | – | – |

The whole v0.7.0 breaking set is still `main`-only: **#240** (gateway JWKS listener, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/52c0b191da03aa7bbb61a5218f3589c915242b89)), **#256** (supply-chain hardening, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/b7e4859ed7dac256fba2d55715cfa9d3ee632359)), **#265** (jsonwebtoken v12, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/f5e88a42fbd90438b76e01b67ec2201a1b4a44bf)), **#261** (fleet limits, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/1e565ed9ec1b1819d698be467788e0bcf2ea4b25)), **#267** (enrollment tokens, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/b57329c32ff564e771dce98baa129a00a8b8c123)). Anything you demo from `main` today is a nightly, not a release.

## Highlights

- **agentgateway: the sign-in-tab storm is the real bug of the day** ([#288](https://github.com/agentdesktop-dev/agentdesktop/pull/288), draft, opened 11-22 by @jmcshane, 3 files / +514). Today every gateway credential request that finds no usable token runs its **own** OIDC browser sign-in — new tab, 10-minute wait, then teardown and failure. Background agents (Claude Code, Codex) queue on the global `LOGIN` lock, so an unattended machine collects dozens of tabs and only the newest works. #288 makes one sign-in per account: the first requester starts it, later ones wait on the same `watch` result and get `BrowserSignInPending` (HTTP 401) naming the open page instead of a second tab; the page is reopened only after a 1h→8h doubling backoff.
- **Why it matters for demos:** the flow is *unattended-friendly by design* — the sign-in itself has no deadline, and when identity is the only prompt step `/` redirects straight to the IdP, so with an existing provider session (e.g. Entra SSO) a tab opened while the user is away completes on its own. That turns the "leave it overnight, sign in once, Copilot CLI / Claude Code / Pi / OpenCode work in the morning" story into something you can actually show. Also: the GitHub step after identity sign-in no longer fails the gateway credential, and identity-only prompts skip the click-through checklist (applies to enrollment sign-in too).
- **Still no gateway credential for zero-touch fleets.** #267's token-enrolled devices do `Connect` / `RenewDeviceCertificate` / `GetDeviceUsage` on the certificate alone, but `GetLlmGatewayCredential` returns `FAILED_PRECONDITION` ("sign in to use the LLM gateway") — [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/b57329c32ff564e771dce98baa129a00a8b8c123). #287 (`AttachUser`/`DetachUser`) is the unblock and it has not moved since 11-21.
- **Config-format landmines from #256/#277 are baked into `main`**, so they apply to every `main` build you hand a customer: duplicate YAML keys are now errors, `.inf`/`.nan` are rejected, `<<` merge keys are honoured, and durations of a day or more print as hours (`720h`, not `30days`) in `client config` and the desktop daemon panel ([#277 commit](https://github.com/agentdesktop-dev/agentdesktop/commit/ddef30744936120f096ec13bea3422784297c371)); `1month`/`1year` are rejected outright.
- **Security posture improved, not regressed.** #256 added `cargo-deny` (`deny.toml` + `make deny` + CI job), SHA-pinned Actions, digest-pinned base images (including the previously floating Chainguard runtime image), and an opt-in Helm `NetworkPolicy`; it also swapped `serde_yaml`/`unsafe-libyaml` for `serde-saphyr`. Known ignores worth quoting if asked: a bundled-SQLite CVE (RUSTSEC-2026-0459, blocked on `sqlx`) plus unmaintained-but-required `zbus`/`zvariant` (Linux keyring) and Tauri GTK build deps. #277 removed the now-archived `humantime` (RUSTSEC-2026-0542).
- **Controller reliability at fleet scale** (#261, still the newest controller work): a zero limit is rejected at config validation instead of silently disabling a refill, eviction is LRU-correct after #240 (previously *every* management and gateway request was invisible to eviction, so the oldest connection could be closed mid-request), each device gets its own token-bucket lock, and IdP-outage logging is throttled. Documented gap: a dead `Connect` stream holds its connection at a full cap for ~50 s — relevant for tight `maxConnections` demos.

## Merged PRs by repo

**None in window.** `search_pull_requests` for `org:agentdesktop-dev is:pr is:merged merged:>=2026-11-21` returns 0 results, and `list_commits` on `agentdesktop` shows the newest `main` commit is #267 at 2026-11-20T19:52Z.

| Repo | PR | Title | Author | Merged |
|---|---|---|---|---|
| agentdesktop | — | none in window | – | – |
| website | — | none in window (no commits since 2026-11-15) | – | – |

For context, the last merge burst (already reported 11-21): [#267](https://github.com/agentdesktop-dev/agentdesktop/pull/267) and [#261](https://github.com/agentdesktop-dev/agentdesktop/pull/261), both merged 11-20.

## Watch list

- **[#288](https://github.com/agentdesktop-dev/agentdesktop/pull/288) (new draft, 11-22) — test this on omni-viper first.** It is the agentgateway sign-in fix above; `mergeable_state: blocked` and provider integration tests were not run locally (they need a Linux-built daemon binary). Watch whether `BrowserSignInPending` becomes the behaviour agents must handle — a 401-with-a-page-URL is a new expectation for anyone scripting Claude Code / Codex against the gateway.
- **[#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287) (open, unchanged since 11-21)** — `AttachUser`/`DetachUser`, `POST /v1/login` / `/v1/logout`, `GET /v1/enrollment` gains `user: signedOut | awaitingAuthentication | signedIn`; unblocks gateway traffic on token-enrolled devices. Here `enrollmentTokenPath` becomes combinable with `deviceAuthorization`.
- **[#285](https://github.com/agentdesktop-dev/agentdesktop/pull/285) (open, 11-21)** — controller removes inactive device records; **[#286](https://github.com/agentdesktop-dev/agentdesktop/pull/286) (open, docs, 11-21)** — "deleting a token-enrolled device does not keep it out". Read #286 before promising device-block behaviour on zero-touch fleets; #285 changes what a stale fleet table looks like mid-demo.
- **[#180](https://github.com/agentdesktop-dev/agentdesktop/pull/180) (open, updated 11-21, 7 comments)** — fleet metrics moved to the **OpenTelemetry SDK** (was a Prometheus `/metrics` scrape). Don't build demo dashboards on a scrape endpoint that may not be the shape that ships.
- Carried over, still untested on omni-viper from the 11-21 report: repoint every agentgateway JWKS URL at `gatewayListener` (`127.0.0.1:8444`, `tls: true`, the `<release>-gateway` Service) **before** upgrading past #240; run migrations `0005_enrollment_receipts` **and** `0006` (#267); expect `daemon.yaml`/Helm `checksum/config` churn from the database-backed config editor in #280.

