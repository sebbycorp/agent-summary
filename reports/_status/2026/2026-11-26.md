# _status - 2026-11-26

2 of 7 reports missing: redteam-posture (MISSING — no reports/redteam-posture/ directory), k8s-cve (STALE — latest.md dated 2026-11-25 and no 2026/2026-11-26.md)

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 (agent redteam-posture) | MISSING | – | No `reports/redteam-posture/` directory; no `latest.md` and no `2026/2026-11-26.md`, so no verdict was published |
| agentgateway-updates | 08:00 (agent oss-update-watcher) | OK | 2026-11-26 | **Thanksgiving Thursday went quiet: zero merged PRs and zero commits to `main` in the ~24h window — `agentgateway` HEAD is still `134cbf02` (#4556, 11-24T23:22Z). No releases (still v1.7.0 from Oct 27). What moved is the *incoming* queue: 9 new PRs opened (Vertex embeddings routing, telemetry-on-drain, BackendTLSPolicy status, Anthropic structured-output schemas) and the auth-hardening pair #4545/#4394 is still active.** |
| kagent-updates | 08:20 (agent oss-update-watcher) | OK | 2026-11-26 | **Quiet day upstream: zero new releases and zero merged PRs in kagent or substrate — only two website merges (#763 MCP-tasks blog post, #764 homepage install bar); substrate #192 is still `blocked`, so the non-`ate-system` installer bug remains live on omni-viper.** |
| agentdesktop-updates | 08:40 (agent oss-update-watcher) | OK | 2026-11-26 | **Quiet day upstream — zero merged PRs, zero new releases since yesterday's v0.7.0; `main` is still `2b5a7e5` (#293).** All the action is in the open queue, and it is queue day for the agentgateway path: new draft #299 fixes Copilot CLI and Claude Code *version discovery* plus the gateway credential-helper socket, and #289/#287 both advanced on top of v0.7.0. |
| fortigate-health | 09:00 (agent fortigate) | OK | 2026-11-26 | **Verdict: HEALTHY** — all resource counters are far below the 80% alert threshold; nothing to flag. |
| k8s-cve | 09:15 (agent k8s-cve-checker) | STALE | 2026-11-25 | **Verdict: ACTION NEEDED** — CoreDNS 1.12.3 (10 HIGH advisories), Flannel 0.27.2 (HIGH cross-node RCE, fixed 0.28.2) and Argo CD v3.5.3 (1 CRITICAL + 1 HIGH, fixed 3.5.4) all have published fixes; patch these three first. *(carried over from yesterday — not today's run)* |
| fortiguard-cve | 09:30 (agent fortigate-cve-watch) | OK | 2026-11-26 | **VERDICT: ACTION NEEDED — of the 7 FortiOS/FortiGate-related advisories published in the last 30 days, 4 affect the running FortiOS v7.4.11 build 2878 (three of them CVSS 9.0+); the single fix is FortiOS 7.4.13.** |

Action: redteam-posture — no `reports/redteam-posture/` directory exists at all (no `latest.md`, no `2026/2026-11-26.md`); check the schedule's execution history in the kagent UI (agent `redteam-posture`, due 07:00 America/Toronto) and re-run — seventh consecutive day absent, and this schedule has never produced a file in this repo.

Action: k8s-cve — today's 09:15 run did not publish; `latest.md` still holds the 2026-11-25 report and `2026/2026-11-26.md` is absent; check the schedule's execution history in the kagent UI (agent `k8s-cve-checker`) and re-run. Yesterday's run was OK, so this is a fresh break, not a long-running failure — and yesterday's unpatched items (Argo CD v3.5.3 CRITICAL, CoreDNS 1.12.3, Flannel 0.27.2) are still unverified as of today.

Note: archive gaps carried over from previous audits — `kagent-updates` still has no `2026/2026-11-24.md`, and `k8s-cve` has no `2026/2026-11-20.md`; worth confirming whether those days' runs failed or the files were simply never published. No unexpected directories under `reports/` beyond the 7 scheduled reports (`_status` and `_digest` excluded per policy).
