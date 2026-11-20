# agent-summary

Daily findings from [kagent](https://kagent.dev) agents running on **Agent Substrate** (cluster `omni-viper`).
Every report below is written and committed by an agent from its sandbox; this page is rewritten each
morning by the `agent-summary-digest` agent.

## Today's highlights - 2026-11-20

**Bottom line:** 6 of 7 reports arrived today and 2 need action - the top item is FortiOS **v7.4.11** on `fw-maniak-hq`, affected by 3 CRITICAL and 1 HIGH PSIRT advisories that are all fixed in **7.4.13**, while the `k8s-cve` run (Argo CD CVSS 10.0, still unpatched) did not publish today.

### Action needed

- **fortiguard-cve** - FortiOS **v7.4.11 build 2878** on `fw-maniak-hq` (FortiGate 80F) is affected by CVE-2026-126254 (9.6 CRITICAL, bgp daemon), CVE-2026-126253 (9.3 CRITICAL, ospf authentication bypass), CVE-2026-126259 (9.0 CRITICAL, unauthenticated HA command execution) and CVE-2026-126255 (7.4 HIGH, sshd) - all four fixed in FortiOS **7.4.13**; until upgraded, use virtual patch FG-VD-61445.0day (FMWP db 26.082) plus the HA/FGSP encryption mitigations and restrict OSPF/BGP peering. [Details](reports/fortiguard-cve/latest.md)
- **kagent-updates** - the nginx fix is still not on the branch we run: backport [#3732](https://github.com/kagent-dev/kagent/pull/3732) to `release/v1.0.x` is open with `mergeable_state: blocked` and that branch had no commits, so kagent-enterprise **1.0.0-alpha12 ships nginx 1.31.8** - keep HTTP/2 off or rebuild the UI image ([#3738](https://github.com/kagent-dev/kagent/pull/3738) fixes the v0.10.6 image CVEs). [Details](reports/kagent-updates/latest.md)
- **k8s-cve** *(report stale - last run 2026-11-19, did not publish today)* - Argo CD **v3.5.3** still carries **CVE-2026-77459** (CVSS 10.0, AppProject restrictions bypassed by hooks) and **CVE-2026-55797** (9.9, repo-server command injection), fix in **3.5.4**; flannel `v0.27.2`, local-path-provisioner `v0.0.31`, CoreDNS `v1.12.3` (12 CVEs) and containerd `2.2.7` also unpatched - the audit asks for a re-run. [Details](reports/k8s-cve/latest.md)

### Worth knowing

- **agentdesktop-updates** - breaking gateway-JWKS split ([#240](https://github.com/agentdesktop-dev/agentdesktop/pull/240)) on `main`: `/.well-known/jwks.json` is now served only by the new `gatewayListener` (default `127.0.0.1:8444`, `tls`), the fleet listener refuses HTTP/1, and `fleetListen`/`adminListen` are renamed with the old keys rejected - **repoint every agentgateway JWKS URL first**; nothing released yet, latest is still [v0.6.0](https://github.com/agentdesktop-dev/agentdesktop/releases/tag/v0.6.0). [Details](reports/agentdesktop-updates/latest.md)
- **agentdesktop-updates** - supply-chain hardening ([#256](https://github.com/agentdesktop-dev/agentdesktop/pull/256)): Dockerfile bases pinned by digest, `cargo-deny` CI, Actions pinned to SHAs, optional Helm `NetworkPolicy`, and `serde_yaml` -> `serde-saphyr` (duplicate keys now an error) - validate `controller.yaml` before a demo upgrade; ignored advisory RUSTSEC-2026-0459 (bundled SQLite) still open. [Details](reports/agentdesktop-updates/latest.md)
- **agentgateway-updates** - config can no longer read arbitrary files: new top-level `config.fileAccess` allow/deny ([#4431](https://github.com/agentgateway/agentgateway/pull/4431), new `schema/config.json` key - update the hardened-tenant demo YAML), and backend mTLS can come from pod files with hot reload ([#3872](https://github.com/agentgateway/agentgateway/pull/3872)); 18 merges, no CVEs against org-owned code, still no 1.8 tag. [Details](reports/agentgateway-updates/latest.md)
- **agentgateway-updates** - open **breaking** [#4430](https://github.com/agentgateway/agentgateway/pull/4430) changes the default `agentgateway.user` to `jwt.email` -> `jwt.sub` -> API-key metadata, dropping `apiKey.name` (pin `standardAttributes.user` to keep request-log history); KEDA `ScaledObject` PR [#4471](https://github.com/agentgateway/agentgateway/pull/4471) was closed unmerged 11-19. [Details](reports/agentgateway-updates/latest.md)
- **kagent-updates** - `main`-only breaking changes now documented: gateway `x-user-id` becomes span attribute **`user.id`, not `enduser.id`** ([#3552](https://github.com/kagent-dev/kagent/pull/3552)) and **`azureAdToken` removed from `ModelConfig`** ([#3651](https://github.com/kagent-dev/kagent/pull/3651)) - docs are ahead of alpha12, verify each against our build. [Details](reports/kagent-updates/latest.md)
- **kagent-updates** - OpenShift SCC friction fixed in substrate `main` ([#180](https://github.com/kagent-dev/substrate/pull/180)) but it is post-tag, so OpenShift still needs an `anyuid`/`nonroot-v2` grant until the next release; the tools PDB rework [#106](https://github.com/kagent-dev/tools/pull/106) is likewise not in v0.3.1. [Details](reports/kagent-updates/latest.md)

### Report status

| Report | Status | Verdict | Today | History |
| --- | --- | --- | --- | --- |
| [fortiguard-cve](reports/fortiguard-cve/latest.md) | ACTION | ACTION NEEDED - FortiOS 7.4.11 affected by 3 CRITICAL and 1 HIGH PSIRT advisories, all fixed in 7.4.13 | [2026-11-20](reports/fortiguard-cve/2026/2026-11-20.md) | [all](reports/fortiguard-cve/) |
| [kagent-updates](reports/kagent-updates/latest.md) | ACTION | No kagent releases or merges; nginx CVE unpatched on alpha12 branch - keep HTTP/2 off or rebuild UI image | [2026-11-20](reports/kagent-updates/2026/2026-11-20.md) | [all](reports/kagent-updates/) |
| [agentgateway-updates](reports/agentgateway-updates/latest.md) | WATCH | File-access lockdown #4431 and backend mTLS #3872 merged; 18 merges, no 1.8 tag, no org-code CVEs | [2026-11-20](reports/agentgateway-updates/2026/2026-11-20.md) | [all](reports/agentgateway-updates/) |
| [agentdesktop-updates](reports/agentdesktop-updates/latest.md) | WATCH | Breaking gateway-JWKS listener split #240 plus supply-chain hardening on main; latest release still v0.6.0 | [2026-11-20](reports/agentdesktop-updates/2026/2026-11-20.md) | [all](reports/agentdesktop-updates/) |
| [fortigate-health](reports/fortigate-health/latest.md) | OK | HEALTHY - every counter far below the 80% flag line; CPU idle, memory ~38%, sessions within 80F capacity | [2026-11-20](reports/fortigate-health/2026/2026-11-20.md) | [all](reports/fortigate-health/) |
| [k8s-cve](reports/k8s-cve/latest.md) | MISSING | - | - | [all](reports/k8s-cve/) |
| redteam-posture | MISSING | - | - | - |

Audit: [reports/_status/latest.md](reports/_status/latest.md) - Past digests: [reports/_digest/](reports/_digest/)

## How it works

| Report | Agent | Schedule (America/Toronto) | Sources |
| --- | --- | --- | --- |
| `redteam-posture` | `redteam-posture` | daily 07:00 (paused) | read-only attack-path review of omni-viper |
| `agentgateway-updates` | `oss-update-watcher` | daily 08:00 | github.com/agentgateway: releases, merged PRs |
| `kagent-updates` | `oss-update-watcher` | daily 08:20 | github.com/kagent-dev: releases, merged PRs |
| `agentdesktop-updates` | `oss-update-watcher` | daily 08:40 | github.com/agentdesktop-dev: releases, merged PRs |
| `fortigate-health` | `fortigate` | daily 09:00 | FortiGate `fg_resource_usage`, `fg_system_status` |
| `k8s-cve` | `k8s-cve-checker` | daily 09:15 | Kubernetes version + images -> OSV.dev |
| `fortiguard-cve` | `fortigate-cve-watch` | daily 09:30 | FortiOS version -> FortiGuard PSIRT + Outbreak Alerts |
| `_status` | `agent-summary-auditor` | daily 10:30 | this repo: did every report arrive today? |
| `_digest` + this README | `agent-summary-digest` | daily 10:45 | all of the above |

Layout: `reports/<report>/latest.md` is the newest run; `reports/<report>/YYYY/YYYY-MM-DD.md` keeps one
file per day. Agents are read-only on the systems they inspect; the only thing they write is this repo.
Manifests: `apps/kagent/` in the viper-env repo.
