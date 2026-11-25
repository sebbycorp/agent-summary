# agentdesktop-updates - 2026-11-25

**v0.7.0 shipped this morning** — the whole breaking v0.7 set that had been `main`-only is now a release: gateway JWKS on its own listener (#240), loopback-only controller authn/authz (#220), enrollment tokens (#267), fleet limits (#237/#261), required `controller.caCertificatePath` (#234), OpenCode 2 credential plugin (#229), plus the Rosetta-2 PKG fix (#293). 2 PRs merged since yesterday's report; no new releases in `website`.

Window: 2026-11-24 → 2026-11-25 (from `latest.md`). Repos: `agentdesktop`, `website`.

## Releases

| Repo | Tag | Published | Highlights | Link |
|---|---|---|---|---|
| agentdesktop | **v0.7.0** (latest stable) | 2026-11-25 11:06Z | Gateway JWKS on a dedicated listener, loopback-only controller auth, enrollment tokens, fleet limits, OpenCode 2 plugin, arm64 PKG without Rosetta | [v0.7.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.7.0) |
| agentdesktop | v0.6.0 (previous) | 2026-11-06 | superseded | [v0.6.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0) |
| website | n/a | – | no releases at all | – |

No pre-releases exist. Artifacts: controller image `ghcr.io/agentdesktop-dev/agentdesktop-controller:v0.7.0`, Helm `oci://ghcr.io/agentdesktop-dev/charts/agentdesktop-controller:0.7.0`, headless static Linux binaries (RHEL 8 OK), signed Windows MSIs, notarized macOS PKGs, Sigstore provenance on Linux (`gh attestation verify <FILE> --repo agentdesktop-dev/agentdesktop`).

## Highlights

- **Breaking for every agentgateway demo: the JWKS moved (#240).** `feat(controller)!: listener-scoped config, and serve the gateway JWKS only on a gateway listener` — repoint every agentgateway JWKS URL at `gatewayListener` (`127.0.0.1:8444`, `tls: true`) before upgrading past v0.6.0, or Copilot CLI / Claude Code / Pi / OpenCode token validation silently stops working. Listener-scoped config also means one listener's config no longer leaks into another.
- **Breaking for remote controllers: #220 makes the controller loopback-only by default** with pluggable authn/authz. Anything that reaches a controller off-box needs explicit auth config now — rehearse this before a multi-machine demo. Related tightening: **#234** requires `controller.caCertificatePath`, so plain-HTTP/self-signed controller endpoints are refused by the agent.
- **Zero-touch enrollment is now real, and the flow changed.** **#267** ships operator-signed enrollment tokens; **#236** moves enrollment after user login (no more "pending enrollment"); **#231** stops a system-mode daemon from opening a browser. Caveat unchanged: on a token-enrolled device there is still no user, so `GetLlmGatewayCredential` returns `FAILED_PRECONDITION` — **#287 is the unblock and it is still open.** Also from #267 docs work (**#286**, merged yesterday): deleting a token-enrolled device does not keep it out.
- **Gateway tool coverage grew: OpenCode 2.** **#229** writes an OpenCode 2 credential plugin *next to* the V1 one, so both config generations get gateway credentials — test both in an OpenCode demo.
- **Two security fixes are finally in a release.** **#294** bumps `cookie` 0.18.2→0.18.3 (RUSTSEC-2026-0594: private cookie nonces can repeat in forking processes) and `url` 2.5.8→2.5.9 (RUSTSEC-2026-0592: stack overflow in `Url::origin()` on nested `blob:` URLs), with workspace minimum `url` = 2.5.9 — both on the web/credential path. They had been failing cargo-deny on `main` itself.
- **Supply-chain hardening landed (#256)**: pinned actions and base images, cargo-deny in CI, Helm NetworkPolicy; **#277** replaced unmaintained `humantime-serde` with `jiff`.
- **Apple-silicon installs are fixed in v0.7.0 (#293, merged 11-24 15:12Z).** The released **v0.6.0 arm64 PKG cannot install on a Mac without Rosetta 2** and runs its `preinstall`/`postinstall` translated when Rosetta is present (`distribution.xml` set no `hostArchitectures`). Always demo from v0.7.0 on M-series; the author did not test the no-Rosetta install path.
- **Quality-of-life for demos:** **#296** shows the version number in the controller UI — no more guessing which controller a live fleet is running.

## Merged PRs by repo

`is:pr is:merged merged:>=2026-11-24` → 3 results; [#294](https://github.com/agentdesktop-dev/agentdesktop/pull/294) (11-24 13:08Z) was covered in yesterday's report, so these 2 are new. `website`: no merged PRs, no commits in window (last push 2026-10-30).

| Repo | PR | Title | Author | Merged |
|---|---|---|---|---|
| agentdesktop | [#293](https://github.com/agentdesktop-dev/agentdesktop/pull/293) | fix(macos): declare the package architecture so the PKG installs without Rosetta 2 | [devmittal02](https://github.com/devmittal02) | 2026-11-24 |
| agentdesktop | [#296](https://github.com/agentdesktop-dev/agentdesktop/pull/296) | display version number in the controller UI | [peterj](https://github.com/peterj) | 2026-11-24 |

`main` head is now `2b5a7e5` (#293); `v0.7.0` was cut from it at 11:06Z today.

## Watch list

- **[#288](https://github.com/agentdesktop-dev/agentdesktop/pull/288)** — one shared browser sign-in across gateway credential requests (`BrowserSignInPending`/401 naming the open page, no sign-in deadline, 1h→8h reopen backoff, GitHub step failure no longer fails the credential, identity-only prompts skip the checklist). Still open, `mergeable_state: blocked`, unchanged since 11-23 — this is what makes "sign in once overnight, the four tools work in the morning" demoable.
- **[#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287)** — AttachUser/DetachUser, `POST /v1/login` + `/v1/logout`, `GET /v1/enrollment` gains `user: signedOut|awaitingAuthentication|signedIn`; 37 files, +2538/−546, refreshed 11-25 08:01Z and still `blocked`. Until it merges, token-enrolled fleets get no gateway credential.
- **Two competing metrics designs — decide before building dashboards:** [#290](https://github.com/agentdesktop-dev/agentdesktop/pull/290) (`agentdesktop_limit_hits_total` + `agentdesktop_connections` on an optional `metricsListener`, fleet-listener 30s timeout moved into a counting `RequestTimeout` layer) vs [#180](https://github.com/agentdesktop-dev/agentdesktop/pull/180) (OpenTelemetry SDK, 8 comments, updated 11-24 17:54Z).
- **[#285](https://github.com/agentdesktop-dev/agentdesktop/pull/285)** — opt-in `inactiveDevices.removeAfter` (e.g. `30days`, min `1day`) hourly sweep of stale device records; keeps devices with an open `Connect` stream and re-checks contact at delete time. Filed against `b57329c`, so it needs a rebase onto v0.7.0. Fixes the duplicate-device drift seen in fleets (4 of ~20 duplicated in a week).
- **[#203](https://github.com/agentdesktop-dev/agentdesktop/pull/203)** — sandboxed launches for managed programs, Windows preview (updated 11-24 18:21Z) — relevant to how we show Claude Code/Codex isolation.
- **[#259](https://github.com/agentdesktop-dev/agentdesktop/pull/259)** — `daemon.controllerLimits.allowedGatewayOrigins`, fail-closed at startup (rejected pushes → `CONFIG_STATE_REJECTED` + controller Rejected badge). Known gap: free-form program URLs (Claude Code `env.ANTHROPIC_BASE_URL`, Codex `base_url`) are not checked.
- **[#297](https://github.com/agentdesktop-dev/agentdesktop/pull/297)** — new (11-25 13:37Z): `--build-only`/`--bundle-only` macOS packaging so the app can be signed before PKG creation, plus numeric WiX version for pre-release suffixes → expect proper `-rc`/pre-release builds soon.
- Also active, unreviewed-ish: [#239](https://github.com/agentdesktop-dev/agentdesktop/pull/239) admin audit logging (A5), [#162](https://github.com/agentdesktop-dev/agentdesktop/pull/162) per-device browser PAC + optional compliance check.
- **Upgrade checklist for omni-viper:** v0.7.0 requires migrations `0005_enrollment_receipts` and `0006` (#267); repoint gateway JWKS at `gatewayListener` (#240); set `controller.caCertificatePath` (#234); re-test Windows agentgateway after the MSI/version changes.
