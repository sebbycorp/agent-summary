# _status - 2026-11-23

1 of 7 reports missing: redteam-posture

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 (agent redteam-posture) | MISSING | – | No `reports/redteam-posture/` directory; no latest.md and no dated file for today |
| agentgateway-updates | 08:00 (agent oss-update-watcher) | OK | 2026-11-23 | Still no code merged into `agentgateway` — `main` untouched at #4501 (11-20T22:04Z) and no releases; the one merge in window is docs (credential-injection Part 4, Eliminating Plaintext), two new code PRs open (HBONE CONNECT host resolution #4516, `backendAuth` short-form credentials #4514), and the 1.8 release-note train has left the station (website#1586). |
| kagent-updates | 08:20 (agent oss-update-watcher) | OK | 2026-11-23 | Dead day for merges: zero releases and zero merged PRs org-wide since 11-22 — but substrate #192 is a real omni-viper installer bug (atelet never registers off `ate-system`), four CRD/API PRs are queued on kagent `main`, and doc2vec's Zendesk OAuth PR left draft. |
| agentdesktop-updates | 08:40 (agent oss-update-watcher) | OK | 2026-11-23 | Second consecutive quiet day upstream: no merged PRs, no new commits to `main`, no releases — `main` still at `b57329c` (#267, 11-20) and v0.6.0 is still the only shipped version; the only movement is two of @peterj's open PRs rebasing this morning, with Copilot resolving conflicts in the DB-backed config editor (#280). |
| fortigate-health | 09:00 (agent fortigate) | OK | 2026-11-23 | Verdict: HEALTHY — no resource counter above 80%; CPU fully idle, memory flat at 38%, sessions and setup rate well within FortiGate 80F limits. |
| k8s-cve | 09:15 (agent k8s-cve-checker) | OK | 2026-11-23 | VERDICT: ACTION NEEDED — 1 CRITICAL (Argo CD 3.5.3) + 3 CRITICAL-class (nginx TLS sidecar), plus HIGH findings in flannel CNI (cross-node RCE), containerd, CoreDNS (x12), Redis (x5) affecting running workloads in `argocd`, `kube-system`, `kagent`, `agentgateway-system`, `ate-system`. |
| fortiguard-cve | 09:30 (agent fortigate-cve-watch) | OK | 2026-11-23 | VERDICT: ACTION NEEDED — 4 FortiOS PSIRT advisories published in the last 30 days explicitly list FortiOS 7.4.0 through 7.4.12 as affected, which includes our running v7.4.11. Three are rated CRITICAL (CVSS 9.0-9.6). All four are fixed in FortiOS 7.4.13 — plan an upgrade. |

Action: redteam-posture — no `reports/redteam-posture/` directory exists at all (no `latest.md`, no `2026/2026-11-23.md`); check the schedule's execution history in the kagent UI (agent `redteam-posture`, due 07:00 America/Toronto) and re-run — this is now the fourth consecutive day the report is absent, and the schedule has never produced a file in this repo.
