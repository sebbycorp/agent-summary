# fortiguard-cve - 2026-11-16

**Verdict:** Review needed — 5 FortiOS PSIRT advisories were published 2026-11-10 (two CVSS >= 9.3 routing-daemon flaws plus an SSH/SSL-VPN issue); the feed text does not state the fixed FortiOS releases, so confirm each advisory against FortiOS 7.4.11 before scheduling an upgrade. No Outbreak Alert in the window names FortiOS/FortiGate/FortiAP.

## Device facts (read-only, from fg_system_status)

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| FortiOS | v7.4.11 (build 2878) |
| VDOM | root |
| Load at check time | CPU ~0-1% (idle 100%), memory ~1.5 GiB / 3.9 GiB |

No FortiAP firmware info was available in this session's context (`fg_system_status` reports only the FortiGate), and no FortiAP-specific advisory appeared in the feeds today — FortiAP items: none in the last 30 days.

## Feeds fetched this session

- PSIRT advisories: `https://filestore.fortinet.com/fortiguard/rss/ir.xml` — OK (50 items; 49 dated 2026-11-10, 1 dated 2026-10-26)
- Outbreak Alerts: `https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml` (feed behind https://fortiguard.fortinet.com/outbreak-alert) — OK (20 items; 2 dated within the window)

## Relevant items — last 30 days (2026-10-17 -> 2026-11-16)

| Date | Advisory / Alert | CVE IDs | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 — Numeric truncation error (BGP daemon), CVSS 9.6 | none listed in feed | FortiOS + FortiSASE (bgpd) | check advisory (FortiOS in scope; version range not in feed — exposure only if iBGP is configured) | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness (OSPF daemon), CVSS 9.3 | none listed in feed | FortiOS + FortiSASE (ospfd) | check advisory (FortiOS in scope; version range not in feed — exposure only if OSPF is in use) | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-269 — Improper neutralization in sshd (path traversal -> auth bypass), CVSS 7.4 | none listed in feed | FortiOS, FortiSASE, FortiProxy, FortiPam (SSH daemon) | check advisory (explicitly names FortiOS and FortiProxy; SSH admin access on the 80F makes this the one to triage first) | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 — SSLVPN improper certificate validation (MiTM to backend), CVSS 7.7 | none listed in feed | FortiOS SSL-VPN | check advisory (SSL-VPN named; version range not in feed — relevant only if the SSL-VPN portal is enabled) | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in Remote RADIUS admin two-factor auth, CVSS 5.9 | none listed in feed | FortiOS (GUI admin / RADIUS TFA) | check advisory (FortiOS named; version range not in feed — exposure only with RADIUS-backed admin TFA) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template engine injection, low-priv Device Manager admin -> super admin on managed FortiGates, CVSS 8.4 | none listed in feed | FortiManager / FortiManager Cloud (impacts managed FortiGates) | check advisory (product is FortiManager; treat as indirect risk if this FortiGate is centrally managed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-10 | FG-IR-26-275 — FGFM elevation before registration password verified, CVSS 9.0 | none listed in feed | FortiManager FGFM (FortiGate-to-FortiManager protocol) | check advisory (product is FortiManager; only relevant if this 80F joins a FortiManager over FGFM) | https://fortiguard.fortinet.com/psirt/FG-IR-26-275 |
| 2026-10-26 (rev. 2026-11-13) | FG-IR-26-187 — "Node2Shell" Node.js CVE bundle, CVSS 9.8 | CVE-2026-105482, CVE-2026-105483, CVE-2026-106069, CVE-2026-111497, CVE-2026-105481, CVE-2026-106072, CVE-2026-106074, CVE-2026-106070, CVE-2026-106071, CVE-2026-106073 | Node.js 22.x/24.x/26.x (third-party runtime; feed text names no FortiOS release) | check advisory (feed lists only Node.js versions — verify whether FortiOS 7.4.11 bundles an affected Node.js) | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-10-27 | Outbreak Alert — Node2Shell Remote Code Execution | CVE-2026-111497 | Node.js / Google Brotli decoder (third-party; no Fortinet product named in feed) | check advisory (pairs with FG-IR-26-187 above) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | Outbreak Alert — PHP2Shell Remote Code Execution Vulnerabilities | none listed in feed | PHP application runtimes (no Fortinet product named in feed) | no (not a FortiOS/FortiGate item per the feed text) | https://fortiguard.fortinet.com/outbreak-alert/php2shell |

## Notes / exclusions

- Of the 50 items currently in the PSIRT feed, only **5 name FortiOS** (FG-IR-26-193, -195, -267, -268, -269 above). The remaining items in the 2026-11-10 batch name products this FortiGate does not run and were reviewed and excluded: ~25 FortiManager/FortiAnalyzer (incl. the FortiSASE Sovereign RADIUS item FG-IR-26-247), 12 FortiMail, 5 FortiAuthenticator, and 1 collectoragent SQL-injection item with no product named in the feed (FG-IR-26-266).
- No FortiAP (e.g. FP231F) or FortiProxy SSL-VPN-specific PSIRT advisory appeared in the last 30 days; the only FortiProxy mention is FG-IR-26-269 (shared SSH daemon code) above.
- No Outbreak Alert in the window concerns Fortinet products directly; the two alerts inside the window cover third-party Node.js and PHP runtimes.
- The RSS descriptions do not include the "affected / solved version" table, so every FortiOS row is deliberately marked **check advisory** — open the linked advisory (or its CVRF/CSAF) and compare the solved release against v7.4.11 build 2878 before upgrading. No firewall configuration was read or changed by this check.

---
*Generated by fortigate_cve_watch (read-only). Feeds fetched: ir.xml, outbreakalert.xml on 2026-11-16.*
