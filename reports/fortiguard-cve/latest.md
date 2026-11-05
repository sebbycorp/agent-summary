# fortiguard-cve - 2026-11-05

**Verdict: REVIEW** — no actively exploited Fortinet vulnerability in the last 30 days, but two new FortiOS advisories (FG-IR-26-181, FG-IR-26-182, published 2026-10-13) do not state their affected version ranges in the feed text, so they must be confirmed against the advisory pages before we can call FortiOS 7.4.11 unaffected.

## Device under watch

| Item | Value | Source |
|---|---|---|
| Hostname | fw-maniak-hq | fg_system_status |
| Model | FortiGate 80F (FGT80F) | fg_system_status |
| Serial | FGT80FTK22061709 | fg_system_status |
| FortiOS | **v7.4.11 build 2878** | fg_system_status |
| VDOM | root | fg_system_status |
| Health | CPU 100% idle (8 cores), mem 1.48 GB / 3.90 GB used, disk status not_available | fg_system_status |

FortiAP: no FortiAP firmware context was supplied for this check and the PSIRT feed contained **no FortiAP items** in the last 30 days, so no FortiAP row is reported.

## Feeds fetched in this session

| Feed | URL | Status |
|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK (49,323 bytes, 50 items, feed lastBuildDate Thu, 05 Nov 2026 14:15:07 +0000) |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | OK (17,411 bytes, 20 items, same lastBuildDate) |

Window applied: pubDate 2026-10-06 → 2026-11-05 (last 30 days). Matches: 13 IR advisories + 3 Outbreak Alerts.

## PSIRT advisories — last 30 days

| Date (pubDate) | Advisory | CVE ids | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-13 | FG-IR-26-181 — GUI Denial of Service (CVSS 6.7, Improper Locking CWE-667) | none listed in feed | **FortiOS** | **check advisory** — feed text has no version range; unauthenticated network attacker with access to the HTTPS mgmt interface | https://fortiguard.fortinet.com/psirt/FG-IR-26-181 |
| 2026-10-13 | FG-IR-26-182 — Double free in signal handler (CVSS 2.1, CWE-415) | none listed in feed | **FortiOS**, FortiProxy, FortiSwitch | **check advisory** — feed text has no version range; local authenticated attacker crashes newcli daemon via crafted CLI commands | https://fortiguard.fortinet.com/psirt/FG-IR-26-182 |
| 2026-10-21 | FG-IR-26-186 — PHP2Shell core RCE patches (CVSS 5.3) | CVE-2026-109618, CVE-2026-109619, CVE-2026-115560 | PHP engine as shipped in Fortinet products | **no** for CVE-2026-115560 — feed states "No Fortinet product meets the conditions required for exploitation… does not affect any Fortinet products". **check advisory** for CVE-2026-109618 / CVE-2026-109619 (feed gives no per-product affected list) | https://fortiguard.fortinet.com/psirt/FG-IR-26-186 |
| 2026-10-26 | FG-IR-26-187 — Node2Shell CVEs (CVSS 9.8) | CVE-2026-105482, CVE-2026-105483, CVE-2026-106069, CVE-2026-111497, CVE-2026-105481, CVE-2026-106072, CVE-2026-106074, CVE-2026-106070, CVE-2026-106071, CVE-2026-106073 | Node.js 22.x / 24.x / 26.x | **check advisory** — feed scopes only Node.js versions, does not name FortiOS 7.4.x | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-11-02 | FG-IR-26-188 — Linux kernel RCE (CVSS 9.8) | CVE-2026-98432 | Linux kernel IPv4 TCP (inet_opt / ireq_opt use-after-free) | **check advisory** — feed text is the upstream kernel patch note and does not list FortiOS versions | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-13 | FG-IR-26-177 — arbitrary command execution in fgfmd daemon (CVSS 5.3) | none listed in feed | FortiManager | **no** — FortiManager only; also requires the cluster node certificate + private key | https://fortiguard.fortinet.com/psirt/FG-IR-26-177 |
| 2026-10-13 | FG-IR-26-178 — Multiple Arbitrary File Read/Write via SQL Injection (CVSS 7.9) | none listed in feed | FortiAnalyzer / FortiManager / FortiManager Cloud / FortiAnalyzer Cloud | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-178 |
| 2026-10-13 | FG-IR-26-179 — Fabric API Authentication bypass (CVSS 4.9, CWE-321) | none listed in feed | FortiSandbox / FortiSandbox Cloud API | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-179 |
| 2026-10-13 | FG-IR-26-180 — Arbitrary file write in grpc phQueryMaster (CVSS 9.4, CWE-22) | none listed in feed | FortiSIEM | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-180 |
| 2026-10-13 | FG-IR-26-185 — Improper access control and session management in web mgmt interface (CVSS 9.6, CWE-288) | none listed in feed | FortiSandbox GUI | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-185 |
| 2026-10-13 | FG-IR-26-176 — Persistent Denial of Service in Admin Profile (CVSS 4.8, CWE-248) | none listed in feed | FortiSandbox | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-176 |
| 2026-10-13 | FG-IR-26-183 — Broken authorization (CVSS 3.4, CWE-862) | none listed in feed | FortiSIEM GUI | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-183 |
| 2026-10-13 | FG-IR-26-184 — SQL command injection in webmail (CVSS 7.7, CWE-89) | none listed in feed | FortiMail webmail | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-184 |

## Outbreak Alerts — last 30 days

| Date (pubDate) | Alert | CVE ids | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Node.js / Google Brotli decoder (third-party) | **check advisory** — alert describes Node.js/Brotli only; see FG-IR-26-187 for Fortinet scope | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | CVE-2026-109618, CVE-2026-109619 (per FG-IR-26-186) | PHP applications (third-party); FortiGuard Labs monitoring | **no** per FG-IR-26-186 statement that no Fortinet product meets exploitation conditions | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Data Center (Bitbucket, Confluence, Jira, Bamboo, Crowd, Crucible, Fisheye) | **no** — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

**No Outbreak Alert about a Fortinet product was published in the last 30 days.** The Fortinet-relevant items on the alert feed all predate the window; the other in-window alerts are third-party products.

## Notes & method

- The IR feed carries only title / link / pubDate / description — it does **not** carry the CSAF "affected" product-version list for these items, so no row above invents a 7.4.x range. Rows marked *check advisory* need the advisory page's "Software Versions and Platforms" table confirmed before closing.
- No item in the 30-day window mentions FortiOS 7.4.x, FortiGate or FortiAP explicitly in its text; the only FortiOS-scoped advisories are FG-IR-26-181 and FG-IR-26-182.
- FG-IR-26-181 is the one worth a human look first: CVSS 6.7, **unauthenticated**, network-reachable, targets the HTTPS management interface — mitigate regardless of version scope by restricting HTTPS admin access to trusted mgmt interfaces/IPs.
- FG-IR-26-182 requires local authenticated CLI access (CVSS 2.1), so admin-account access control and TACACS/RADIUS authorisation already limit exposure.
- This check was **read-only**: no changes were made to the FortiGate; only status reads plus the two public FortiGuard RSS feeds.

*Generated 2026-11-05 (America/Toronto) by fortigate_cve_watch for fw-maniak-hq, FortiOS v7.4.11 build 2878.*
