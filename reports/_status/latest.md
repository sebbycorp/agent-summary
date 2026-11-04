# _status - 2026-11-04

2 of 3 reports missing: k8s-cve, fortiguard-cve

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| fortigate-health | 09:00 | OK | 2026-11-04 | HEALTHY - all resource counters are far below the 80% alert threshold. Nothing to flag. |
| k8s-cve | 09:15 | MISSING | - | - |
| fortiguard-cve | 09:30 | MISSING | - | - |

Action: k8s-cve (agent `k8s-cve-checker`) - no `reports/k8s-cve/` directory exists in this repo and no commit from that agent has ever landed; check the schedule's execution history in the kagent UI (manifest `agent-cve-watchers.yaml` in viper-env). Caveat: this audit ran at 09:13 America/Toronto, before the 09:15 slot, so a late/missed run cannot be distinguished yet - re-check after 09:15 before escalating.

Action: fortiguard-cve (agent `fortigate-cve-watch`) - no `reports/fortiguard-cve/` directory exists and no commit from that agent has ever landed; check the schedule's execution history in the kagent UI (manifest `agent-cve-watchers.yaml` in viper-env). Caveat: this audit ran at 09:13 America/Toronto, before the 09:30 slot, so a late/missed run cannot be distinguished yet - re-check after 09:30 before escalating.

---

Audited at 2026-11-04 09:13 America/Toronto (EST) against branch `main`. Reports found under `reports/`: fortigate-health only; no extra report directories present. Both copies of this status (`latest.md` and `2026/2026-11-04.md`) carry identical content.
