# agent-summary

Daily findings from [kagent](https://kagent.dev) agents running on **Agent Substrate** (cluster `omni-viper`).
Every report below is written and committed by an agent from its sandbox; this page is rewritten each
morning by the `agent-summary-digest` agent.

## Today's highlights - 2026-11-19

**Bottom line:** 6 of 7 reports arrived, 3 need action - top item is Argo CD `v3.5.3` on `omni-viper`, hit by CVE-2026-77459 (10.0) and CVE-2026-55797 (9.9), fix in `3.5.4`.

### Action needed

- **k8s-cve** - Argo CD `v3.5.3`: **CVE-2026-77459** (10.0, AppProject restrictions bypassed by PreDelete/PostDelete hooks) + **CVE-2026-55797** (9.9, repo-server command injection via a crafted SSH repo SOCKS5 URL) - bump to **3.5.4**. [Details](reports/k8s-cve/latest.md)
- **k8s-cve** - unpatched too: flannel `v0.27.2` CVE-2026-32241 (cross-node RCE, 0.28.2), local-path-provisioner `v0.0.31` (>= v0.0.36), CoreDNS `v1.12.3` (12 CVEs, >= v1.14.7), containerd `2.2.7`; Talos 1.14.x covers these + the `kube-proxy:v1.34.11` skew. [Details](reports/k8s-cve/latest.md)
- **fortiguard-cve** - 4 advisories of 2026-11-10 hit FortiOS 7.4.11, 3 CRITICAL: CVE-2026-126254 (9.6 bgp), CVE-2026-126253 (9.3 ospf), CVE-2026-126259 (9.0 HA), CVE-2026-126255 (7.4 sshd) - **upgrade fw-maniak-hq to 7.4.13**. [Details](reports/fortiguard-cve/latest.md)
- **kagent-updates** - nginx HTTP/2 fix [#3730](https://github.com/kagent-dev/kagent/pull/3730) (1.31.8 -> 1.31.9, CVE-2026-123633 / CVE-2026-123007) is `main`-only, backport [#3732](https://github.com/kagent-dev/kagent/pull/3732) blocked - **alpha12 has nginx 1.31.8**; rebuild the UI image or keep HTTP/2 off. [Details](reports/kagent-updates/latest.md)

### Worth knowing

- **agentgateway-updates** - prompt-guard policies moved top-level ([#4387](https://github.com/agentgateway/agentgateway/pull/4387)), nested fields only deprecated - fix demo YAML that nests them; open **breaking** [#4430](https://github.com/agentgateway/agentgateway/pull/4430) changes default `agentgateway.user` (pin `standardAttributes.user`). [Details](reports/agentgateway-updates/latest.md)
- **agentgateway-updates** - catalog from models.dev v2 ([#4457](https://github.com/agentgateway/agentgateway/pull/4457)) so quoted rates shift; UI is antd ([#4409](https://github.com/agentgateway/agentgateway/pull/4409)), deck shots stale; no CVEs; no 1.8 tag. [Details](reports/agentgateway-updates/latest.md)
- **agentdesktop-updates** - no release; enrollment redone ([#236](https://github.com/agentdesktop-dev/agentdesktop/pull/236)) as `GetLoginConfiguration` + `EnrollDevice` - **controller and agents must upgrade together**; open breaking [#240](https://github.com/agentdesktop-dev/agentdesktop/pull/240) moves JWKS to a `gatewayListener`. [Details](reports/agentdesktop-updates/latest.md)
- **kagent-updates** - no releases (7th day); Python BYO entry becomes `app.build().run()` ([#3729](https://github.com/kagent-dev/kagent/pull/3729)); blog post shipped ([website #722](https://github.com/kagent-dev/website/pull/722)). [Details](reports/kagent-updates/latest.md)
- **fortigate-health** - HEALTHY (CPU 0%, mem 38%); disk unmeasured, FortiCloud the only log flow. [Details](reports/fortigate-health/latest.md)

### Report status

| Report | Status | Verdict | Today | History |
| --- | --- | --- | --- | --- |
| [k8s-cve](reports/k8s-cve/latest.md) | ACTION | Argo CD v3.5.3 CVSS 10.0/9.9 (fix 3.5.4); flannel, local-path-provisioner, containerd, CoreDNS unpatched | [2026-11-19](reports/k8s-cve/2026/2026-11-19.md) | [all](reports/k8s-cve/) |
| [fortiguard-cve](reports/fortiguard-cve/latest.md) | ACTION | 3 CRITICAL FortiOS PSIRT advisories affect 7.4.11, fixed in 7.4.13 | [2026-11-19](reports/fortiguard-cve/2026/2026-11-19.md) | [all](reports/fortiguard-cve/) |
| [kagent-updates](reports/kagent-updates/latest.md) | ACTION | nginx HTTP/2 RCE fix on main only; alpha12 backport blocked | [2026-11-19](reports/kagent-updates/2026/2026-11-19.md) | [all](reports/kagent-updates/) |
| [agentgateway-updates](reports/agentgateway-updates/latest.md) | WATCH | Prompt-guard CRD, models.dev v2 pricing, antd UI | [2026-11-19](reports/agentgateway-updates/2026/2026-11-19.md) | [all](reports/agentgateway-updates/) |
| [agentdesktop-updates](reports/agentdesktop-updates/latest.md) | WATCH | No release; enrollment RPC rewritten | [2026-11-19](reports/agentdesktop-updates/2026/2026-11-19.md) | [all](reports/agentdesktop-updates/) |
| [fortigate-health](reports/fortigate-health/latest.md) | OK | HEALTHY; disk not reported | [2026-11-19](reports/fortigate-health/2026/2026-11-19.md) | [all](reports/fortigate-health/) |
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
