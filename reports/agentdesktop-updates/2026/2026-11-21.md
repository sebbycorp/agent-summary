# agentdesktop-updates - 2026-11-21

Zero-touch enrollment (**#267**) and the fleet-limit follow-ups (**#261**) both merged — but still no release; latest remains v0.6.0, so everything on `main` is unreleased; 2 PRs merged in `agentdesktop`, none in `website`.

Window: 2026-11-20 → 2026-11-21 (from `latest.md`).

## Releases

**None in window.** No new tags or releases in either repo.

| Repo | Latest stable | Latest pre-release | Published | Link |
|---|---|---|---|---|
| agentdesktop | v0.6.0 | none | 2026-11-06 | [v0.6.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0) |
| website | n/a | n/a | – | – |

Yesterday's breaking set (#240 JWKS listener, #256 supply chain, #265 JWT v12) is still `main`-only. This window adds two more unreleased changes on top of it.

## Highlights

- **Zero-touch enrollment is in** ([#267](https://github.com/agentdesktop-dev/agentdesktop/pull/267), merged 11-20, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/b57329c32ff564e771dce98baa129a00a8b8c123)). A device can now enroll with an operator-signed JWT — no user, no browser. Gated behind new config: `enrollmentTokens: { audience, issuers: [{ name, publicKeyPath, revokedIds }] }` on the controller and `controller.enrollmentTokenPath` on the daemon (mutually exclusive with `deviceAuthorization`). Algorithm comes from the configured key, never from the token (ES256/EdDSA/RS256). **Migration `0006`** adds `enrollment_token_issuer` / `enrollment_token_id` to `devices` — run it with any `main` build.
- **⚠ agentgateway impact — token-enrolled devices get no gateway credential.** `GetLlmGatewayCredential` returns `FAILED_PRECONDITION` ("sign in to use the LLM gateway") on a token-enrolled device; `Connect`, `RenewDeviceCertificate` and `GetDeviceUsage` work on the certificate alone. So a zero-touch fleet can be *visible* (usage reporting) but cannot drive Copilot CLI / Claude Code / Pi / OpenCode through agentgateway until `AttachUser` lands — that is now open as [#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287). **Do not demo gateway traffic on token-enrolled devices yet.**
- **Revocation semantics to know before a demo:** revoking a `jti` in `revokedIds` locks out not just future enrollments but every device that token already enrolled (`Unauthenticated`). Removing an issuer — or `enrollmentTokens` entirely — rejects its devices too. Same for a typo'd issuer config; treat this file as blast-radius-bearing.
- **Protobuf/API change:** `EnrollDeviceRequest` gained `oneof credential { id_token = 3; enrollment_token = 4; }`. Field 3 keeps its number, so existing agents are unaffected — but matched controller/agent builds are still the safe bet on `main`.
- **Controller stability at fleet scale** ([#261](https://github.com/agentdesktop-dev/agentdesktop/pull/261), merged 11-20, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/1e565ed9ec1b1819d698be467788e0bcf2ea4b25), fixes #268). Hardens the #237 limits: a **zero limit is now rejected** at config validation instead of silently disabling a refill (or panicking at startup on an oversized `enrollmentInFlight`) — check any harness config that used `0` for "unlimited".
- **Eviction is now LRU-correct after #240.** Previously requests that did not mark a connection busy were invisible to eviction — which covers *every management and gateway request since #240* — so the controller could close the oldest connection mid-request. Idle connections now live in ordered `BTreeMap`s keyed by last request start/end. Relevant if you saw flaky agentgateway or fleet sessions after upgrading past #240.
- Per-device message budget now applies before a new stream's first read, and each device has its own token-bucket lock (`Connect` messages no longer take the global `DeviceStreams` lock). Noisy IdP-outage logging is throttled to one warning per stream per 5 min.
- **Known gap persists:** a device whose dead `Connect` stream holds a connection is refused at a full cap for ~50 s (until keepalive) — documented in `connections.rs`. Matters for tight `maxConnections` demos.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
|---|---|---|---|---|
| agentdesktop | [#267](https://github.com/agentdesktop-dev/agentdesktop/pull/267) | feat(enrollment): enroll devices with an operator-signed token | ibalat | 2026-11-20 |
| agentdesktop | [#261](https://github.com/agentdesktop-dev/agentdesktop/pull/261) | fix(controller): fleet limit follow-ups to #237 | ilackarms | 2026-11-20 |

`website`: **none in window** (no commits, PRs or releases; last activity 2026-10-30).

## Watch list

- **[#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287) (open, opened 11-21 — the one to track).** `AttachUser` / new `DetachUser`; `POST /v1/login` (`agentdesktop sign-in`), `POST /v1/logout` (`agentdesktop sign-out`) now signs the *user* out and keeps the device enrolled (replaces #267's logout-re-enrolls-under-a-new-key behaviour). `GET /v1/enrollment` gains `user: signedOut | awaitingAuthentication | signedIn`. Gateway credentials are tagged with the sign-in so another user's can never be reused. This is what unblocks zero-touch gateway demos — test on omni-viper when it merges. Note `enrollmentTokenPath` **becomes** combinable with `deviceAuthorization` here.
- **[#280](https://github.com/agentdesktop-dev/agentdesktop/pull/280) (open, 34 files)** — database-backed daemon config editing: one-time seeding, validated saves, conflict detection, revision history/restore, web YAML editor, Helm support; file mode stays read-only by default. Follows design [#244](https://github.com/agentdesktop-dev/agentdesktop/pull/244) (draft), mode-switching tracked in issue #279. Expect `daemon.yaml` / Helm `checksum/config` churn, which compounds the #256 strict-YAML parsing (duplicate keys are errors).
- [#286](https://github.com/agentdesktop-dev/agentdesktop/pull/286) (open, docs) — "deleting a token-enrolled device does not keep it out": read this before promising device-block behaviour on token-enrolled fleets. Related [#285](https://github.com/agentdesktop-dev/agentdesktop/pull/285) (open) removes inactive device records.
- **[#180](https://github.com/agentdesktop-dev/agentdesktop/pull/180) (open, updated 11-21)** — retitled to **fleet metrics with the OpenTelemetry SDK** (was Prometheus `/metrics`); check before building demo dashboards on a scrape endpoint that may not be the shape that ships.
- [#257](https://github.com/agentdesktop-dev/agentdesktop/pull/257) (open) — Muse Code discovery + managed gateway config; agentgateway must also route `/muse-code/models`, long idle sessions need `bearer_refresh_secs`. [#259](https://github.com/agentdesktop-dev/agentdesktop/pull/259) (draft) — control-plane threat model + local limits on controller-pushed config; expect config/schema churn.
- Longer-running opens: [#239](https://github.com/agentdesktop-dev/agentdesktop/pull/239) admin audit logging, [#260](https://github.com/agentdesktop-dev/agentdesktop/pull/260) usage/admin session states, [#196](https://github.com/agentdesktop-dev/agentdesktop/pull/196) system daemon manages Pi for one user, [#162](https://github.com/agentdesktop-dev/agentdesktop/pull/162) browser PAC report, [#243](https://github.com/agentdesktop-dev/agentdesktop/pull/243) version detection, [#203](https://github.com/agentdesktop-dev/agentdesktop/pull/203) Windows AppContainer sandboxed launches for Claude Code / OpenCode / Copilot CLI (demo-worthy, needs manual `wxc-exec.exe`).
- **Carried over, still untested on omni-viper from yesterday:** repoint every agentgateway JWKS URL to `gatewayListener` (`127.0.0.1:8444`, `tls: true`, `<release>-gateway` Service) before upgrading past #240; run migrations `0005_enrollment_receipts` (#236) **and now `0006`** (#267); re-validate all YAML for duplicate keys (#256).
- **Combination risk is now five unreleased breaking-ish changes deep** (#236, #240, #256, #265, #267) with no tag since v0.6.0. Use matched controller/agent builds; this whole stack is the v0.7.0 breaking set.
