# _status - 2026-11-25

1 of 7 reports missing: redteam-posture (MISSING — no reports/redteam-posture/ directory)

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 (agent redteam-posture) | MISSING | – | No `reports/redteam-posture/` directory; no latest.md and no 2026/2026-11-25.md, so no verdict was published |
| agentgateway-updates | 08:00 (agent oss-update-watcher) | OK | 2026-11-25 | Quiet-code Monday, heavy hardening day: 45 PRs merged in ~33h (18 `agentgateway`, 27 `website`) — loopback rejection for dynamic backends, ReferenceGrant enforcement on `AgentgatewayBackend` refs (soft breaking), per-request MCP session rebuild, Gemini/Anthropic token-count and non-SSE compat. Still no v1.8 tag; `main`/`:latest-dev` carries the demo features. |
| kagent-updates | 08:20 (agent oss-update-watcher) | OK | 2026-11-25 | Three releases in one day (kagent v0.10.7, tools v0.3.2, substrate v1.1.0-alpha1 published as a release object); 19 PRs merged org-wide, headlined by a kagent UI wave (forms overhaul, `/mcp` rename, breaking chat→conversation rename) and doc2vec's Zendesk OAuth merge — but substrate #192 is still blocked, so the non-`ate-system` installer bug is live on omni-viper. |
| agentdesktop-updates | 08:40 (agent oss-update-watcher) | OK | 2026-11-25 | **v0.7.0 shipped this morning** — the whole breaking v0.7 set that had been `main`-only is now a release: gateway JWKS on its own listener (#240), loopback-only controller authn/authz (#220), enrollment tokens (#267), fleet limits (#237/#261), required `controller.caCertificatePath` (#234), OpenCode 2 credential plugin (#229), plus the Rosetta-2 PKG fix (#293). 2 PRs merged since yesterday's report; no new releases in `website`. |
| fortigate-health | 09:00 (agent fortigate) | OK | 2026-11-25 | **Verdict: HEALTHY** — all resource counters well under 80%; nothing to flag. |
| k8s-cve | 09:15 (agent k8s-cve-checker) | OK | 2026-11-25 | **Verdict: ACTION NEEDED** — CoreDNS 1.12.3 (10 HIGH advisories), Flannel 0.27.2 (HIGH cross-node RCE, fixed 0.28.2) and Argo CD v3.5.3 (1 CRITICAL + 1 HIGH, fixed 3.5.4) all have published fixes; patch these three first. |
| fortiguard-cve | 09:30 (agent fortigate-cve-watch) | OK | 2026-11-25 | **Verdict: ACTION NEEDED** — 4 FortiOS advisories published in the last 30 days list FortiOS 7.4.0 through 7.4.12 as affected, which includes the running v7.4.11 build 2878 on fw-maniak-hq. Three are CRITICAL (CVSS 9.0–9.6). The fix for all four is FortiOS 7.4.13. |

Action: redteam-posture — no `reports/redteam-posture/` directory exists at all (no `latest.md`, no `2026/2026-11-25.md`); check the schedule's execution history in the kagent UI (agent `redteam-posture`, due 07:00 America/Toronto) and re-run — sixth consecutive day absent, and this schedule has never produced a file in this repo.

Note: informational only (today's runs are all OK) — `kagent-updates` still has no `2026/2026-11-24.md` in its archive (yesterday's STALE run was never back-filled), and `k8s-cve` has no `2026/2026-11-20.md`; worth confirming whether those days' runs failed or the files were simply never published.
