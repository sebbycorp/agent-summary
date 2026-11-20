# agentdesktop-updates - 2026-11-20

The gateway-JWKS split (**PR #240, breaking**) and supply-chain hardening (#256) landed on `main` — no release yet, latest is still v0.6.0; 8 PRs merged in `agentdesktop`, none in `website`.

Window: 2026-11-19 → 2026-11-20 (from `latest.md`).

## Releases

**None in window.** No new tags or releases in `agentdesktop-dev/agentdesktop` or `.../website`.

| Repo | Latest stable | Latest pre-release | Published | Link |
|---|---|---|---|---|
| agentdesktop | v0.6.0 | none | 2026-11-06 | [v0.6.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0) |
| website | n/a | n/a | – | – |

Everything below is `main`-only and unreleased — v0.6.0 agents/controllers do not carry it.

## Highlights

- **Breaking (agentgateway): the JWKS moved to its own listener** ([#240](https://github.com/agentdesktop-dev/agentdesktop/pull/240), merged 11-19, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/52c0b191da03aa7bbb61a5218f3589c915242b89)). Only the new `gatewayListener` (default `127.0.0.1:8444`, `tls`, started when `gatewayJwt` is set) serves `/.well-known/jwks.json`; the management and **fleet** listeners no longer do, and the fleet listener **refuses HTTP/1**. **Upgrade order: repoint every agentgateway's JWKS URL first** (Helm: the `<release>-gateway` Service) or Copilot CLI / Claude Code / Pi / OpenCode credentials get rejected.
  - Config renames, old keys now **rejected**: `fleetListen` → `fleetListener.listen`, `adminListen` → `adminListener.listen`. Helm handles it; an `existingConfigSecret` must be edited by hand.
  - New connection limits on the management listener (previously unlimited) and gateway listener: `maxConnections` / `connectionsPerAddress` (admin 256/256, gateway 1024/64). Docs recommend `gatewayListener.tls: true` — whoever can alter the keys a gateway fetches can forge credentials.
  - Gateways that export usage still need the fleet port. Controller settings page does not show the gateway listener yet.
- **Breaking-adjacent (YAML): `serde_yaml` replaced by `serde-saphyr`** ([#256](https://github.com/agentdesktop-dev/agentdesktop/pull/256), merged 11-19, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/b7e4859ed7dac256fba2d55715cfa9d3ee632359)). Duplicate keys are now an error, `.inf`/`.nan` rejected, `<<` merge keys honoured, `yes`/`no`/`on`/`off` stay strings. Validate your `controller.yaml` / daemon configs before a demo upgrade.
  - Same PR: `cargo-deny` + `deny.toml` + `make deny` CI job (pinned `cargo-deny 0.20.2`), all GitHub Actions re-pinned to SHAs, **all Dockerfile base images pinned by digest** (incl. the previously floating Chainguard `:latest`), and an **optional Helm `NetworkPolicy`** (`networkPolicy.enabled`, default false; fleet port open by default, admin port namespace-scoped). Not yet tested on a real cluster — good omni-viper candidate.
  - Ignored advisories to be aware of: RUSTSEC-2026-0459 (bundled SQLite CVE, blocked on `sqlx`/`libsqlite3-sys`) plus unmaintained `proc-macro-error`, `zvariant`/`zbus_names` (Linux keyring), `astral-tokio-tar`.
- **Supply chain: `humantime` dropped for `jiff`** ([#277](https://github.com/agentdesktop-dev/agentdesktop/pull/277), merged 11-19, [commit](https://github.com/agentdesktop-dev/agentdesktop/commit/ddef30744936120f096ec13bea3422784297c371)) — RUSTSEC-2026-0542 marked it unmaintained, which was failing the new `deny` job. User-visible: durations ≥1 day now print as hours (`720h`, not `30days`) in `client config` and the desktop daemon info panel; `1month`/`1year` are rejected.
- **JWT stack moved to `jsonwebtoken` 12.0.0** ([#265](https://github.com/agentdesktop-dev/agentdesktop/pull/265), merged 11-19) — rejects unknown critical extensions, fixes `sub`/`iss`/`aud` type confusion, breaking empty-HMAC-key handling. Smoke-test agent→controller and gateway auth after upgrading from `main`.
- **UI test/CI fix that was masking breakage** ([#266](https://github.com/agentdesktop-dev/agentdesktop/pull/266)) — duplicate React copies made 89/92 controller storybook tests fail on a cold cache; CI only passed warm.
- Still open and relevant: operator-signed-token enrollment ([#267](https://github.com/agentdesktop-dev/agentdesktop/pull/267)) — note a token-enrolled device gets `FAILED_PRECONDITION` on `GetLlmGatewayCredential` ("sign in to use the LLM gateway"), so zero-touch devices can't drive gateway demos until `AttachUser` lands; Muse Code support ([#257](https://github.com/agentdesktop-dev/agentdesktop/pull/257)).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
|---|---|---|---|---|
| agentdesktop | [#240](https://github.com/agentdesktop-dev/agentdesktop/pull/240) | feat(controller)!: listener-scoped config, gateway JWKS on its own listener | yuval-k | 2026-11-19 |
| agentdesktop | [#256](https://github.com/agentdesktop-dev/agentdesktop/pull/256) | build: supply-chain hardening (pins, cargo-deny, NetworkPolicy, serde-saphyr) | yuval-k | 2026-11-19 |
| agentdesktop | [#277](https://github.com/agentdesktop-dev/agentdesktop/pull/277) | build(deps): replace unmaintained humantime-serde with jiff | yuval-k | 2026-11-19 |
| agentdesktop | [#275](https://github.com/agentdesktop-dev/agentdesktop/pull/275) | build(deps): bump the rust group with 3 updates (tauri 2.12.6) | dependabot[bot] | 2026-11-19 |
| agentdesktop | [#274](https://github.com/agentdesktop-dev/agentdesktop/pull/274) | build(deps): bump the ui group in /frontend with 2 updates | dependabot[bot] | 2026-11-19 |
| agentdesktop | [#273](https://github.com/agentdesktop-dev/agentdesktop/pull/273) | build(deps): bump library/rust 1.99.0 → 1.99.1-trixie | dependabot[bot] | 2026-11-19 |
| agentdesktop | [#272](https://github.com/agentdesktop-dev/agentdesktop/pull/272) | build(deps): bump library/node 26.11.1 → 26.13.1-bookworm | dependabot[bot] | 2026-11-19 |
| agentdesktop | [#271](https://github.com/agentdesktop-dev/agentdesktop/pull/271) | build(deps): bump chainguard/glibc-dynamic `4cefd4a` → `3359c6b` | dependabot[bot] | 2026-11-19 |

Also closed 11-19 and already in yesterday's report: [#265](https://github.com/agentdesktop-dev/agentdesktop/pull/265), [#266](https://github.com/agentdesktop-dev/agentdesktop/pull/266).

`website`: **none in window** (no commits, PRs or releases; last activity 2026-10-30).

## Watch list

- **Test on omni-viper before any `main` upgrade:** agentgateway JWKS → `gatewayListener` (`127.0.0.1:8444` / `<release>-gateway` Service) with `tls: true`; then re-verify Copilot CLI, Claude Code, Pi and OpenCode authenticate. Also run migration `0005_enrollment_receipts` (from #236) and re-validate every YAML for duplicate keys (#256).
- **Combination risk:** #236 (enrollment rewrite, unreleased) + #240 (listener/config renames, unreleased) + #265 (JWT v12) all landed on `main` with no release since v0.6.0. Use matched controller/agent builds and expect config churn — this is the v0.7.0 breaking set.
- [#261](https://github.com/agentdesktop-dev/agentdesktop/pull/261) (open) — fleet-limit follow-ups to #237: eviction ordering, per-device message budget, zero-limit validation. Known gap: a dead `Connect` stream holds a connection ~50 s at a full cap.
- [#267](https://github.com/agentdesktop-dev/agentdesktop/pull/267) (open) — operator-signed-token enrollment (migration `0006`, `revokedIds` revocation); no LLM-gateway credential until the `AttachUser` follow-up.
- [#257](https://github.com/agentdesktop-dev/agentdesktop/pull/257) (open) — Muse Code discovery + managed gateway config; gateway must also route `/muse-code/models`; needs `bearer_refresh_secs` for long idle sessions.
- [#244](https://github.com/agentdesktop-dev/agentdesktop/pull/244) (draft, updated 11-19) — fleet configuration API design: `PUT /api/v1/daemon-config` with revisions; moves `daemon.yaml` out of the Helm `checksum/config` ConfigMap.
- [#259](https://github.com/agentdesktop-dev/agentdesktop/pull/259) (draft) — control-plane threat model + local limits on controller-pushed config; expect config/schema churn.
- [#203](https://github.com/agentdesktop-dev/agentdesktop/pull/203) (open, large) — Windows AppContainer sandboxed launches for Claude Code / OpenCode / Copilot CLI via a `/sandbox` relay; demo-worthy but Windows-only and needs a manually installed `wxc-exec.exe`.
- Longer-running opens: [#239](https://github.com/agentdesktop-dev/agentdesktop/pull/239) admin audit logging, [#180](https://github.com/agentdesktop-dev/agentdesktop/pull/180) Prometheus `/metrics`, [#196](https://github.com/agentdesktop-dev/agentdesktop/pull/196) system daemon manages Pi for one user, [#162](https://github.com/agentdesktop-dev/agentdesktop/pull/162) browser PAC report, [#243](https://github.com/agentdesktop-dev/agentdesktop/pull/243) version-detection fixes.
