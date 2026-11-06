# _status - 2026-11-06

1 of 6 reports missing: redteam-posture

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 | MISSING | - | - |
| agentgateway-updates | 08:00 | OK | 2026-11-06 | Hardening day, no shipping: three HTTP/2 connection-pool deadlock/stall fixes landed inside ~90 minutes, GCP `backendAuth` tokens and OTLP access-log bodies merged, Bedrock Mantle got two param fixes - **20 PRs merged across 2 repos, still no release since v1.7.0**. |
| kagent-updates | 08:20 | OK | 2026-11-06 | Agent Substrate raced to **v1.0.0-rc4** with a brand-new `substrate-atelet` chart, kagent main now pins rc4 and installs atelet as its own Helm release, and **kagent v0.10.6** landed - which hard-disables Substrate on the 0.10.x charts; 20 PRs merged across kagent, substrate and website since the last digest. |
| fortigate-health | 09:00 | OK | 2026-11-06 | **Verdict: HEALTHY** - nothing above 80%; CPU 0%, memory 37% (1.38 GiB / 3.63 GiB), disk 0%, sessions 817 (24 h peak 4955), setup rate 9/s. |
| k8s-cve | 09:15 | OK | 2026-11-06 | **Verdict: ACTION NEEDED** - unpatched CRITICAL + HIGH in Argo CD, 12 advisories (10 HIGH) in CoreDNS, a HIGH cross-node RCE in Flannel, and 3 MODERATE in the containerd node runtime; every other mappable component is clean or already past the fix. |
| fortiguard-cve | 09:30 | OK | 2026-11-06 | **Verdict: ACTION NEEDED** - FortiOS 7.4.11 is listed as affected by FG-IR-26-182 (CVE-2026-106175, CVSS 2.1, local-authenticated CLI denial of service, "FortiOS 7.4 all versions"); plan a 7.4.x maintenance release. No remotely exploitable, unauthenticated or in-the-wild FortiOS / FortiGate / FortiAP issue published in the last 30 days. |

Action: redteam-posture (agent `redteam-posture`, schedule 07:00 America/Toronto) - the `reports/redteam-posture/` directory does not exist on `main`: no `latest.md`, no `2026/2026-11-06.md`, and no commit in the repo history has ever touched that path, so the schedule has never published (not a late run - the 07:00 slot is >3 h past, and it was already MISSING in the 2026-11-05 audit). Check the schedule's execution history in the kagent UI: confirm the schedule exists, is enabled, and points at this repo/branch and path.

Action: k8s-cve and fortiguard-cve published OK but late against their slots - k8s-cve committed 09:42 EST (schedule 09:15, +27 min) and fortiguard-cve 09:46 EST (schedule 09:30, +16 min). No data gap, but watch the schedule's run duration/queueing in the kagent UI before the drift pushes a later run past the audit.

---

Audited at 2026-11-06 10:30 America/Toronto (EST) against branch `main` (head `4a9899a`). Directories found under `reports/`: `_status`, `agentgateway-updates`, `fortigate-health`, `fortiguard-cve`, `k8s-cve`, `kagent-updates` - no extra/unexpected report directories beyond the scheduled list. For each OK report both `latest.md` and `2026/2026-11-06.md` exist, and each first line matches `# <report> - YYYY-MM-DD` dated today (latest.md SHAs match the dated archive file in every case). Today's commits: agentgateway-updates 08:11, kagent-updates 08:25, fortigate-health 09:02, k8s-cve 09:42, fortiguard-cve 09:46 EST. Both copies of this status (`reports/_status/latest.md` and `reports/_status/2026/2026-11-06.md`) carry identical content. No path outside `reports/_status/` was modified.
