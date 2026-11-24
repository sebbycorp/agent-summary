# agentdesktop-updates - 2026-11-24

Two PRs merged — both worth noticing: **#294 fixed the two RustSec advisories that had turned cargo-deny red on `main`** (cookie + url), and #286 documented that deleting a token-enrolled device does **not** keep it out; plus three new/updated open PRs that matter for gateway demos — **#288 (single browser sign-in) is now ready for review**, **#290 adds a limit-hit metrics listener**, and **#293 fixes the arm64 PKG needing Rosetta 2**. No new releases; v0.6.0 still latest.

Window: 2026-11-23 → 2026-11-24 (from `latest.md`). Repos: `agentdesktop`, `website` (both active, neither archived).

## Releases

**None in window.** No new tags or releases in either repo; no pre-releases exist at all.

| Repo | Latest stable | Latest pre-release | Published | Link |
|---|---|---|---|---|
| agentdesktop | v0.6.0 | none | 2026-11-06 | [v0.6.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0) |
| website | n/a | n/a | – | – |

The whole v0.7.0 breaking set is still `main`-only: #240 (gateway JWKS listener), #256 (supply-chain hardening), #265 (jsonwebtoken v12), #261 (fleet limits), #267 (enrollment tokens). `main` head moved `b57329c` → `0a683a84` ([#286](https://github.com/agentdesktop-dev/agentdesktop/commit/0a683a84afe6fbfdab75f81ba8d562f68e7f5916)) → `1cacb926` ([#294](https://github.com/agentdesktop-dev/agentdesktop/commit/1cacb9262d3bc3083ed754020c92f05facde14df)). Anything demoed from `main` is a nightly, not a release.

## Highlights

