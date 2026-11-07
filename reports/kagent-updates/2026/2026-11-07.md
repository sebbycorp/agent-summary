# kagent-updates - 2026-11-07

**Agent Substrate shipped v1.0.0 GA and kagent v1.0.0-rc3 landed with the full split-atelet upgrade runbook** — this is the pair to run omni-viper on for the Enterprise alpha12 cut; 19 PRs merged across kagent, substrate and website since yesterday's digest.

Window: 2026-11-06 → 2026-11-07 (since the 2026-11-06 digest; kagent#3548 and substrate#136 reported there are not repeated).

## Releases

| Repo | Tag | Date | Highlights |
| --- | --- | --- | --- |
| substrate | [v1.0.0](https://github.com/kagent-dev/substrate/releases/tag/v1.0.0) | 2026-11-06 | **First GA** (`v0.6.0...v1.0.0`); charts at OCI version `1.0.0` (`substrate-crds`, `substrate`, `substrate-podcert`, `substrate-atelet`); amd64/arm64 images; `kubectl-ate` attached |
| kagent | [v1.0.0-rc3](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc3) | 2026-11-06 | Pins Substrate **v1.0.0**; installer puts atelet in its own release; 7-step rc2→rc3 migration (Helm 3.17+, `--take-ownership`, `GoldenEgressPolicy` gate) |

Latest per main repo: substrate stable `v1.0.0` (last 0.x `v0.6.0`, newest RC `v1.0.0-rc4`), kagent `v0.10.6` (0.x) / `v1.0.0-rc3` (1.x pre-GA). No releases in window elsewhere in the org.

## Highlights

- **Move off Substrate 0.6.** `v1.0.0` is the GA the alpha12 line should pin; the fork delta since 0.6 is large ([changelog](https://github.com/kagent-dev/substrate/compare/v0.6.0...v1.0.0)) and the notes carry the canonical `ate-setup` deploy command and Helm OCI path.
- **rc3 breaks installs that skip the atelet migration.** Annotating `helm.sh/resource-policy=keep` *before* upgrading `substrate` is required, or the upgrade deletes the atelet DaemonSet; `GoldenEgressPolicy` must be in the `preview` list or ate-api-server rejects golden egress policies ([rc3 notes](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc3)).
- **Runtime config no longer travels in env vars**: compiled config and Agent Cards ship as immutable Postgres-backed bundles over an actor-authenticated bootstrap RPC, verified and persisted before readiness — drops Substrate's per-value env limit (256 KiB bundle cap), with **no legacy env fallback**, so bootstrap-capable runtime images are mandatory ([kagent#3515](https://github.com/kagent-dev/kagent/pull/3515)). Rebuild BYO/`byo-ext` harness images.
- **Breaking tracing rename**: the gateway user is recorded as `user.id`, not `enduser.id` (semconv v1.45). Dashboards, queries and access rules on `enduser.id` must change; a Collector `transform` rule is offered for one-release back-compat. Claude Code's native `user.id` is an account hash — read the trusted user off the invocation span ([kagent#3552](https://github.com/kagent-dev/kagent/pull/3552)).
- **Crashed actors are recoverable**: `SessionService.RevertSession` reverts to the last durable snapshot as a durable `RUNTIME_OPERATION_REVERT` operation (new migration `000002_...sql`); requires `AccessUpdate`, so read-only shares can't trigger it ([kagent#3395](https://github.com/kagent-dev/kagent/pull/3395)).
- **Slow MCP servers no longer kill resumes**: HTTP/SSE/stdio MCP connects are deferred to invocation preparation instead of startup, which was blowing router timeouts and pinning worker reservations; failed toolsets are omitted per-execution and retried later ([kagent#3560](https://github.com/kagent-dev/kagent/pull/3560)).
- **Go API break for embedders**: `go/api/v1alpha3` shed controller-runtime and `database/sql`; `RemoteMCPServerSpec` loses `sql.Scanner`/`driver.Valuer` and the header/`ValueRef`/`ValueSource` resolvers moved to the tool service; `go/api/utils` is deleted ([kagent#3563](https://github.com/kagent-dev/kagent/pull/3563)).
- **ModelConfig save 500 fixed** — API-key edits raced the controller's `Status.SecretHash` write; the handler retries on conflict ([kagent#3546](https://github.com/kagent-dev/kagent/pull/3546)). Also removes KMCP-scaffolder dead code left behind by #3223 ([kagent#3562](https://github.com/kagent-dev/kagent/pull/3562)).
- **New demo asset**: Agent Substrate calculator at `/substrate-calculator` — wake-up time (new conversation / within 30 min / later) plus CPU, memory and snapshot sizing, measured on rc2 + gVisor ([website#697](https://github.com/kagent-dev/website/pull/697)).
- **Upgrade CI is real again**: upgrade tests select rc3 (already split-atelet) and invoke four Harness fixtures per release across upgrade and rollback ([kagent#3558](https://github.com/kagent-dev/kagent/pull/3558)).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3515](https://github.com/kagent-dev/kagent/pull/3515) | feat(runtime): bootstrap revision configuration from durable bundles | supreme-gg-gg | 2026-11-06 |
| kagent | [#3552](https://github.com/kagent-dev/kagent/pull/3552) | feat(telemetry): record the trusted user as user.id | krisztianfekete | 2026-11-06 |
| kagent | [#3395](https://github.com/kagent-dev/kagent/pull/3395) | feat(session): revert crashed runtimes through RevertSession | adalundhe | 2026-11-06 |
| kagent | [#3560](https://github.com/kagent-dev/kagent/pull/3560) | adk/mcp: defer tool discovery until invocation | shashankram | 2026-11-06 |
| kagent | [#3563](https://github.com/kagent-dev/kagent/pull/3563) | refactor(api): move MCP header resolution out of v1alpha3 | timflannagan | 2026-11-06 |
| kagent | [#3546](https://github.com/kagent-dev/kagent/pull/3546) | fix: retry ModelConfig update when the controller writes status first | Charlesthebird | 2026-11-06 |
| kagent | [#3555](https://github.com/kagent-dev/kagent/pull/3555) | chore(deps): upgrade Substrate to v1.0.0 | EItanya | 2026-11-06 |
| kagent | [#3558](https://github.com/kagent-dev/kagent/pull/3558) | fix(ci): restore upgrade tests from RC3 | EItanya | 2026-11-06 |
| kagent | [#3545](https://github.com/kagent-dev/kagent/pull/3545) | fix(ui): show when the Substrate lists were read, not a running age | Charlesthebird | 2026-11-06 |
| kagent | [#3562](https://github.com/kagent-dev/kagent/pull/3562) | refactor: remove helpers orphaned by KMCP removal | timflannagan | 2026-11-06 |
| substrate | [#137](https://github.com/kagent-dev/substrate/pull/137) | Sync upstream main through 94e0a26ac | EItanya | 2026-11-06 |
| website | [#697](https://github.com/kagent-dev/website/pull/697) | Add an Agent Substrate calculator at /substrate-calculator | rinormaloku | 2026-11-06 |
| website | [#698](https://github.com/kagent-dev/website/pull/698) | Document split Agent Substrate atelet release | Rachael-Graham | 2026-11-06 |
| website | [#658](https://github.com/kagent-dev/website/pull/658) | Add reference for A2A metadata and extensions | Stevenjin8 | 2026-11-06 |
| website | [#701](https://github.com/kagent-dev/website/pull/701) | Update kagent 1.x reference docs at v1.0.0-rc3 | github-actions[bot] | 2026-11-06 |

+4 more (website #699/#700 doc/theme, kagent #3556/#3559 unit-test CI): [search](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3A%3E%3D2026-11-06&type=pullrequests). No merged PRs in tools, doc2vec, kmcp, khook, mockllm, community, byo-ext.

## Watch list

- **Pin for alpha12**: kagent main is on Substrate `v1.0.0` ([#3555](https://github.com/kagent-dev/kagent/pull/3555)); rehearse the 7-step rc2→rc3 runbook on omni-viper before cutting alpha12, and expect a kagent `v1.0.0` tag shortly.
- **Helm 3.17+ is now a hard requirement** for the atelet migration (`--take-ownership`); the keep-annotation step is not optional ([rc3 notes](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc3)).
- **Scheduling changed on both atelet charts**: upstream critical priority class + namespace quota, and the staging volume's eviction limit is gone ([substrate#137](https://github.com/kagent-dev/substrate/pull/137)) — check pending-atelet behaviour on a one-node omni-viper.
- **Docs caveats before demoing the split release**: object-store/telemetry config, plus a limitation on git and S3 artifact sources ([website#698](https://github.com/kagent-dev/website/pull/698)).
- **Doc pins** are kagent `1.0.0-rc3` / agent-substrate `1.0.0` / kagent-tools `0.3.0` / max-kube `1.35` ([website#701](https://github.com/kagent-dev/website/pull/701)) — verify copied `helm install --version` lines resolve.
- **Still open**: Substrate unsupported on kagent 0.10.x, and the arm64 Wolfi gVisor PAC issue.
