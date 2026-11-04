# _status - 2026-11-04

1 of 5 reports missing: kagent-updates

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| agentgateway-updates | 08:00 | OK | 2026-11-04 | Quiet day: **no new release and no new merged PRs since the 09:46 ET run earlier today** - agentgateway is still v1.7.0 (2026-10-27) with no 1.8 RC open; only in-flight open work since then (extensible model sources #4292, budget-keyed-by-id fix #4284, GCP backendAuth tokens #4279). |
| kagent-updates | 08:20 | MISSING | - | - |
| fortigate-health | 09:00 | OK | 2026-11-04 | **Verdict: HEALTHY** - all resource counters are far below the 80% alert threshold. Nothing to flag. |
| k8s-cve | 09:15 | OK | 2026-11-04 | **Verdict: ACTION NEEDED** - Argo CD v3.5.3 is affected by a CRITICAL AppProject-restriction bypass (CVE-2026-77459, fixed in 3.5.4), CoreDNS v1.12.3 carries multiple HIGH advisories, and flannel v0.27.2 has a HIGH cross-node RCE (CVE-2026-32241). |
| fortiguard-cve | 09:30 | OK | 2026-11-04 | **Verdict: Review** - two FortiOS advisories were published in the last 30 days (FG-IR-26-181 GUI DoS, FG-IR-26-182 double free), but neither feed entry states affected FortiOS version ranges, so confirm on the advisory page whether FortiOS 7.4.11 is fixed; no actively-exploited FortiOS / FortiGate / FortiAP vulnerability and no Fortinet-product Outbreak Alert in the window. |

Action: kagent-updates (agent `oss-update-watcher`, schedule 08:20 America/Toronto) - no `reports/kagent-updates/` directory exists on `main`: neither `latest.md` nor `2026/2026-11-04.md`. The only commits ever touching that path are a `probe` commit (`5fedbea`, 14:45 UTC) and a later "remove probe file written by kagent-updates run" commit (`a981f25`, 14:53 UTC), i.e. the schedule reached the repo but produced no report. Check the schedule's execution history in the kagent UI (same `oss-update-watcher` agent that did deliver `agentgateway-updates` today, so the agent itself is healthy - suspect the kagent-updates schedule entry / its target repo path).

---

Audited at 2026-11-04 10:30 America/Toronto (EST) against branch `main` (head `a981f25`). Directories found under `reports/`: `_status`, `agentgateway-updates`, `fortigate-health`, `fortiguard-cve`, `k8s-cve` - no extra/unexpected report directories. For every OK report both `latest.md` and `2026/2026-11-04.md` exist, and each first line matches `# <report> - YYYY-MM-DD` dated today. Supersedes the 09:13 ET status run, which reported `k8s-cve` and `fortiguard-cve` as MISSING before their 09:15 / 09:30 slots; both have since landed. Both copies of this status (`latest.md` and `2026/2026-11-04.md`) carry identical content. No path outside `reports/_status/` was modified.
