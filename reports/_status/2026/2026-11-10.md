# _status - 2026-11-10

1 of 6 reports missing: redteam-posture

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 | MISSING | - | - |
| agentgateway-updates | 08:00 | OK | 2026-11-10 | Quiet on the release front (**no new tag — still v1.7.0 from Oct 27**), but the brotli **CVE-2026-111497** fix finally merged, the model catalog gained **Claude Fable 5.2**, and website did a same-day cleanup pass on yesterday's breaking-change docs — **13 PRs merged (2 core, 11 website)**. |
| kagent-updates | 08:20 | OK | 2026-11-10 | **kagent v1.0.0 GA shipped — first stable release, with the new `curl \| bash` → `kagent install` onboarding and default Harnesses; substrate landed declarative install/upgrade values (`#165`, backported in `#167`); doc2vec v2.17.4 published. ~31 PRs merged across 4 repos.** |
| fortigate-health | 09:00 | OK | 2026-11-10 | **Verdict: HEALTHY** — all resource counters well below the 80% threshold; nothing to flag. |
| k8s-cve | 09:15 | OK | 2026-11-10 | **Verdict: ACTION NEEDED** — 23 distinct OSV advisories: 1 CRITICAL + 12 HIGH (Argo CD v3.5.3, flannel v0.27.2, CoreDNS v1.12.3), 7 MODERATE, 3 LOW. Patch now: Argo CD 3.5.3→3.5.4, flannel 0.27.2→0.28.2, CoreDNS 1.12.3→1.14.7, containerd 2.2.7→2.2.9. |
| fortiguard-cve | 09:30 | OK | 2026-11-10 | **Verdict: ACTION NEEDED (low urgency)** — of the two in-window FortiOS advisories, one applies to our running build: FG-IR-26-182 / CVE-2026-106175 lists "FortiOS 7.4 all versions" as *known affected*, so FortiOS 7.4.11 build 2878 on fw-maniak-hq **is affected** — but it is CVSS 2.1 (LOW), local, high-privilege CLI only, and Fortinet publishes no 7.4 fix (migrate to a fixed release / upgrade to 7.6.9+). No unauthenticated, SSL-VPN, or FortiAP advisory applies today. |

Action: redteam-posture (agent `redteam-posture`, schedule 07:00 America/Toronto) - check the schedule's execution history in the kagent UI. `reports/redteam-posture/` still does not exist on `main` (no `latest.md`, no `2026/2026-11-10.md`), so the schedule has never published here - today is the 7th consecutive MISSING day (2026-11-04 through today). Verify the schedule exists, is enabled, and points at this repo / `main` / `reports/redteam-posture/`; re-run it manually once fixed.

Action (advisory, no data impact): all five schedules published late again, though the drift narrowed vs yesterday - agentgateway-updates 08:07 (+7 min vs its 08:00 slot), kagent-updates 08:28 (+8 min), fortigate-health 09:01 (+2 min), k8s-cve 09:31 (+17 min), fortiguard-cve 09:39 (+9 min). k8s-cve is the consistent worst offender (also +23 min on 2026-11-09); check run duration / queueing in the kagent UI before the drift grows.

---

Audited at 2026-11-10 10:30 America/Toronto (EST; DST ended Sun 2026-11-01, so the zone is UTC-5) against branch `main` (head `cf309ab`). Directories found under `reports/`: `_status`, `agentgateway-updates`, `fortigate-health`, `fortiguard-cve`, `k8s-cve`, `kagent-updates` - no extra/unexpected report directories beyond the scheduled list, and `redteam-posture` is absent entirely. For each of the 5 OK reports both `latest.md` and `2026/2026-11-10.md` exist, each first line matches `# <report> - 2026-11-10`, and each `latest.md` blob SHA is identical to its dated archive file (agentgateway-updates `7cdeedd`, kagent-updates `002eb43`, fortigate-health `d9d6a2a`, k8s-cve `c484d75`, fortiguard-cve `5a3d2f5`) - so no STALE or MALFORMED cases. Today's report commits (America/Toronto): agentgateway-updates 08:07 (`7a9b393`), kagent-updates 08:28 (`317b977`), fortigate-health 09:01 (`cae80eb`), k8s-cve 09:31 (`6b121c9`), fortiguard-cve 09:39 (`cf309ab`). Both copies of this status (`reports/_status/latest.md` and `reports/_status/2026/2026-11-10.md`) carry identical content. No path outside `reports/_status/` was modified.