- **Two security fixes landed on `main` — and they were blocking all CI.** [#294](https://github.com/agentdesktop-dev/agentdesktop/pull/294) (merged 11-24 13:08Z) bumps `cookie` 0.18.2→0.18.3 ([RUSTSEC-2026-0594](https://rustsec.org/advisories/RUSTSEC-2026-0594): private cookie nonces can repeat in forking processes) and `url` 2.5.8→2.5.9 ([RUSTSEC-2026-0592](https://rustsec.org/advisories/RUSTSEC-2026-0592): stack overflow in `Url::origin()` on nested `blob:` URLs), with `form_urlencoded` 1.2.2→1.2.3 and a new workspace minimum `url` = 2.5.9. Both are in the **web/credential path**, so this touches the daemon's local API and OAuth handling. Worth knowing: the check was failing on `main` itself, so every PR was red until this merged; verified with `make deny` (cargo-deny 0.20.2), clean clippy, 744 tests.
- **New docs land that changes what you can promise on zero-touch fleets.** [#286](https://github.com/agentdesktop-dev/agentdesktop/pull/286) (merged 11-23 16:24Z) states plainly: deleting a token-enrolled device in the controller UI does **not** keep it out — the next `Connect` is rejected, the daemon drops its identity and re-enrolls with the same token under a **new device ID**. To keep it out until #283 lands: remove the token file first, or revoke the token.
- **#288 is now out of draft — the agentgateway sign-in fix is the one to track.** [Share one browser sign-in across LLM gateway credential requests](https://github.com/agentdesktop-dev/agentdesktop/pull/288) (3 files, +514/−56, updated 11-23 14:34Z, `draft: false`): first requester starts the sign-in, later ones wait on one `watch` result and get `BrowserSignInPending` (HTTP 401) naming the open page instead of a new tab; no deadline on the sign-in, reopen only after 1h→8h doubling backoff; GitHub step failure no longer fails the credential; identity-only prompts skip the checklist straight to the IdP. That's what makes "sign in once overnight, Copilot CLI / Claude Code / Pi / OpenCode work in the morning" demoable — and a new behaviour agents must handle.
- **Fleet-limit observability gets its own listener.** New [#290](https://github.com/agentdesktop-dev/agentdesktop/pull/290) (11-23 19:06Z, 26 files, +1573/−123): `agentdesktop_limit_hits_total` by listener/limit/outcome plus `agentdesktop_connections`, served as Prometheus text on an optional `metricsListener` with its own caps (64/64), labels from fixed sets (≤32 series). Note the behaviour change: the fleet listener's 30 s request timeout moves out of tonic into a counting `RequestTimeout` layer.
- **Enrollment-token hardening follow-ups.** New [#289](https://github.com/agentdesktop-dev/agentdesktop/pull/289) (11 commits, +894/−93, `mergeable_state: dirty`): revoked/removed-issuer tokens now reject with `PERMISSION_DENIED` (not `UNAUTHENTICATED`) so the device **keeps its identity** and re-enrolls only when the token file actually changes; the agent stores a SHA-256 of the token; `publicKeys` per issuer for rotation with `kid` enforcement; API/UI expose `enrollment_token_issuer`/`enrollment_token_id`. Schema regenerated.
- **macOS installs on Apple silicon are broken without Rosetta 2 — and it affects the shipped v0.6.0.** New [#293](https://github.com/agentdesktop-dev/agentdesktop/pull/293) fixes #292: `distribution.xml` sets no `hostArchitectures`, so Installer evaluates the arm64 PKG under Rosetta — "This package requires Rosetta 2" on a clean M-series Mac, and `preinstall`/`postinstall` run translated when Rosetta is present. The released v0.6.0 arm64 package has this `Distribution`.
- **Controller can be locked out of the gateway URL.** Updated [#259](https://github.com/agentdesktop-dev/agentdesktop/pull/259) (11-23 22:35Z, +1378/−52) adds a control-plane threat model plus local-only `daemon.controllerLimits.allowedGatewayOrigins`: pushes outside it are neither applied nor saved, reported as `CONFIG_STATE_REJECTED` with a controller **Rejected** badge, and a saved `remote-config.yaml` outside the list is not applied at startup (fail closed). Known gap: free-form program URLs (Claude Code `env.ANTHROPIC_BASE_URL`, Codex `base_url`) are **not** checked. Strict rollout order: controller → daemons → MDM.
- **Zero-touch fleets still can't get a gateway credential.** [#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287) (`AttachUser`/`DetachUser`, `POST /v1/login` + `/v1/logout`, `GET /v1/enrollment` gains `user: signedOut|awaitingAuthentication|signedIn`) is unchanged since 11-21 and still `blocked`; today `GetLlmGatewayCredential` on a token-enrolled device returns `FAILED_PRECONDITION`.

## Merged PRs by repo

`is:pr is:merged merged:>=2026-11-23` → 2 results, both in `agentdesktop`. `website`: no merged PRs and no commits since 2026-11-15.

| Repo | PR | Title | Author | Merged |
|---|---|---|---|---|
| agentdesktop | [#294](https://github.com/agentdesktop-dev/agentdesktop/pull/294) | build(deps): bump cookie to 0.18.3 and url to 2.5.9 for RustSec advisories | [LutzLange](https://github.com/LutzLange) | 2026-11-24 |
| agentdesktop | [#286](https://github.com/agentdesktop-dev/agentdesktop/pull/286) | docs(enrollment): deleting a token-enrolled device does not keep it out | [ibalat](https://github.com/ibalat) | 2026-11-23 |

## Watch list

- **[#293](https://github.com/agentdesktop-dev/agentdesktop/pull/293)** — test the arm64 PKG on a clean Apple-silicon Mac (no Rosetta) before any macOS fleet demo; author explicitly did not run that case.
- **[#288](https://github.com/agentdesktop-dev/agentdesktop/pull/288)** (now ready for review) — `BrowserSignInPending` 401 + open sign-in URL becomes a contract for anyone scripting Claude Code / Codex against the gateway; provider integration tests not run locally (needs a Linux-built daemon binary).
- **[#290](https://github.com/agentdesktop-dev/agentdesktop/pull/290)** — build demo dashboards on `metricsListener` + `agentdesktop_limit_hits_total`, not the scrape endpoint #180 may change; Helm adds a `<release>-metrics` Service and NetworkPolicy rule.
- **[#289](https://github.com/agentdesktop-dev/agentdesktop/pull/289)** — needs a rebase (`dirty`); if it lands, test token rotation with `publicKeys` + `kid`, and the new "revocation is reversible" identity behaviour.
- **[#259](https://github.com/agentdesktop-dev/agentdesktop/pull/259)** — `allowedGatewayOrigins` fail-closed at startup removes tool settings the daemon wrote; rehearse the revert-and-restart before demoing it. Follow-ups #258 (configurable fail-open/closed), #291 (type-level enforcement).
- **[#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287)** — still the unblock for gateway traffic on token-enrolled devices; `enrollmentTokenPath` becomes combinable with `deviceAuthorization`.
- **[#280](https://github.com/agentdesktop-dev/agentdesktop/pull/280)** / **[#260](https://github.com/agentdesktop-dev/agentdesktop/pull/260)** — DB-backed config editor and explicit usage/admin-session states; re-validate `daemon.yaml`, Helm `checksum/config` churn and console screenshots after merge.
- Carried over: repoint every agentgateway JWKS URL at `gatewayListener` (`127.0.0.1:8444`, `tls: true`) **before** upgrading past #240, and run migrations `0005_enrollment_receipts` **and** `0006` (#267). Related issues: #282 (revoke tokens from UI), #283 (make device deletion stick), #292 (Rosetta PKG).
