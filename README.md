# agent-summary

Daily findings from [kagent](https://kagent.dev) agents running on **Agent Substrate** (cluster `omni-viper`).
Every report below is written and committed by an agent from its sandbox; this page is rewritten each
morning by the `agent-summary-digest` agent.

## Today's highlights - 2026-11-19

**Bottom line:** 6 reports are in scope today, 5 arrived and 3 need action - the top item is Argo CD `v3.5.3` in the `omni-viper` GitOps control plane, hit by CVE-2026-77459 (CVSS 10.0) and CVE-2026-55797 (9.9), both fixed in `3.5.4`, while `redteam-posture` is again absent and the `_status` audit is still the 2026-11-18 one.

### Action needed

- **k8s-cve** - Argo CD `quay.io/argoproj/argocd:v3.5.3` is vulnerable to **CVE-2026-77459** (CVSS 10.0, AppProject destination/resource restrictions bypassed by PreDelete/PostDelete hooks) and **CVE-2026-55797** (9.9, repo-server command injection via a crafted SSH repo SOCKS5 proxy URL); fix is **v3.5.4**. [Details](reports/k8s-cve/latest.md)
- **fortiguard-cve** - four FortiOS PSIRT advisories (2026-11-10) list FortiOS 7.4.11 as affected - FG-IR-26-268 / CVE-2026-126254 (9.6), FG-IR-26-267 / CVE-2026-126253 (9.3), FG-IR-26-273 / CVE-2026-126259 (9.0), FG-IR-26-269 / CVE-2026-126255 (7.4): upgrade fw-maniak-hq from **v7.4.11 to 7.4.13**. [Details](reports/fortiguard-cve/latest.md)
- **k8s-cve** - unpatched on the same cluster: flannel `v0.27.2` CVE-2026-32241 (cross-node RCE, fix 0.28.2), local-path-provisioner `v0.0.31` (CVE-2025-62878 9.9 / CVE-2026-44543, fix >= v0.0.36), CoreDNS `v1.12.3` (12 CVEs, >= v1.14.7 clears all), containerd `2.2.7` (3 CVEs, fix 2.2.9); the Talos `v1.13.10 -> 1.14.x` step covers flannel + containerd and the `kube-proxy:v1.34.11` skew. [Details](reports/k8s-cve/latest.md)
- **kagent-updates** - nginx HTTP/2 RCE fix [#3730](https://github.com/kagent-dev/kagent/pull/3730) (UI base 1.31.8 -> 1.31.9, CVE-2026-123633 / CVE-2026-123007) is on `main` only: backport [#3732](https://github.com/kagent-dev/kagent/pull/3732) to `release/v1.0.x` is still blocked, so Enterprise `alpha12` ships nginx 1.31.8 - rebuild the UI image or keep HTTP/2 off. [Details](reports/kagent-updates/latest.md)

### Worth knowing

- **agentgateway-updates** - prompt-guard policies moved to top level ([#4387](https://github.com/agentgateway/agentgateway/pull/4387)): the CRD shape change is in `main` with per-type CEL validation, nested fields only deprecated for now (real size win comes with [#4205](https://github.com/agentgateway/agentgateway/pull/4205)) - update demo YAML that nests policies. [Details](reports/agentgateway-updates/latest.md)
- **agentgateway-updates** - model catalog moved to models.dev `experimental/v2.0/api.json` ([#4457](https://github.com/agentgateway/agentgateway/pull/4457), [#4447](https://github.com/agentgateway/agentgateway/pull/4447)) so quoted rates shift - re-screenshot the cost dashboard; the UI is now antd ([#4409](https://github.com/agentgateway/agentgateway/pull/4409), +16% gzipped JS+CSS), so deck screenshots are stale. Still no 1.8 tag; newest artifact is the Oct 27 v1.7.0 cut. [Details](reports/agentgateway-updates/latest.md)
- **agentgateway-updates** - open **breaking** PR [#4430](https://github.com/agentgateway/agentgateway/pull/4430) changes the default `agentgateway.user` (drops `apiKey.name`), splitting request-log history per person across an upgrade - pin `standardAttributes.user` for old behaviour. [Details](reports/agentgateway-updates/latest.md)
- **kagent-updates** - no releases anywhere (7th day), only 5 merged PRs org-wide; your blog post shipped ([website #722](https://github.com/kagent-dev/website/pull/722)) and substrate's IPv6 actor-networking series ([#185](https://github.com/kagent-dev/substrate/pull/185) et al.) is open, unmerged. [Details](reports/kagent-updates/latest.md)
- **kagent-updates** - breaking-ish for Python BYO on Substrate: [#3729](https://github.com/kagent-dev/kagent/pull/3729) moves the entry point `uvicorn.run(app.build(), ...)` -> `app.build().run()`, gRPC to port 80 and `/readyz` to 8081 - re-test when it lands. [Details](reports/kagent-updates/latest.md)
- **fortigate-health** - verdict HEALTHY (CPU 0%, memory 38%), but disk is a *missing measurement*, not a proven-healthy one: the `disk` counter returns 0 and `log_disk_status: not_available`, with FortiCloud as the only log destination - confirm out-of-band. [Details](reports/fortigate-health/latest.md)

### Report status

| Report | Status | Verdict | Today | History |
| --- | --- | --- | --- | --- |
| [k8s-cve](reports/k8s-cve/latest.md) | ACTION | ACTION NEEDED - exploitable advisories in Argo CD v3.5.3 (CVSS 10.0/9.9), plus unpatched flannel, local-path-provisioner, containerd and 12 CoreDNS CVEs | [2026-11-19](reports/k8s-cve/2026/2026-11-19.md) | [all](reports/k8s-cve/) |
| [fortiguard-cve](reports/fortiguard-cve/latest.md) | ACTION | ACTION NEEDED - four FortiOS PSIRT advisories list 7.4.11 as affected (three CRITICAL), all fixed in 7.4.13; upgrade fw-maniak-hq | [2026-11-19](reports/fortiguard-cve/2026/2026-11-19.md) | [all](reports/fortiguard-cve/) |
| [kagent-updates](reports/kagent-updates/latest.md) | ACTION | No releases (7th day), 5 merges; nginx HTTP/2 RCE fixed on main but the alpha12 backport is blocked; Python BYO entry point changes | [2026-11-19](reports/kagent-updates/2026/2026-11-19.md) | [all](reports/kagent-updates/) |
| [agentgateway-updates](reports/agentgateway-updates/latest.md) | WATCH | 17 merges in 36h: prompt-guard CRD reshuffle merged, catalog on models.dev v2 pricing, UI on antd; still no 1.8 tag | [2026-11-19](reports/agentgateway-updates/2026/2026-11-19.md) | [all](reports/agentgateway-updates/) |
| [fortigate-health](reports/fortigate-health/latest.md) | OK | HEALTHY - no resource counter above 80%; every reported metric below 50% (CPU 0%, memory 38%) on the 80F | [2026-11-19](reports/fortigate-health/2026/2026-11-19.md) | [all](reports/fortigate-health/) |
| redteam-posture | MISSING | - | - | - |

Audit: [reports/_status/latest.md](reports/_status/latest.md) - Past digests: [reports/_digest/](reports/_digest/)

## How it works

| Report | Agent | Schedule (America/Toronto) | Sources |
| --- | --- | --- | --- |
| `redteam-posture` | `redteam-posture` | daily 07:00 (paused) | read-only attack-path review of omni-viper |
| `agentgateway-updates` | `oss-update-watcher` | daily 08:00 | github.com/agentgateway: releases, merged PRs |
| `kagent-updates` | `oss-update-watcher` | daily 08:20 | github.com/kagent-dev: releases, merged PRs |
| `fortigate-health` | `fortigate` | daily 09:00 | FortiGate `fg_resource_usage`, `fg_system_status` |
| `k8s-cve` | `k8s-cve-checker` | daily 09:15 | Kubernetes version + images -> OSV.dev |
| `fortiguard-cve` | `fortigate-cve-watch` | daily 09:30 | FortiOS version -> FortiGuard PSIRT + Outbreak Alerts |
| `_status` | `agent-summary-auditor` | daily 10:30 | this repo: did every report arrive today? |
| `_digest` + this README | `agent-summary-digest` | daily 10:45 | all of the above |

Layout: `reports/<report>/latest.md` is the newest run; `reports/<report>/YYYY/YYYY-MM-DD.md` keeps one
file per day. Agents are read-only on the systems they inspect; the only thing they write is this repo.
Manifests: `apps/kagent/` in the viper-env repo.
