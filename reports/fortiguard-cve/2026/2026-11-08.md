# fortiguard-cve - 2026-11-08

**Verdict: REVIEW — no critical/remote exposure. Of the 13 FortiGuard PSIRT advisories published in the last 30 days, exactly one names FortiOS 7.4 (FG-IR-26-182, CVSS 2.1, local-authenticated CLI daemon crash) and therefore applies to the running 7.4.11; every other in-window advisory concerns FortiSIEM / FortiSandbox / FortiManager / FortiAnalyzer / FortiMail / FortiSOAR or third-party runtimes. No action-needed item, no Fortinet outbreak alert.**

## Asset under watch (read-only, no changes made)

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | **v7.4.11, build 2878** (GA) |
| vDOM | root |
| Load at check | CPU 100% idle, ~1.49 GB / 3.90 GB RAM used |
| Managed FortiAPs | FortiAP firmware version not supplied in this check's context — see notes |

## Feeds fetched this session

| Feed | URL | Status | Items | Last build |
|---|---|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK (HTTP 200) | 50 | Sun, 08 Nov 2026 14:15:07 +0000 |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml (page: https://fortiguard.fortinet.com/outbreak-alert) | OK (HTTP 200) | 20 | — |

Per-advisory CSAF JSON was also fetched from `https://filestore.fortinet.com/fortiguard/psirt/…` to resolve exact version ranges and CVE IDs (the RSS description omits them for the 13 Oct batch).

Window applied: **2026-10-09 → 2026-11-08** (30 days). Filter: FortiOS / FortiGate / FortiAP / FortiProxy SSL-VPN, plus any Fortinet-related outbreak alert.

## PSIRT advisories in window (13 items)

"Affects 7.4.11?" = verdict for FortiOS **7.4.11 build 2878**.

| Date | Advisory / alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-02 | FG-IR-26-188 – Linux kernel RCE (CVSS 9.8) | CVE-2026-98432 | Linux kernel (ipv4/inet_opt use-after-free); Fortinet product list not published in the CSAF feed | **check advisory** – CSAF `product_tree` is empty, so FortiOS exposure cannot be confirmed from the feed | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-11-02 (rev. 2026-11-06) | FG-IR-26-187 – Node2Shell CVEs (CVSS 9.8) | CVE-2026-105482, CVE-2026-105483, CVE-2026-106069, CVE-2026-111497, CVE-2026-105481, CVE-2026-106072, CVE-2026-106074, CVE-2026-106070, CVE-2026-106071, CVE-2026-106073 | Node.js 22.x/24.x/26.x (Brotli decoder) – no Fortinet product named | **no** – FortiOS/FortiGate not listed | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-10-21 (rev. 2026-11-05) | FG-IR-26-186 – PHP2Shell core RCE patches (CVSS 5.3) | CVE-2026-109618, CVE-2026-109619, CVE-2026-115560 | FortiSOAR PaaS / FortiSOAR on-premise (7.3–8.0.0); Fortinet states CVE-2026-115560 affects no Fortinet product | **no** – FortiOS not in the affected list | https://fortiguard.fortinet.com/psirt/FG-IR-26-186 |
| 2026-10-13 | FG-IR-26-180 – Arbitrary file write in grpc phQueryMaster (CVSS 9.4) | not published in feed | FortiSIEM | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-180 |
| 2026-10-13 | FG-IR-26-183 – Broken authorization (CVSS 3.4) | not published in feed | FortiSIEM GUI | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-183 |
| 2026-10-13 | **FG-IR-26-182 – Double free in signal handler (CVSS 2.1)** | **CVE-2026-106175** | **FortiOS, FortiProxy, FortiSwitch** – CSAF `known_affected`: FortiOS 7.6.0–7.6.8, **FortiOS 7.4 all versions**, 7.2, 7.0 | **yes** – 7.4.11 is inside "FortiOS 7.4 all versions"; fixed only in FortiOS 8.0 | https://fortiguard.fortinet.com/psirt/FG-IR-26-182 |
| 2026-10-13 | FG-IR-26-179 – Fabric API Authentication bypass (CVSS 4.9) | not published in feed | FortiSandbox / FortiSandbox Cloud API | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-179 |
| 2026-10-13 | FG-IR-26-181 – GUI Denial of Service (CVSS 6.7) | CVE-2026-106174 | FortiOS – CSAF `known_affected`: FortiOS 8.0.0, 7.6.4–7.6.6; `known_not_affected`: **FortiOS 7.4 all versions**, 7.2 all versions | **no** – 7.4 explicitly not affected | https://fortiguard.fortinet.com/psirt/FG-IR-26-181 |
| 2026-10-13 | FG-IR-26-185 – Improper access control/session mgmt in web UI (CVSS 9.6) | not published in feed | FortiSandbox GUI | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-185 |
| 2026-10-13 | FG-IR-26-178 – Multiple arbitrary file read/write via SQL injection (CVSS 7.9) | not published in feed | FortiAnalyzer, FortiManager, FortiManager Cloud, FortiAnalyzer Cloud | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-178 |
| 2026-10-13 | FG-IR-26-176 – Persistent Denial of Service in Admin Profile (CVSS 4.8) | not published in feed | FortiSandbox | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-176 |
| 2026-10-13 | FG-IR-26-184 – SQL command injection in webmail (CVSS 7.7) | not published in feed | FortiMail webmail | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-184 |
| 2026-10-13 | FG-IR-26-177 – Arbitrary command execution in fgfmsd daemon (CVSS 5.3) | not published in feed | FortiManager (fgfmd, cluster-node impersonation) | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-177 |

## Outbreak Alerts in window (3 items)

| Date | Alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 (Brotli out-of-bounds write) | Node.js / Google Brotli decoder | **no** – Fortinet products not named in the alert | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | CVE-2026-109618, CVE-2026-109619 (+ unpatched CVE-2026-115560 per FG-IR-26-186) | PHP core / PHP application servers; FortiGuard Labs monitoring | **no** – Fortinet products not named; see FG-IR-26-186 (FortiOS not affected) | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Bitbucket / Confluence / Jira / JSM / Bamboo / Crowd / Crucible / Fisheye | **no** – third-party product | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

**No Fortinet-product outbreak alert in the window.** The next-older alerts — GitLab Path Traversal (2026-09-23) and Orkes Conductor Evaluator RCE (2026-09-15) — are both third-party products and both fall outside the 30-day window; the last Fortinet-adjacent alert on the page, HTTP/2 Bomb, is dated 2026-06-19.

## Detail on the one item that touches us — FG-IR-26-182

- **CVSS v3 2.1 (low), local authenticated attacker; no privilege escalation, no data exposure.**
- Double Free (CWE-415) in the `newcli` daemon, reached via **crafted CLI commands** — the attacker must already hold valid local CLI access on fw-maniak-hq.
- CSAF affected set: FortiOS `>=7.6.0|<=7.6.8`, **FortiOS 7.4 all versions**, 7.2 all versions, 7.0 all versions → **7.4.11 build 2878 is affected**. Only FortiOS 8.0 is listed as known-not-affected; the advisory offers no 7.4.x patch release.
- The same advisory lists FortiProxy 7.4 all versions and FortiSwitch 7.4 all versions as affected — relevant only if a FortiProxy or FortiSwitch is managed here (none reported for fw-maniak-hq).
- Recommended posture: accept risk and compensate with admin ACL / trusted-host CLI access controls; re-check on the next 7.4.x maintenance release. Advisory acknowledges UK NCSC.

## Notes and caveats

- **FG-IR-26-188 (Linux kernel RCE, CVSS 9.8) is the only "check advisory" row.** Its RSS description is an upstream kernel write-up and its CSAF `product_tree.branches` array is empty, so the feed text does not state whether FortiOS 7.4.11 is affected. Read the advisory page before dismissing it — it is the highest-scoring item in the window.
- FG-IR-26-180 / 183 / 179 / 185 / 178 / 176 / 184 / 177 carry **no CVE ID** in the feed or in their CSAF JSON (13 Oct batch); they are recorded as "not published in feed" rather than having an ID invented here.
- **FortiAP:** zero items in either feed (all 50 IR + 20 outbreak entries) mention FortiAP, and no FortiAP firmware version was supplied in this check's context. If the managed FortiAPs (e.g. FP231F v7.4.x) should be tracked, provide the firmware version and it will be matched in the next run.
- **FortiProxy SSL-VPN:** no FortiProxy SSL-VPN advisory in the window; the only FortiProxy mention is FG-IR-26-182 (CLI `newcli` crash, not SSL-VPN).
- Every row above comes from the two feeds fetched in this session (plus their linked CSAF JSON). Nothing was inferred from memory.
- This run was strictly read-only: `fg_system_status` only — no configuration, firmware or policy changes attempted on fw-maniak-hq.

*Generated 2026-11-08 (America/Toronto) by fortigate_cve_watch.*
