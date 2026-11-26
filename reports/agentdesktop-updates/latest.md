# agentdesktop-updates - 2026-11-26

**Quiet day upstream — zero merged PRs, zero new releases since yesterday's v0.7.0; `main` is still `2b5a7e5` (#293).** All the action is in the open queue, and it is queue day for the agentgateway path: new draft [#299] fixes Copilot CLI and Claude Code *version discovery* plus the gateway credential-helper socket, and [#289]/[#287] both advanced on top of v0.7.0.

Window: 2026-11-25 → 2026-11-26 (from `latest.md`). Repos: `agentdesktop`, `website`.

## Releases

| Repo | Tag | Published | Highlights | Link |
|---|---|---|---|---|
| agentdesktop | **v0.7.0** (latest stable, unchanged) | 2026-11-25 11:06Z | Cut from `2b5a7e5`; gateway JWKS on its own listener, loopback-only controller auth, enrollment tokens, fleet limits, OpenCode 2 plugin, arm64 PKG without Rosetta | [v0.7.0][r070] |
| agentdesktop | v0.6.0 (previous) | 2026-11-06 | superseded; arm64 PKG is the broken Rosetta one | [v0.6.0][r060] |
| website | n/a | – | no releases | – |

**No new releases in window.** Tag list unchanged from yesterday: v0.7.0 is newest of 9 tags, still no pre-releases and no drafts — so [#297] (pre-release MSI/`-rc` versioning) is not yet paying off. Demo artifacts stay: controller image `ghcr.io/agentdesktop-dev/agentdesktop-controller:v0.7.0`, Helm chart `0.7.0`.

## Highlights

- **Nothing to upgrade today — that is the headline.** v0.7.0 has sat on `main` for ~26 h with no follow-up merges, so omni-viper stays on `v0.7.0` / chart `0.7.0` with no churn. No breaking changes landed in window.
- **New, aimed straight at the agentgateway demos: [#299] (draft, 11-25 16:59Z, +419/−51, 4 commits)** — four daemon bugs found while "running the supported clients through a local gateway on macOS":
  - **Copilot CLI reported a stale version.** Its launcher runs the newest per-user cached build but discovery read only the `@github/copilot` manifest, so it went stale after the first self-update; discovery now reports the higher of newest complete cached build and manifest. Caveats: on a shared device you get the newest build *any* user has, and `COPILOT_*` cache overrides aren't read.
  - **npm-installed Claude Code had no version at all** (only the native `versions/<version>` layout was read); now reads the `@anthropic-ai/claude-code` manifest via a helper shared with Copilot.
  - **A relative `daemon.socket` silently broke credential helpers** — the socket path is baked into each managed tool's helper command and each tool runs it from its own CWD. Now made absolute at startup. **This is the failure mode behind "Claude Code / Pi / OpenCode asks me to sign in and nothing happens."**
  - **Behaviour change:** the daemon now *refuses to load its own config* that sets `telemetry` without `controller` (events were answered 424 and dropped silently). Controller-pushed docs unaffected — but a hand-written local daemon config can now fail to start.
- **Enrollment is converging, and it changes what a demo clicks.** [#289] (11-25 17:18Z, +892/−98, 12 commits) makes a revoked/removed-issuer token device get `PERMISSION_DENIED` and **keep its identity** instead of re-enrolling under new device IDs — undoing a revocation or fixing a typo'd issuer no longer recreates the fleet. Adds `publicKeys` key rotation and surfaces `enrollment_token_issuer`/`_id` in the device UI (search matches token IDs). Open, `blocked`, review on ilackarms; Helm has no `enrollmentTokens` values yet.
- **The gateway-credential blocker is still the blocker.** [#287] (AttachUser, 37 files, +2538/−546) refreshed 11-25 08:01Z, still `mergeable_state: blocked`. Until it lands, **a token-enrolled device cannot get `GetLlmGatewayCredential` (`FAILED_PRECONDITION`) — no Copilot CLI / Claude Code / Pi / OpenCode through agentgateway on a zero-touch fleet.** Demo OIDC (user-signed-in) enrollment until then.
- **Config-as-data is close:** [#280] (11-25 13:03Z) — 33 files, +3922 lines: opt-in DB store for daemon config with validated saves, conflict detection, revision history + restore, and a web YAML editor/builder with rollout status. File mode stays read-only by default; mode switching deferred to issue #279.
- **Queue hygiene:** five Dependabot PRs opened 11-25 18:2x, all unmerged — [#304] (rust, 12 updates), [#303] (frontend, 8), [#302] (glibc-dynamic), [#301], [#300]. Nothing newly vulnerable; `main` still carries yesterday's RustSec fixes.

## Merged PRs by repo

**None in window.** `is:pr is:merged merged:>=2026-11-25` → 0 results for both repos. `main` head unchanged at [`2b5a7e5`][c2b5a7e5] (#293, 11-24 15:12Z); `list_commits since=2026-11-25` is empty. `website` last push 2026-10-30.

## Watch list

- **[#299]** — *highest demo impact of anything open.* Draft + `blocked`; Windows/Linux Copilot cache paths untested. Test on omni-viper: does the fleet UI finally show sane client versions for all four gateway tools?
- **[#287]** — AttachUser/DetachUser, `POST /v1/login` + `/v1/logout`, `GET /v1/enrollment` gains `user: signedOut|awaitingAuthentication|signedIn`, gateway credentials tagged per sign-in. **The unblock for gateway credentials on token-enrolled fleets.** Still `blocked`.
- **[#289]** — stable device IDs under revocation, `publicKeys` rotation, token columns in device UI. Watch for Helm `enrollmentTokens` values, since demos deploy via chart.
- **[#280]** — DB-backed daemon config + web YAML editor with rollout status; API/UX surface change worth testing on omni-viper before it merges.
- **[#290] vs [#180]** — two competing metrics designs (`agentdesktop_limit_hits_total` + `agentdesktop_connections` on an optional `metricsListener`, vs an OpenTelemetry SDK). #290 moved 11-25 16:58Z. Don't build demo dashboards until one wins.
- **[#297]** — `--build-only`/`--bundle-only` macOS packaging (sign the app before PKG creation) + numeric WiX version for pre-release suffixes; 11-25 13:47Z, `blocked`. First `-rc`/pre-release artifacts land behind this.
- **[#259]** — `daemon.controllerLimits.allowedGatewayOrigins`, fail-closed at startup, plus a control-plane threat model doc (11-25 16:45Z). Gap: free-form program URLs (Claude Code `env.ANTHROPIC_BASE_URL`, Codex `base_url`) are *not* checked — don't claim full gateway pinning.
- **[#239]** — admin audit logging (A5), active 11-25 21:27Z. Most-requested compliance item for enterprise fleet pitches.
- **[#285] / [#288] / [#203] / [#162]** — no movement. #285 (opt-in `inactiveDevices.removeAfter`, fixes duplicate-device drift) still filed against pre-v0.7.0 `b57329c`, needs a rebase; [#288] (shared browser sign-in across gateway credential requests) remains the "sign in once overnight, four tools work in the morning" enabler.

<!-- links -->
[r070]: https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.7.0
[r060]: https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0
[c2b5a7e5]: https://github.com/agentdesktop-dev/agentdesktop/commit/2b5a7e5c899d4f96c4b3aabeae6797582cf7a842
[#299]: https://github.com/agentdesktop-dev/agentdesktop/pull/299
[#289]: https://github.com/agentdesktop-dev/agentdesktop/pull/289
[#287]: https://github.com/agentdesktop-dev/agentdesktop/pull/287
[#280]: https://github.com/agentdesktop-dev/agentdesktop/pull/280
[#297]: https://github.com/agentdesktop-dev/agentdesktop/pull/297
[#290]: https://github.com/agentdesktop-dev/agentdesktop/pull/290
[#180]: https://github.com/agentdesktop-dev/agentdesktop/pull/180
[#259]: https://github.com/agentdesktop-dev/agentdesktop/pull/259
[#239]: https://github.com/agentdesktop-dev/agentdesktop/pull/239
[#285]: https://github.com/agentdesktop-dev/agentdesktop/pull/285
[#288]: https://github.com/agentdesktop-dev/agentdesktop/pull/288
[#203]: https://github.com/agentdesktop-dev/agentdesktop/pull/203
[#162]: https://github.com/agentdesktop-dev/agentdesktop/pull/162
[#304]: https://github.com/agentdesktop-dev/agentdesktop/pull/304
[#303]: https://github.com/agentdesktop-dev/agentdesktop/pull/303
[#302]: https://github.com/agentdesktop-dev/agentdesktop/pull/302
[#301]: https://github.com/agentdesktop-dev/agentdesktop/pull/301
[#300]: https://github.com/agentdesktop-dev/agentdesktop/pull/300
