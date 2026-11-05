# _status - 2026-11-05

1 of 6 reports missing: redteam-posture

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 | MISSING | - | - |
| agentgateway-updates | 08:00 | OK | 2026-11-05 | Busy 24h: custom model-source plugin points merged, a new compression policy and "UI under any path prefix" landed, Bedrock moved to native Runtime + Converse fallback - **15 PRs merged across 2 repos**, still no release since v1.7.0. |
| kagent-updates | 08:20 | OK | 2026-11-05 | kagent **v1.0.0-rc2** and **v0.10.5** shipped, Agent Substrate **v1.0.0-rc1** is the new pin (mTLS actor ingress), tools **0.3.1** cleared 148 CVE findings; ~76 merged PRs across kagent, substrate, tools, doc2vec and website in the last 7 days. |
| fortigate-health | 09:00 | OK | 2026-11-05 | **Verdict: HEALTHY** - nothing above 80%; CPU 0%, memory 38%, disk 0%, sessions 782 (24 h peak 2215), setup rate 23/s. |
| k8s-cve | 09:15 | OK | 2026-11-05 | **Verdict: ACTION NEEDED** - Argo CD v3.5.3 carries a CRITICAL AppProject-restriction bypass (CVE-2026-77459, fixed in 3.5.4), and CoreDNS 1.12.3, flannel v0.27.2, the Argo Redis 8.2.3 and the nginx:1.27-alpine TLS sidecar are all multiple releases behind their fixed versions. |
| fortiguard-cve | 09:30 | OK | 2026-11-05 | **Verdict: REVIEW** - no actively exploited Fortinet vulnerability in the last 30 days, but two new FortiOS advisories (FG-IR-26-181, FG-IR-26-182, published 2026-10-13) do not state their affected version ranges in the feed text, so they must be confirmed against the advisory pages before FortiOS 7.4.11 can be called unaffected. |

Action: redteam-posture (agent `redteam-posture`, schedule 07:00 America/Toronto) - no `reports/redteam-posture/` directory exists on `main`: neither `latest.md` nor `2026/2026-11-05.md`, and no commit has ever touched that path, so the schedule never published anything (not a late run - the 07:00 slot is 3.5 h past). Check the schedule's execution history in the kagent UI and confirm the schedule exists / is enabled and points at this repo path; the other four schedules all landed their 2026-11-05 files, so the publishing pipeline itself is healthy.

---

Audited at 2026-11-05 10:30 America/Toronto (EST) against branch `main` (head `176ebdc`). Directories found under `reports/`: `_status`, `agentgateway-updates`, `fortigate-health`, `fortiguard-cve`, `k8s-cve`, `kagent-updates` - no extra/unexpected report directories beyond the scheduled list. For every OK report both `latest.md` and `2026/2026-11-05.md` exist, and each first line matches `# <report> - YYYY-MM-DD` dated today. Note: `reports/kagent-updates/2026/` contains only `2026-11-05.md` (its first dated archive); today's requirement is met. Both copies of this status (`latest.md` and `2026/2026-11-05.md`) carry identical content. No path outside `reports/_status/` was modified.
