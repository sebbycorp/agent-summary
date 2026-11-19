# agent-summary

Daily findings from [kagent](https://kagent.dev) agents running on **Agent Substrate** (cluster `omni-viper`).
Every report below is written and committed by an agent from its sandbox; this page is rewritten each
morning by the `agent-summary-digest` agent.

## Today's highlights - 2026-11-19

**Bottom line:** 6 of 7 scheduled reports arrived and 2 need action — top item is Argo CD `v3.5.3` on omni-viper (CVE-2026-77459, CVSS 10.0, fixed in v3.5.4), with FortiOS 7.4.11 close behind.

### Action needed
- **k8s-cve** - Argo CD `v3.5.3` is vulnerable to **CVE-2026-77459** (CVSS 10.0, AppProject hook bypass) and **CVE-2026-55797** (CVSS 9.9, repo-server command injection); fix is **v3.5.4**. Also unpatched on omni-viper: flannel `v0.27.2` (CVE-2026-32241 cross-node RCE, fix 0.28.2), local-path-provisioner `v0.0.31` (CVE-2025-62878, 9.9), CoreDNS `v1.12.3` (12 CVEs, fix 1.14.7), containerd `2.2.7` (3 CVEs) — a Talos `v1.13.10 → 1.14.x` upgrade covers flannel + containerd. [Details](reports/k8s-cve/latest.md)
- **fortiguard-cve** - Four 2026-11-10 PSIRT advisories list FortiOS 7.4.11 as affected — FG-IR-26-268 / CVE-2026-126254 (9.6, bgpd), FG-IR-26-267 / CVE-2026-126253 (9.3, ospfd), FG-IR-26-273 / CVE-2026-126259 (9.0, HA), FG-IR-26-269 / CVE-2026-126255 (7.4, sshd). Upgrade fw-maniak-hq (FortiGate 80F) **v7.4.11 → 7.4.13**. [Details](reports/fortiguard-cve/latest.md)

### Worth knowing
- **kagent-updates** - No releases for a 7th day; the nginx HTTP/2 RCE fix [#3730](https://github.com/kagent-dev/kagent/pull/3730) (UI base 1.31.8→1.31.9, CVE-2026-123633 / CVE-2026-123007) is only on `main` — backport [#3732](https://github.com/kagent-dev/kagent/pull/3732) is blocked, so Enterprise alpha12 lacks it; stock installs are safe (HTTP/2 off by default). [Details](reports/kagent-updates/latest.md)
- **kagent-updates** - Breaking-ish for Python BYO on Substrate: [#3729](https://github.com/kagent-dev/kagent/pull/3729) moves the entry point to `app.build().run()`; Substrate IPv6 is a new 6-PR workstream, all unmerged. [Details](reports/kagent-updates/latest.md)
- **agentgateway-updates** - No 1.8 tag yet (newest is the Oct 27 v1.7.0 cut) despite 17 merges in 36h: prompt-guard `policies` moved to top level (#4387) — CRD shape change in `main`, update nested-policy demo YAML; catalog moved to models.dev v2 pricing, so quoted rates shift. [Details](reports/agentgateway-updates/latest.md)
- **agentdesktop-updates** - Unreleased `main` broke enrollment: `BeginEnrollment`/`CompleteEnrollment` → `GetLoginConfiguration` + `EnrollDevice` (#236) — controller and agents must upgrade together, no fallback. Open #240 moves JWKS to a `gatewayListener` (`127.0.0.1:8444`) — repoint gateway JWKS URLs first. [Details](reports/agentdesktop-updates/latest.md)
- **fortigate-health** - Healthy (CPU 0%, memory 38% at 09:00), but disk is unverifiable here (`log_disk_status: not_available`) and logging is FortiCloud-only. [Details](reports/fortigate-health/latest.md)

### Report status
| Report | Status | Verdict | Today | History |
| --- | --- | --- | --- | --- |
| [k8s-cve](reports/k8s-cve/latest.md) | ACTION | ACTION NEEDED — Argo CD v3.5.3 CVSS 10.0/9.9 (fix 3.5.4), plus flannel, containerd, local-path-provisioner, CoreDNS | [2026-11-19](reports/k8s-cve/2026/2026-11-19.md) | [all](reports/k8s-cve/) |
| [fortiguard-cve](reports/fortiguard-cve/latest.md) | ACTION | Four FortiOS PSIRT advisories (three CRITICAL) affect 7.4.11; upgrade fw-maniak-hq to 7.4.13 | [2026-11-19](reports/fortiguard-cve/2026/2026-11-19.md) | [all](reports/fortiguard-cve/) |
| [kagent-updates](reports/kagent-updates/latest.md) | WATCH | No releases (7th day); nginx HTTP/2 fix on main, backport #3732 to release/v1.0.x still open | [2026-11-19](reports/kagent-updates/2026/2026-11-19.md) | [all](reports/kagent-updates/) |
| [agentgateway-updates](reports/agentgateway-updates/latest.md) | WATCH | No 1.8 tag; prompt-guard CRD reshuffle #4387 merged, models.dev v2 pricing catalog, 17 merges in 36h | [2026-11-19](reports/agentgateway-updates/2026/2026-11-19.md) | [all](reports/agentgateway-updates/) |
| [agentdesktop-updates](reports/agentdesktop-updates/latest.md) | WATCH | No release (v0.6.0); enrollment RPC rewrite #236 breaking and unreleased, fleet gRPC limits, OpenCode 2 auth fix | [2026-11-19](reports/agentdesktop-updates/2026/2026-11-19.md) | [all](reports/agentdesktop-updates/) |
| [fortigate-health](reports/fortigate-health/latest.md) | OK | HEALTHY — no resource counter above 80%; every reported metric sits below 50% | [2026-11-19](reports/fortigate-health/2026/2026-11-19.md) | [all](reports/fortigate-health/) |
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
