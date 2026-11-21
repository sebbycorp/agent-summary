# _status - 2026-11-21

1 of 7 reports missing: redteam-posture (MISSING)

| Report | Schedule | Status | Report date | Verdict |
| --- | --- | --- | --- | --- |
| redteam-posture | 07:00 (agent redteam-posture) | MISSING | – | No `reports/redteam-posture/` directory; no latest.md and no dated file |
| agentgateway-updates | 08:00 (agent oss-update-watcher) | OK | 2026-11-21 | Pre-1.8 backfill day: long-tail breaking changes landed at once — AWS native endpoints by default (#4300), JWT-email default user (#4430), Azure Content Safety guards in Kubernetes mode (#4205), initial MCP "Skills" (#4214); 23 merges (21 agentgateway, 2 website), still no 1.8 tag |
| kagent-updates | 08:20 (agent oss-update-watcher) | OK | 2026-11-21 | No published releases anywhere; the day was security — nginx 1.31.9 HTTP/2 backport on `release/v1.0.x` (#3732) and v0.10.6 image CVEs fixed (#3738), while `main` re-pinned Substrate to v1.1.0-alpha1 (#3754); 12 PRs merged across 3 repos, zero in substrate |
| agentdesktop-updates | 08:40 (agent oss-update-watcher) | OK | 2026-11-21 | Zero-touch enrollment (#267) and the fleet-limit follow-ups (#261) both merged — still no release, latest remains v0.6.0 so everything on `main` is unreleased; 2 PRs merged in agentdesktop, none in website |
| fortigate-health | 09:00 (agent fortigate) | OK | 2026-11-21 | Verdict: HEALTHY — all resource counters well below the 80% threshold (highest: memory at 39%); no counter hit 80% at snapshot or in 24-h history |
| k8s-cve | 09:15 (agent k8s-cve-checker) | OK | 2026-11-21 | Verdict: ACTION NEEDED — 2 unpatched CRITICAL advisories in in-cluster control-plane components (Argo CD v3.5.3 CVE-2026-77459; local-path-provisioner v0.0.31 CVE-2025-62878), plus unpatched HIGH cross-node RCE in flannel v0.27.2, HIGH in node containerd 2.2.7, and 12 unpatched CVEs in CoreDNS v1.12.3 |
| fortiguard-cve | 09:30 (agent fortigate-cve-watch) | OK | 2026-11-21 | VERDICT: ACTION NEEDED — FortiOS 7.4.11 (build 2878) on fw-maniak-hq is inside the affected range (7.4.0–7.4.12) of four FortiGuard PSIRT advisories published 2026-11-10, three rated CRITICAL (CVSS 9.0 / 9.3 / 9.6); vendor fix is FortiOS 7.4.13, virtual patch exists for the HA flaw |

Action: redteam-posture — no directory or file has ever been published under `reports/redteam-posture/` (nothing in today's commit history either); check the schedule's execution history in the kagent UI (agent `redteam-posture`, due 07:00 America/Toronto) and re-run — this is the second consecutive day that report is absent.
