# _status - 2026-11-19

1 of 6 reports missing: redteam-posture

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 (agent redteam-posture) | MISSING | – | No `reports/redteam-posture/` directory; no latest.md and no dated file |
| agentgateway-updates | 08:00 (agent oss-update-watcher) | OK | 2026-11-19 | Prompt-guard CRD reshuffle merged (#4387), model catalog moved to models.dev v2 pricing, new CEL `recurse()`; 17 merges in 36h, still no 1.8 tag |
| kagent-updates | 08:20 (agent oss-update-watcher) | OK | 2026-11-19 | No releases anywhere (7th day), only 5 merges org-wide; nginx HTTP/2 RCE fixed on `main` but backport #3732 to `release/v1.0.x` still open |
| fortigate-health | 09:00 (agent fortigate) | OK | 2026-11-19 | Verdict: HEALTHY — no resource counter above 80%; every reported metric sits below 50% |
| k8s-cve | 09:15 (agent k8s-cve-checker) | OK | 2026-11-19 | Verdict: ACTION NEEDED — Argo CD v3.5.3 CVSS 10.0/9.9 (fix in 3.5.4), plus unpatched flannel, containerd, local-path-provisioner and CoreDNS CVEs on omni-viper |
| fortiguard-cve | 09:30 (agent fortigate-cve-watch) | OK | 2026-11-19 | Verdict: ACTION NEEDED — four FortiOS PSIRT advisories (2026-11-10, three CRITICAL) affect 7.4.11; upgrade fw-maniak-hq to 7.4.13 |

Action: redteam-posture — no directory or file ever published under `reports/redteam-posture/`; check the schedule's execution history in the kagent UI (agent `redteam-posture`, due 07:00 America/Toronto).
