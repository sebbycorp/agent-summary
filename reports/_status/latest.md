# _status - 2026-11-22

1 of 7 reports missing: redteam-posture

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 (agent redteam-posture) | MISSING | – | No `reports/redteam-posture/` directory; no latest.md and no dated file for today |
| agentgateway-updates | 08:00 (agent oss-update-watcher) | OK | 2026-11-22 | Quiet weekend: zero merges and zero commits in `agentgateway`, `website`, `benchmarks` and `community` since 2026-11-20T22:04Z - `main` is still at #4501. No releases. All the signal is forward-looking: 8 new open PRs, headlined by request-log retention (4512), JWT key `maxAge` (4142) and the Substrate router/egress rewrite (4505); 1.8 doc drafts now at six. |
| kagent-updates | 08:20 (agent oss-update-watcher) | OK | 2026-11-22 | Quiet day: zero releases and one merged PR org-wide (kagent #3407, prompts-page namespace fix on `release/v0.10.x`); substrate `main` still idle at `fd66419` — but three new kagent PRs opened today all touch CRD/API surface (EgressPolicy `authentications`, RemoteMCPServer TLS CEL, MCP tool disambiguation). |
| agentdesktop-updates | 08:40 (agent oss-update-watcher) | OK | 2026-11-22 | Quiet day: no merges, no commits to `main`, no releases since the 11-21 report — `main` still sits at `b57329c` (#267, 11-20) and v0.6.0 is still the only shipped version; the only new upstream activity is draft PR #288, which fixes the agentgateway browser sign-in storm. |
| fortigate-health | 09:00 (agent fortigate) | OK | 2026-11-22 | Verdict: HEALTHY — no resource counter above 80%; CPU idle, memory at 38%, sessions and setup rate well within FortiGate 80F limits. |
| k8s-cve | 09:15 (agent k8s-cve-checker) | OK | 2026-11-22 | VERDICT: ACTION NEEDED — 1 CRITICAL (Argo CD), multiple HIGH (flannel CNI RCE, containerd, CoreDNS x12, Redis, nginx TLS sidecar) affecting running workloads in `argocd`, `kube-system`, `kagent`, `agentgateway-system`. |
| fortiguard-cve | 09:30 (agent fortigate-cve-watch) | OK | 2026-11-22 | Verdict: ACTION NEEDED — FortiOS 7.4.11 falls inside the affected range of 4 new FortiOS PSIRT advisories (incl. 3 CRITICAL, one publicly exploited); upgrade to FortiOS 7.4.13. |

Action: redteam-posture — still no `reports/redteam-posture/` directory at all (no `latest.md`, no `2026/2026-11-22.md`); check the schedule's execution history in the kagent UI (agent `redteam-posture`, due 07:00 America/Toronto) and re-run — this is now the third consecutive day that report is absent.
