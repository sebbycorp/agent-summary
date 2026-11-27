# agentdesktop-updates - 2026-11-27

**Still zero merged PRs and no new release — v0.7.0 (11-25) is 2 days old, `main` unchanged at `2b5a7e5` — but the agentgateway blocker moved: [#287] (AttachUser) got a reviewer-ready push at 11-27 10:57Z, with [#289] refreshed the same minute and an explicit ask to merge #287 next.** That pair is what finally gives a token-enrolled device an LLM-gateway credential, so the Copilot CLI / Claude Code / Pi / OpenCode-through-agentgateway zero-touch demo is close.

Window: 2026-11-26 → 2026-11-27 (from `latest.md`, dated 2026-11-26). Repos: `agentdesktop`, `website`.

## Releases

| Repo | Tag | Published | Highlights | Link |
|---|---|---|---|---|
| agentdesktop | **v0.7.0** (latest stable, unchanged) | 2026-11-25 11:06Z | Gateway JWKS on its own listener, loopback-only controller auth, enrollment tokens, fleet limits, OpenCode 2 plugin, arm64 PKG without Rosetta | [v0.7.0][r070] |
| agentdesktop | v0.6.0 (previous) | 2026-11-06 | Superseded — its arm64 PKG is the broken Rosetta one | [v0.6.0][r060] |
| website | n/a | – | no releases; repo last pushed 2026-10-30 | – |

**No releases published in window; no pre-releases or drafts exist**, so [#297]'s `-rc` packaging has still produced nothing. Demo artifacts unchanged: controller image `ghcr.io/agentdesktop-dev/agentdesktop-controller:v0.7.0`, chart `oci://ghcr.io/agentdesktop-dev/charts/agentdesktop-controller:0.7.0`.

## Highlights

- **Nothing to upgrade, nothing broken.** `is:pr is:merged merged:>=2026-11-26` → 0 for both repos; newest commit on `main` is still [`2b5a7e5`][c2b5a7e5] (#293, macOS PKG arch, 11-24). omni-viper stays on v0.7.0 / chart 0.7.0.
- **Gateway-credential unblock advanced: [#287] AttachUser/DetachUser, updated 11-27 10:57Z.** `ibalat` addressed review and pinged `yuval-k` to re-review and merge *after* #289 so a rebase can happen first. 37 files, +2538/−546, still `mergeable_state: blocked`. From the PR: `POST /v1/login` (`agentdesktop sign-in`) + `POST /v1/logout`, `GET /v1/enrollment` gains `user: signedOut | awaitingAuthentication | signedIn`, cached gateway credentials tagged per sign-in, and a token-enrolled device finally answers `GetLlmGatewayCredential` with a credential instead of `FAILED_PRECONDITION` ("sign in to use the LLM gateway"). **Until it merges, the four gateway clients only work on an OIDC (user-signed-in) enrolled device — keep demos on that path.**
- **[#289] refreshed in the same minute (11-27 10:57Z) — the merge gate for #287.** Revoked / removed-issuer token now returns `PERMISSION_DENIED` so the device **keeps its identity and device ID** instead of re-enrolling under a new one ([`b5fb3818`][c-permdenied]); agent holds identity until the token file changes, recording a SHA-256 of the token ([`9c789f4e`][c-identity]); `publicKeys` gives per-issuer key rotation ([`61ae2e49`][c-publickeys]); device UI shows `enrollment_token_issuer`/`_id`, search matches token IDs ([`74683465`][c-tokenui]). Still no Helm `enrollmentTokens` values — matters because we deploy by chart.
- **[#299] unchanged since 11-25 but still the highest-impact open item for gateway demos** (draft, +419/−51, 4 commits): Copilot CLI version discovery reports the newest complete cached build instead of the stale `@github/copilot` manifest; npm-installed Claude Code gets a version via the shared `@anthropic-ai/claude-code` manifest helper; a **relative `daemon.socket` is made absolute at startup** — the "Claude Code / Pi / OpenCode asks me to sign in and nothing happens" failure, since the socket path is baked into each tool's credential-helper command and each tool runs it from its own CWD; and the daemon refuses its own config that sets `telemetry` without `controller` (behaviour change for hand-written local configs).
- **Queue otherwise frozen.** Only #287 and #289 touched since 11-26; no PRs *opened* in the window; the 11-25 Dependabot batch (rust ×12, frontend ×8, glibc-dynamic, +2) untouched.

## Merged PRs by repo

**None in window.** `is:pr is:merged merged:>=2026-11-26` → 0 results for `agentdesktop` and `website`; no commits reached `main` since [`2b5a7e5`][c2b5a7e5] (11-24 15:12Z). Most recent merges, all pre-window and shipped in v0.7.0:

| Repo | PR | Title | Author | Merged |
|---|---|---|---|---|
| agentdesktop | [#293](https://github.com/agentdesktop-dev/agentdesktop/pull/293) | fix(macos): declare package architecture so the PKG installs without Rosetta 2 | @devmittal02 | 2026-11-24 |
| agentdesktop | [#296](https://github.com/agentdesktop-dev/agentdesktop/pull/296) | display version number in the controller UI | @peterj | 2026-11-24 |
| agentdesktop | [#294](https://github.com/agentdesktop-dev/agentdesktop/pull/294) | build(deps): bump cookie 0.18.3 / url 2.5.9 for RustSec advisories | @LutzLange | 2026-11-24 |
| agentdesktop | [#286](https://github.com/agentdesktop-dev/agentdesktop/pull/286) | docs(enrollment): deleting a token-enrolled device does not keep it out | @ibalat | 2026-11-23 |
| agentdesktop | [#267](https://github.com/agentdesktop-dev/agentdesktop/pull/267) | feat(enrollment): enroll devices with an operator-signed token | @ibalat | 2026-11-20 |

## Watch list

- **[#287]** — the unblock for gateway credentials on token-enrolled fleets; review-ready, merge order #289 → #287. Test on omni-viper once merged: token-enroll, `agentdesktop sign-in`, then confirm all four gateway tools reach agentgateway.
- **[#289]** — stable device IDs under revocation, `publicKeys` rotation, token columns in device UI; watch for Helm `enrollmentTokens` values.
- **[#299]** — draft + `blocked`; Windows/Linux Copilot cache paths untested. Test: sane client versions for all four gateway tools, and credential helpers still resolving on a relative-socket config.
- **[#280]** — DB-backed daemon config + web YAML editor with rollout status (33 files, +3922, `blocked`); API/UX change worth testing before it lands.
- **[#290] vs [#180]** — competing metrics designs (Prometheus on an optional `metricsListener` vs an OpenTelemetry SDK). Don't build demo dashboards until one wins.
- **[#297]** — `--build-only`/`--bundle-only` macOS packaging + numeric WiX version; first `-rc` artifacts land behind this.
- **[#259]** — `daemon.controllerLimits.allowedGatewayOrigins` (fail-closed). Keep honest in security pitches: free-form program URLs (Claude Code `env.ANTHROPIC_BASE_URL`, Codex `base_url`) are not checked.
- **[#239]** — admin audit logging (A5); the compliance item most asked for in enterprise fleet conversations.
- **[#285] / [#288] / [#203] / [#162]** — no movement. #285 (opt-in `inactiveDevices.removeAfter`) still sits against pre-v0.7.0 `b57329c`, needs a rebase; [#288] (shared browser sign-in across gateway credential requests) remains the "sign in once overnight, four tools work in the morning" enabler.

<!-- links -->
[r070]: https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.7.0
[r060]: https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0
[c2b5a7e5]: https://github.com/agentdesktop-dev/agentdesktop/commit/2b5a7e5c899d4f96c4b3aabeae6797582cf7a842
[c-permdenied]: https://github.com/agentdesktop-dev/agentdesktop/commit/b5fb3818c6334aea959b74e31e7729642803ec97
[c-identity]: https://github.com/agentdesktop-dev/agentdesktop/commit/9c789f4ef2f721aa32140472ad81d44f25356d76
[c-publickeys]: https://github.com/agentdesktop-dev/agentdesktop/commit/61ae2e49977d76201fdb4c20a411db488f87cd03
[c-tokenui]: https://github.com/agentdesktop-dev/agentdesktop/commit/7468346550f1182513160181ac9535a08860dcee
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
