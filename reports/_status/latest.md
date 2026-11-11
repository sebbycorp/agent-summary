# _status - 2026-11-11

1 of 6 reports missing: redteam-posture

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 | MISSING | - | - |
| agentgateway-updates | 08:00 | OK | 2026-11-11 | Still **no new tag - v1.7.0 (Oct 27) remains the newest artifact**, but core landed the fix that makes **Anthropic fast-mode billing correct (2x undercount)** and a **Go 1.27.4** bump, website finally documented the **Docker `localhost` tracing trap**, and the catalog picked up **Gemini 4 Carbon** + **Meta Muse Ember** - **6 PRs merged since the 11-10 report (4 core, 2 website)**. |
| kagent-updates | 08:20 | OK | 2026-11-11 | **No new releases anywhere - kagent still v1.0.0 GA - but main moved: harness/sandbox images jump to Alpine 3.24 (`#3618`), current Gemini IDs enter the model picker (`#3525`), and the docs finally catch up to 1.0 with a UI Policies page, A2A `ListTasks`, Foundry-Claude and PDB/`extraObjects` guidance (`website#719`). 15 PRs merged org-wide, 11 new since yesterday (kagent 5, website 6).** |
| fortigate-health | 09:00 | OK | 2026-11-11 | **Verdict: HEALTHY** - all monitored resources are well below the 80% alerting threshold; nothing to flag. (Disk utilization is not assessable: `log_disk_status: not_available`, logs are going off-box to FortiCloud.) |
| k8s-cve | 09:15 | OK | 2026-11-11 | **Verdict: ACTION NEEDED - 2 CRITICAL and several HIGH findings with fixes available (Argo CD 3.5.3, Flannel 0.27.2, local-path-provisioner 0.0.31).** |
| fortiguard-cve | 09:30 | OK | 2026-11-11 | **Verdict: ACTION NEEDED** - FortiOS v7.4.11 build 2878 on fw-maniak-hq (FGT80F, FGT80FTK22061709) is listed as affected by FG-IR-26-267 (CVSS 9.3, CVE-2026-126253) and FG-IR-26-269 (CVSS 7.4, CVE-2026-126255); both fixed in FortiOS 7.4.13 - plan an upgrade to 7.4.13+ (read-only check, nothing changed on the firewall). |

Action: redteam-posture (agent `redteam-posture`, schedule 07:00 America/Toronto) - check the schedule's execution history in the kagent UI. `reports/redteam-posture/` still does not exist on `main` (no `latest.md`, no `2026/2026-11-11.md`) and no commit has ever touched that path, so this is not a late run - the 07:00 slot is ~3.5 h past and the schedule has never published here. Today is the 7th consecutive MISSING day (2026-11-05 through 2026-11-11). Verify the schedule exists, is enabled, and targets this repo / `main` / `reports/redteam-posture/`, then re-run it manually once fixed.

Action (advisory, no data impact): all five schedules that ran published late again, and the drift widened vs yesterday for the two OSS watchers - agentgateway-updates 08:05:50 (+6 min vs its 08:00 slot), kagent-updates 08:34:45 (+15 min, was +8 min), fortigate-health 09:01:46 (+2 min), k8s-cve 09:31:14 (+16 min), fortiguard-cve 09:38:44 (+9 min). k8s-cve and kagent-updates are the consistent worst offenders; check run duration / queueing in the kagent UI before the drift grows.

---

Audited at 2026-11-11 10:32 America/Toronto (EST; DST ended Sun 2026-11-01, so the zone is UTC-5) against branch `main` (head `a2f2400`, the fortiguard-cve commit). Directories found under `reports/`: `_status`, `agentgateway-updates`, `fortigate-health`, `fortiguard-cve`, `k8s-cve`, `kagent-updates` - no extra/unexpected report directories beyond the scheduled list, and `redteam-posture` is absent entirely. For each of the 5 OK reports both `latest.md` and `2026/2026-11-11.md` exist and each `latest.md` first line matches `# <report> - 2026-11-11`, so no STALE or MALFORMED cases. Today's report commits (America/Toronto): agentgateway-updates 08:05:50 (`572d5b2`), kagent-updates 08:34:45 (`d32f339`), fortigate-health 09:01:46 (`d3943a3`), k8s-cve 09:31:14 (`6e7bb78`), fortiguard-cve 09:38:44 (`a2f2400`). Both copies of this status (`reports/_status/latest.md` and `reports/_status/2026/2026-11-11.md`) carry identical content. No path outside `reports/_status/` was modified.
