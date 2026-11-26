# agent-summary

Daily findings from [kagent](https://kagent.dev) agents running on **Agent Substrate** (cluster `omni-viper`).
Every report below is written and committed by an agent from its sandbox; this page is rewritten each
morning by the `agent-summary-digest` agent.

## Today's highlights - 2026-11-26

**Bottom line:** 5 of 7 reports arrived (redteam-posture missing again, k8s-cve stale); 1 needs action today — upgrade fw-maniak-hq to FortiOS 7.4.13 to close four advisories, three CVSS 9.0+.

### Action needed
- **fortiguard-cve** - 4 of 7 FortiOS/FortiGate advisories from the last 30 days affect the running FortiOS **v7.4.11 build 2878**: FG-IR-26-268 / CVE-2026-126254 (BGP, 9.6), FG-IR-26-267 / CVE-2026-126253 (OSPF auth bypass, 9.3), FG-IR-26-273 / CVE-2026-126259 (HA/FGSP, 9.0), FG-IR-26-269 / CVE-2026-126255 (SSH path traversal, 7.4). All four are fixed by: **FortiOS 7.4.13** (interim: restrict BGP/OSPF peers, isolate HA/FGSP links, limit admin SSH). [Details](reports/fortiguard-cve/latest.md)
- **k8s-cve** (**stale** — last run 2026-11-25; today's 09:15 run never published) - unpatched: **Argo CD v3.5.3 → v3.5.4** (CRITICAL CVE-2026-77459 + HIGH CVE-2026-55797), **CoreDNS v1.12.3 → v1.14.7+** (10 HIGH), **Flannel v0.27.2 → ≥ v0.28.2** (HIGH CVE-2026-32241 cross-node RCE). Re-run to confirm. [Details](reports/k8s-cve/latest.md)
- **redteam-posture** - no `reports/redteam-posture/` directory exists at all — 7th consecutive day absent; check the schedule's history in the kagent UI and re-run. [Audit](reports/_status/latest.md)

### Worth knowing
- **agentgateway-updates** - quiet (HEAD `134cbf02`, still v1.7.0), 9 PRs opened: [#4570](https://github.com/agentgateway/agentgateway/pull/4570) leaves Vertex `embedContent` picked up as the model name until merged, [#4568](https://github.com/agentgateway/agentgateway/pull/4568) shuts telemetry down after drain, and auth pair [#4545](https://github.com/agentgateway/agentgateway/pull/4545)/[#4394](https://github.com/agentgateway/agentgateway/pull/4394) change IdP-outage fail-open/closed behaviour — re-run the JWT demo when either lands. [Details](reports/agentgateway-updates/latest.md)
- **kagent-updates** - substrate [#192](https://github.com/kagent-dev/substrate/pull/192) still `blocked`, so the non-`ate-system` installer bug stays live — do not rename the substrate release/namespace on omni-viper or workers never register. New MCP-tasks demo post [#763](https://github.com/kagent-dev/website/pull/763); install one-liner [#764](https://github.com/kagent-dev/website/pull/764). [Details](reports/kagent-updates/latest.md)
- **agentdesktop-updates** - nothing to upgrade (v0.7.0 / chart 0.7.0); token-enrolled fleets still cannot get `GetLlmGatewayCredential` until [#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287) merges — keep demos on OIDC. Draft [#299](https://github.com/agentdesktop-dev/agentdesktop/pull/299) fixes Copilot CLI / Claude Code version discovery and the credential-helper socket. [Details](reports/agentdesktop-updates/latest.md)
- **fortigate-health** - healthy: CPU 0%, memory flat at 38%, 627 IPv4 sessions vs a 24-h peak of 1,819 — nothing reached the 80% threshold. [Details](reports/fortigate-health/latest.md)

### Report status
| Report | Status | Verdict | Today | History |
| --- | --- | --- | --- | --- |
| [fortiguard-cve](reports/fortiguard-cve/latest.md) | ACTION | 4 of 7 30-day FortiOS advisories hit running v7.4.11, three CVSS 9.0+; fix is 7.4.13. | [2026-11-26](reports/fortiguard-cve/2026/2026-11-26.md) | [all](reports/fortiguard-cve/) |
| [k8s-cve](reports/k8s-cve/latest.md) | MISSING | - | - | [all](reports/k8s-cve/) |
| [kagent-updates](reports/kagent-updates/latest.md) | WATCH | No releases or merges; two website merges; substrate #192 still blocked. | [2026-11-26](reports/kagent-updates/2026/2026-11-26.md) | [all](reports/kagent-updates/) |
| [agentdesktop-updates](reports/agentdesktop-updates/latest.md) | WATCH | Nothing to upgrade; v0.7.0 unchanged; #287 blocker and draft #299 open. | [2026-11-26](reports/agentdesktop-updates/2026/2026-11-26.md) | [all](reports/agentdesktop-updates/) |
| [agentgateway-updates](reports/agentgateway-updates/latest.md) | WATCH | Zero merges or releases (v1.7.0); 9 new PRs, auth pair active. | [2026-11-26](reports/agentgateway-updates/2026/2026-11-26.md) | [all](reports/agentgateway-updates/) |
| [fortigate-health](reports/fortigate-health/latest.md) | OK | HEALTHY - all resource counters far below the 80% alert threshold; nothing to flag. | [2026-11-26](reports/fortigate-health/2026/2026-11-26.md) | [all](reports/fortigate-health/) |
| redteam-posture (no directory) | MISSING | - | - | - |

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
