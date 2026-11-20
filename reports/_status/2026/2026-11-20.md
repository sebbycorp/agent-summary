# _status - 2026-11-20

2 of 7 reports missing: redteam-posture (MISSING), k8s-cve (STALE — last report 2026-11-19)

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 (agent redteam-posture) | MISSING | – | No `reports/redteam-posture/` directory; no latest.md and no dated file |
| agentgateway-updates | 08:00 (agent oss-update-watcher) | OK | 2026-11-20 | File-access lockdown shipped: config deny/allowlist (#4431) + backend mTLS from pod files with hot reload (#3872); MCP `clientId` works with any IdP (#4477), CoreWeave Serverless added (#4463); 18 merges in ~24h, still no 1.8 tag |
| kagent-updates | 08:20 (agent oss-update-watcher) | OK | 2026-11-20 | No releases and zero merged PRs/commits in `kagent` itself — the window was docs and charts: substrate OpenShift SCC fix (#180), tools PDB rework (#106), website 10 PRs; 12 PRs merged across 3 repos |
| agentdesktop-updates | 08:40 (agent oss-update-watcher) | OK | 2026-11-20 | Gateway-JWKS split (PR #240, breaking) and supply-chain hardening (#256) landed on `main` — no release yet, latest still v0.6.0; 8 PRs merged in `agentdesktop`, none in `website` |
| fortigate-health | 09:00 (agent fortigate) | OK | 2026-11-20 | Verdict: HEALTHY — every resource counter far below the 80% flag line; CPU idle, memory ~38%, sessions and setup rate well within 80F capacity |
| k8s-cve | 09:15 (agent k8s-cve-checker) | STALE | 2026-11-19 | (from 2026-11-19) Verdict: ACTION NEEDED — Argo CD v3.5.3 CVSS 10.0/9.9 (fix 3.5.4), plus unpatched flannel, containerd, local-path-provisioner and CoreDNS CVEs on omni-viper |
| fortiguard-cve | 09:30 (agent fortigate-cve-watch) | OK | 2026-11-20 | Verdict: ACTION NEEDED — FortiOS 7.4.11 on fw-maniak-hq affected by 3 CRITICAL and 1 HIGH PSIRT advisory (all fixed in 7.4.13); plan the 7.4.13 upgrade or apply virtual patches/mitigations |

Action: redteam-posture — no directory or file ever published under `reports/redteam-posture/`; check the schedule's execution history in the kagent UI (agent `redteam-posture`, due 07:00 America/Toronto).
Action: k8s-cve — `latest.md` still shows 2026-11-19 and `reports/k8s-cve/2026/2026-11-20.md` is absent, so the 09:15 run did not publish; check the schedule's execution history in the kagent UI (agent `k8s-cve-checker`) and re-run — yesterday's findings (Argo CD CVSS 10.0, CoreDNS x12) are now 24h old.
