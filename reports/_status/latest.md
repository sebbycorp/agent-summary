# _status - 2026-11-07

1 of 6 reports missing: redteam-posture

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 | MISSING | - | - |
| agentgateway-updates | 08:00 | OK | 2026-11-07 | Protocol-compatibility day: the OpenAPI MCP bridge finally negotiates `initialize` (unblocks Copilot Studio), a2a agent-card paths stop doubling, gRPC local-500s become retryable - **18 PRs merged across 2 repos on Nov 6, still no release since v1.7.0**. |
| kagent-updates | 08:20 | OK | 2026-11-07 | **Agent Substrate shipped v1.0.0 GA and kagent v1.0.0-rc3 landed with the full split-atelet upgrade runbook** - this is the pair to run omni-viper on for the Enterprise alpha12 cut; 19 PRs merged across kagent, substrate and website since yesterday's digest. |
| fortigate-health | 09:00 | OK | 2026-11-07 | **Verdict: HEALTHY** - CPU 0%, memory 38%, disk 0%, sessions ~1k (24h peak 2,475). No counter above 80%; nothing to flag. |
| k8s-cve | 09:15 | OK | 2026-11-07 | **Verdict: ACTION NEEDED** - 1 CRITICAL and several HIGH findings on in-use components (Argo CD 3.5.3, CoreDNS 1.12.3, Flannel 0.27.2), all with fixed versions available; no exploited/unfixed criticals on the control plane itself. |
| fortiguard-cve | 09:30 | OK | 2026-11-07 | **Verdict: REVIEW** - one low-severity advisory (FG-IR-26-182 / CVE-2026-106175) does affect FortiOS 7.4.x including our running 7.4.11 build 2878, but it is a local-authenticated CLI DoS with no published FortiOS 7.4 fix branch and no known exploitation; no critical/high FortiOS, FortiGate or FortiAP exposure in the last 30 days. |

Action: redteam-posture (agent `redteam-posture`, schedule 07:00 America/Toronto) - check the schedule's execution history in the kagent UI. `reports/redteam-posture/` does not exist on `main` (no `latest.md`, no `2026/2026-11-07.md`) and no commit in the repo history has ever touched that path, so this is not a late run: the schedule has never published (also MISSING on 2026-11-04, 2026-11-05 and 2026-11-06). Confirm the schedule exists, is enabled, and points at this repo/branch and path.

Action: k8s-cve is drifting against its 09:15 slot - published 09:39 and re-published 09:43 EST (+28 min), the same pattern as yesterday's +27 min. Data is complete today, but check the schedule's run duration/queueing in the kagent UI before the drift pushes a future run past the audit.

---

Audited at 2026-11-07 10:33 America/Toronto (EST; DST ended Sun 2026-11-01, so the zone is UTC-5) against branch `main` (head `674f8e3`). Directories found under `reports/`: `_status`, `agentgateway-updates`, `fortigate-health`, `fortiguard-cve`, `k8s-cve`, `kagent-updates` - no extra/unexpected report directories beyond the scheduled list, and `redteam-posture` is absent entirely. For each of the 5 OK reports both `latest.md` and `2026/2026-11-07.md` exist, each first line matches `# <report> - YYYY-MM-DD` dated today, and each `latest.md` SHA matches its dated archive file. Today's commits: agentgateway-updates 08:13, kagent-updates 08:28, fortigate-health 09:01, k8s-cve 09:39 + 09:43, fortiguard-cve 09:44 EST. Both copies of this status (`reports/_status/latest.md` and `reports/_status/2026/2026-11-07.md`) carry identical content. No path outside `reports/_status/` was modified.
