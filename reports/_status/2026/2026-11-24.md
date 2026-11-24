# _status - 2026-11-24

2 of 7 reports missing: redteam-posture (MISSING), kagent-updates (STALE — latest is 2026-11-23, no file for today)

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 (agent redteam-posture) | MISSING | – | No reports/redteam-posture/ directory; no latest.md and no 2026/2026-11-24.md |
| agentgateway-updates | 08:00 (agent oss-update-watcher) | OK | 2026-11-24 | The code repo finally moved: 13 PRs merged into agentgateway in ~12 hours (multi-endpoint standalone MCP #4473, in-tree SSE decoder replacing tokio_sse_codec #4502, Responses→Chat-Completions image/file translation #4529, Claude-Code-on-Bedrock beta-header fix #4420, plus strict backendAuth parsing — soft breaking). Still no v1.8 tag; the release-note roll-up is up to ~28 entries. |
| kagent-updates | 08:20 (agent oss-update-watcher) | STALE | 2026-11-23 | Dead day for merges: zero releases and zero merged PRs org-wide since 11-22 — but substrate #192 is a real omni-viper installer bug (atelet never registers off ate-system), four CRD/API PRs are queued on kagent main, and doc2vec's Zendesk OAuth PR left draft. (yesterday's verdict — today's run produced nothing) |
| agentdesktop-updates | 08:40 (agent oss-update-watcher) | OK | 2026-11-24 | Two PRs merged — #294 fixed the two RustSec advisories that had turned cargo-deny red on main (cookie + url), and #286 documented that deleting a token-enrolled device does not keep it out; plus #288 (single browser sign-in) ready for review, #290 limit-hit metrics listener, #293 fixes the arm64 PKG needing Rosetta 2. No new releases; v0.6.0 still latest. |
| fortigate-health | 09:00 (agent fortigate) | OK | 2026-11-24 | Verdict: HEALTHY — no counter above 80%; CPU idle, memory ~39% used, sessions and setup rate well within limits. |
| k8s-cve | 09:15 (agent k8s-cve-checker) | OK | 2026-11-24 | Verdict: ACTION NEEDED — CoreDNS 1.12.3 (12 unfixed CVEs, mostly HIGH), Flannel 0.27.2 (HIGH, cross-node RCE class), and Argo CD v3.5.3 (1 CRITICAL + 1 HIGH, both patched in 3.5.4) require upgrades; Kubernetes 1.36.4 and cert-manager 1.21.2 are clean in OSV. |
| fortiguard-cve | 09:30 (agent fortigate-cve-watch) | OK | 2026-11-24 | Verdict: ACTION NEEDED — 4 FortiOS advisories published in the last 30 days affect FortiOS 7.4.11 (our running version); the fixed release for all four is FortiOS 7.4.13, so plan an upgrade of fw-maniak-hq. |

Action: redteam-posture — no reports/redteam-posture/ directory exists at all (no latest.md, no 2026/2026-11-24.md); check the schedule's execution history in the kagent UI (agent redteam-posture, due 07:00 America/Toronto) and re-run — another consecutive day absent, and this schedule has never produced a file in this repo.

Action: kagent-updates — latest.md is still dated 2026-11-23 and reports/kagent-updates/2026/2026-11-24.md does not exist; check the schedule's execution history in the kagent UI (agent oss-update-watcher, due 08:20 America/Toronto) and re-run. The sibling oss-update-watcher schedules at 08:00 and 08:40 both landed, so this is a per-schedule failure rather than the agent being down.

Note: informational only (today's runs are OK) — k8s-cve has no 2026/2026-11-20.md in its archive; worth confirming whether that day's run failed or the file was never published.
