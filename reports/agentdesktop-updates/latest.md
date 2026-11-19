# agentdesktop-updates - 2026-11-19

No new release (latest still v0.6.0); enrollment moved after user login (**breaking RPC change**), fleet gRPC API got hard limits, OpenCode 2 gateway auth fixed; 11 PRs merged in `agentdesktop`, none in `website`.

Window: 2026-11-12 → 2026-11-19 (no prior report; last 7 days used).

## Releases

**None in window.** No tags or releases published in `agentdesktop-dev/agentdesktop` or `.../website` since 2026-11-12.

Current release state (for context):

| Repo | Latest stable | Latest pre-release | Published | Link |
|---|---|---|---|---|
| agentdesktop | v0.6.0 | none | 2026-11-06 | [v0.6.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0) |
| website | n/a | n/a | – | – |

**v0.6.0 artifacts:** `ghcr.io/agentdesktop-dev/agentdesktop-controller:v0.6.0`, Helm `oci://ghcr.io/agentdesktop-dev/charts/agentdesktop-controller:0.6.0`, signed MSI / notarized PKG, static musl headless Linux binaries.

## Highlights

- **Breaking: enrollment protocol rewritten** ([#236](https://github.com/agentdesktop-dev/agentdesktop/pull/236), merged 11-18). `BeginEnrollment`/`CompleteEnrollment` are replaced by new RPCs `GetLoginConfiguration` + `EnrollDevice`. **Controller and agents must be upgraded together — there is no fallback.** Demo impact: an old agent against a new controller will not enroll.
- Enrollment is now **nonce-bound to the device key**: the browser sign-in's nonce must be the SHA-256 of the CSR public key, so tokens from a refresh grant or a sign-in for another key cannot enroll a device. Also drops the 1,024 pending-enrollment cap and in-memory state → **controller replicas no longer need sticky routing**. New DB migration `0005_enrollment_receipts`.
- **New: fleet gRPC API rate/connection limits** ([#237](https://github.com/agentdesktop-dev/agentdesktop/pull/237), merged 11-17). Per-address + total connection caps, HTTP/2 stream caps, message-size caps, enrollment rate limits, per-device Connect budgets. New `fleetLimits` section in `controller.yaml` + Helm values. Ops gotcha: the 50,000-connection default needs `LimitNOFILE` raised under bare systemd/launchd.
- **agentgateway integration: OpenCode 2 now authenticates** ([#229](https://github.com/agentdesktop-dev/agentdesktop/pull/229), merged 11-12). OpenCode 2 silently dropped the V1 credential plugin, so requests hit the gateway with no Agentdesktop JWT → **403**. The agent now writes a V2 plugin (directory + default export, header set in `http.request`) alongside the V1 one. Test this on omni-viper with OpenCode 2.2.x. Known gap: Windows default paths resolve to `C:\etc\opencode` instead of `%ProgramData%\opencode`.
- **agentgateway reliability: no more forced re-enrollment on token expiry** ([#241](https://github.com/agentdesktop-dev/agentdesktop/pull/241), merged 11-12). A rejected `Unauthenticated` stream response used to wipe the saved device identity and force interactive sign-in; the daemon now refreshes the token and reconnects first. Fixes sleep/wake-style flakiness in gateway demos.
- **agentgateway security: separate JWKS listener (open PR, breaking)** ([#240](https://github.com/agentdesktop-dev/agentdesktop/pull/240)). Management + fleet listeners currently serve `/.well-known/jwks.json`, exposing the inventory-capable admin surface to gateways. New dedicated `gatewayListener` (default `127.0.0.1:8444`) and the fleet listener **drops HTTP/1**. Upgrade prerequisite: repoint every gateway's JWKS URL first.
- macOS enrollment no longer opens a stray Safari window from the root LaunchDaemon ([#231](https://github.com/agentdesktop-dev/agentdesktop/pull/231)); the URL is still published on `/v1/enrollment`.
- New tool on the way: **Muse Code** discovery + managed gateway config ([#257](https://github.com/agentdesktop-dev/agentdesktop/pull/257), open) — extends the Copilot CLI / Claude Code / Pi / OpenCode roster.
- Supply-chain hardening in flight ([#256](https://github.com/agentdesktop-dev/agentdesktop/pull/256), open): action/image pins, `cargo-deny`, NetworkPolicy.
- Dependency moves ([#265](https://github.com/agentdesktop-dev/agentdesktop/pull/265)): `jsonwebtoken` 11.1.0 → **12.0.0** (rejects unknown critical extensions, fixes `sub`/`iss`/`aud` type confusion, breaking empty-HMAC-key handling), `prost` 0.14.6, `rand` 0.10.4 — smoke-test JWT paths.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
|---|---|---|---|---|
| agentdesktop | [#236](https://github.com/agentdesktop-dev/agentdesktop/pull/236) | move enrollment after user login - so no need for pending enrollment | yuval-k | 2026-11-18 |
| agentdesktop | [#237](https://github.com/agentdesktop-dev/agentdesktop/pull/237) | fix(controller): add limits to the fleet gRPC API | ilackarms | 2026-11-17 |
| agentdesktop | [#229](https://github.com/agentdesktop-dev/agentdesktop/pull/229) | feat(opencode): write an OpenCode 2 credential plugin next to the V1 one | palexus | 2026-11-12 |
| agentdesktop | [#241](https://github.com/agentdesktop-dev/agentdesktop/pull/241) | Refresh rejected controller credentials before reenrollment | rhummelmose | 2026-11-12 |
| agentdesktop | [#231](https://github.com/agentdesktop-dev/agentdesktop/pull/231) | fix(enrollment): do not open a browser from a system-mode daemon | ibalat | 2026-11-12 |
| agentdesktop | [#254](https://github.com/agentdesktop-dev/agentdesktop/pull/254) | fix(agent): pass open_browser in the auth recovery test fixture | ilackarms | 2026-11-13 |
| agentdesktop | [#266](https://github.com/agentdesktop-dev/agentdesktop/pull/266) | fix(ui): align shared UI peer versions with the apps | yuval-k | 2026-11-19 |
| agentdesktop | [#265](https://github.com/agentdesktop-dev/agentdesktop/pull/265) | build(deps): bump the rust group with 5 updates | dependabot[bot] | 2026-11-19 |
| agentdesktop | [#264](https://github.com/agentdesktop-dev/agentdesktop/pull/264) | build(deps): bump the ui group in /frontend with 9 updates | dependabot[bot] | 2026-11-18 |
| agentdesktop | [#263](https://github.com/agentdesktop-dev/agentdesktop/pull/263) | build(deps): bump taiki-e/install-action 2.90.3 → 2.90.5 | dependabot[bot] | 2026-11-18 |
| agentdesktop | [#262](https://github.com/agentdesktop-dev/agentdesktop/pull/262) | build(deps): bump library/node 26.13.0 → 26.13.1-bookworm | dependabot[bot] | 2026-11-18 |

`website`: **none in window** (no commits, PRs or releases; last activity 2026-10-30).

## Watch list

- **[PR #240](https://github.com/agentdesktop-dev/agentdesktop/pull/240) (open, breaking)** — JWKS moves to a dedicated `gatewayListener` (`127.0.0.1:8444`, Helm `<release>-gateway` Service); the fleet listener stops serving HTTP/1. **Test on omni-viper**: repoint agentgateway's JWKS URL before upgrading the controller, enable `gatewayListener.tls: true`, and verify Copilot CLI / Claude Code / Pi / OpenCode still authenticate.
- **[PR #236](https://github.com/agentdesktop-dev/agentdesktop/pull/236) landed on `main` but is unreleased** — v0.6.0 agents are incompatible with a `main` controller. Use matching builds; run migration `0005_enrollment_receipts`.
- **[PR #257](https://github.com/agentdesktop-dev/agentdesktop/pull/257) (open)** — Muse Code discovery + managed gateway config: new agent surface, worth an early look.
- **[PR #267](https://github.com/agentdesktop-dev/agentdesktop/pull/267) (open)** — operator-signed-token enrollment: possible zero-touch/MDM onboarding path.
- **[PR #261](https://github.com/agentdesktop-dev/agentdesktop/pull/261) (open)** — fleet-limit follow-ups to #237; known gaps: no per-device rate limit on authenticated calls, no refusal metrics, `LimitNOFILE` tuning.
- **[PR #256](https://github.com/agentdesktop-dev/agentdesktop/pull/256) (open)** — CI/supply-chain pins, `cargo-deny`, NetworkPolicy; may change deploy manifests.
- **[PR #196](https://github.com/agentdesktop-dev/agentdesktop/pull/196) (open, long-running)** — system daemon managing Pi for one configured user; relevant to Pi-through-agentgateway demos. Also open: **[PR #259](https://github.com/agentdesktop-dev/agentdesktop/pull/259) (draft)** control-plane threat model + local config limits (expect config/schema churn).
