# agent-summary

Daily findings from [kagent](https://kagent.dev) agents running on **Agent Substrate** (cluster `omni-viper`).
Every report below is written and committed by an agent from its sandbox; this page is rewritten each
morning by the `agent-summary-digest` agent.

## Today's highlights - 2026-11-22

**Bottom line:** 6 of 7 reports arrived, 2 need action - top item is FortiOS **v7.4.11 build 2878** on `fw-maniak-hq`, hit by four PSIRT advisories (three CRITICAL), all cleared by **7.4.13**.

### Action needed
- **fortiguard-cve** - FortiOS **v7.4.11 build 2878** (`fw-maniak-hq`, 80F) is inside `>=7.4.0 - <=7.4.12` for CVE-2026-126254 (9.6, bgpd, exploit code "F"), CVE-2026-126253 (9.3, ospfd auth bypass, PoC "P"), CVE-2026-126259 (9.0, unauthenticated HA command execution) and CVE-2026-126255 (7.4, sshd path traversal). Upgrade to **FortiOS 7.4.13**; interim virtual patch **FG-VD-61445.0day** (FMWP db **26.082**). [Details](reports/fortiguard-cve/latest.md)
- **k8s-cve** - `omni-viper`: **Argo CD v3.5.3** CRITICAL CVE-2026-77459 (AppProject bypass via PreDelete/PostDelete hooks) + HIGH CVE-2026-55797 (repo-server command injection) -> **3.5.4**; **flannel v0.27.2** HIGH cross-node RCE CVE-2026-32241 -> **0.28.2**; containerd **2.2.7** -> **2.2.10** via Talos. [Details](reports/k8s-cve/latest.md)
- **k8s-cve** - Also: **CoreDNS v1.12.3** -> **>= 1.14.7** (12 advisories), **argocd-redis 8.2.3-alpine** -> **8.2.6** (5 HIGH RCE-class), pin floating `nginx:1.27-alpine` on `kagent-ui-tls` to **>= 1.30.6-alpine** (3 CRITICAL), align `kube-proxy` **v1.34.11** with the v1.36.4 control plane. [Details](reports/k8s-cve/latest.md)

### Worth knowing
- **agentgateway-updates** - No merges or releases (stable stays **v1.7.0**, no `v1.8.0-alpha.1`); 8 new open PRs incl. log retention [4512](https://github.com/agentgateway/agentgateway/pull/4512) (use `2160h`) and Substrate egress rewrite [4505](https://github.com/agentgateway/agentgateway/pull/4505), live e2e not run. Breaking trio #4300/#4430/#4205 is still newest on `:latest-dev`; website [1577](https://github.com/agentgateway/website/pull/1577) is a Gateway API **v1.7** move - regenerate CRDs before the 1.8 RC. [Details](reports/agentgateway-updates/latest.md)
- **agentdesktop-updates** - No merges since `b57329c`; **v0.6.0** is the only release. Draft [#288](https://github.com/agentdesktop-dev/agentdesktop/pull/288) fixes the browser sign-in storm (new `BrowserSignInPending` 401); token-enrolled devices still get `FAILED_PRECONDITION` on `GetLlmGatewayCredential` pending [#287](https://github.com/agentdesktop-dev/agentdesktop/pull/287). [Details](reports/agentdesktop-updates/latest.md)
- **kagent-updates** - No releases, one merge ([#3407](https://github.com/kagent-dev/kagent/pull/3407)); new PRs [#3762](https://github.com/kagent-dev/kagent/pull/3762) deprecates EgressPolicy `authentication`, [#3763](https://github.com/kagent-dev/kagent/pull/3763) admitted `HTTP://` with TLS set, [#3764](https://github.com/kagent-dev/kagent/pull/3764) MCP tool disambiguation; substrate SCC fix still unreleased, alpha12 pairs with **v1.0.1**. [Details](reports/kagent-updates/latest.md)

### Report status
| Report | Status | Verdict | Today | History |
| --- | --- | --- | --- | --- |
| [fortiguard-cve](reports/fortiguard-cve/latest.md) | ACTION | FortiOS 7.4.11 hit by 4 PSIRT advisories, 3 CRITICAL; fix 7.4.13 | [2026-11-22](reports/fortiguard-cve/2026/2026-11-22.md) | [all](reports/fortiguard-cve/) |
| [k8s-cve](reports/k8s-cve/latest.md) | ACTION | CRITICAL Argo CD plus HIGH flannel, containerd, CoreDNS x12, Redis, nginx | [2026-11-22](reports/k8s-cve/2026/2026-11-22.md) | [all](reports/k8s-cve/) |
| [agentgateway-updates](reports/agentgateway-updates/latest.md) | WATCH | No merges or releases; 8 open PRs | [2026-11-22](reports/agentgateway-updates/2026/2026-11-22.md) | [all](reports/agentgateway-updates/) |
| [agentdesktop-updates](reports/agentdesktop-updates/latest.md) | WATCH | Quiet; draft #288 fixes the sign-in storm | [2026-11-22](reports/agentdesktop-updates/2026/2026-11-22.md) | [all](reports/agentdesktop-updates/) |
| [kagent-updates](reports/kagent-updates/latest.md) | WATCH | No releases; three new PRs touch CRD/API surface | [2026-11-22](reports/kagent-updates/2026/2026-11-22.md) | [all](reports/kagent-updates/) |
| [fortigate-health](reports/fortigate-health/latest.md) | OK | HEALTHY - nothing above 80%; mem 38%, CPU idle | [2026-11-22](reports/fortigate-health/2026/2026-11-22.md) | [all](reports/fortigate-health/) |
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
