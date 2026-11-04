# fortiguard-cve - 2026-11-04

**Verdict: ACTION NEEDED** — one FortiGuard PSIRT advisory in the last 30 days (FG-IR-26-182 / CVE-2026-106175) lists "FortiOS 7.4 all versions" as *known affected*, which includes our running 7.4.11; Fortinet's guidance for the 7.4 branch is "migrate to a fixed release".

## Asset under review

| Field | Value | Source |
|---|---|---|
| Hostname | fw-maniak-hq | `fg_system_status` |
| Model | FortiGate 80F (FGT80F) | `fg_system_status` |
| Serial | FGT80FTK22061709 | `fg_system_status` |
| FortiOS | **v7.4.11 (build 2878)**, vdom root | `fg_system_status` |
| FortiAP firmware | Not reported by the status tool and no FortiAP firmware was supplied in this run's context — no FortiAP item appeared in either feed (0 "FortiAP" occurrences in ir.xml), so nothing to match | `fg_system_status` / feeds |
| Load at check | CPU ~100% idle, mem 1.48 GB / 3.90 GB used — healthy, no DoS symptoms | `fg_system_status` |

## Feeds fetched this session

| Feed | URL | Result |
|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK — 50 items (via BusyBox wget) |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml (the fortiguard.fortinet.com/outbreak-alert page) | OK — 20 items (via BusyBox wget) |
| CSAF detail (for affected-version calls) | https://filestore.fortinet.com/fortiguard/psirt/csaf_*.json | OK — downloaded per advisory |

Both feeds were reachable; none unreachable. Window applied: **2026-10-05 → 2026-11-04** (last 30 days, America/Toronto). No advisory text in the RSS carried an affected-version range, so `product_status.known_affected` / vendor_fix text from each advisory's CSAF file on filestore.fortinet.com was used for the "affects 7.4.11?" column — that is Fortinet's own wording, not inference.

## Table — items in the window that concern FortiOS / FortiGate / FortiAP

| Date (feed) | Advisory / alert | CVE ids | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-13 | FG-IR-26-182 — Double free in signal handler (CVSS 2.1) | CVE-2026-106175 | FortiOS, FortiProxy, FortiSwitch | **YES** — CSAF `known_affected`: "FortiOS 7.4 all versions"; fix guidance "FortiOS 7.4: Migrate to a fixed release" | https://fortiguard.fortinet.com/psirt/FG-IR-26-182 |
| 2026-10-13 | FG-IR-26-181 — GUI Denial of Service (CVSS 6.7) | CVE-2026-106174 | FortiOS | **NO** — CSAF `known_affected` is FortiOS 8.0.0 and >=7.6.4<=7.6.6 only; "FortiOS 7.4 all versions" is listed *known_not_affected* ("FortiOS 7.4: Not Applicable") | https://fortiguard.fortinet.com/psirt/FG-IR-26-181 |

## Table — adjacent context in the window (Fortinet PSIRT advisories / Fortinet-published alerts, not FortiOS-FortiGate-FortiAP code paths)

| Date (feed) | Advisory / alert | CVE ids | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-02 | FG-IR-26-188 — Linux kernel RCE (CVSS 9.8) | CVE-2026-98432 | Linux kernel (advisory-level; CSAF product tree is empty) | **check advisory** — no product/version list in feed or CSAF | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-26 (rev. 2026-11-03) | FG-IR-26-187 — "Node2Shell" CVEs (CVSS 9.8) | CVE-2026-105482, CVE-2026-105483, CVE-2026-106069, CVE-2026-111497, CVE-2026-105481, CVE-2026-106072, CVE-2026-106074, CVE-2026-106070, CVE-2026-106071, CVE-2026-106073 | Node.js 26.x/24.x/22.x (CSAF product tree empty) | **check advisory** — feed names Node.js versions only, does not say whether FortiOS is affected | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-10-27 | Outbreak Alert — Node2Shell Remote Code Execution | CVE-2026-111497 | Google Brotli decoder / Node.js (third-party) | **check advisory** — no Fortinet product named; active exploitation observed | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-21 (rev. 2026-10-28) | FG-IR-26-186 — "PHP2Shell" core RCE patches (CVSS 9.8) | CVE-2026-109618, CVE-2026-109619 | PHP (CSAF product tree empty) | **check advisory** — feed does not say whether FortiOS is affected | https://fortiguard.fortinet.com/psirt/FG-IR-26-186 |
| 2026-10-27 | Outbreak Alert — PHP2Shell Remote Code Execution | (none listed in alert) | PHP application process (third-party) | **check advisory** — no Fortinet product named | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-01 (rev. 2026-10-07) | FG-IR-26-175 — Path traversal + NULL byte, **exploited in the wild** (CVSS 9.8) | (none in feed text) | FortiMail >=7.2.0<=7.2.9, >=7.4.0<=7.4.8, >=7.6.0<=7.6.6, >=8.0.0<=8.0.1 | **NO** for FortiOS — CSAF names FortiMail only; relevant only if a FortiMail is in scope | https://fortiguard.fortinet.com/psirt/FG-IR-26-175 |
| 2026-10-13 | FG-IR-26-180 — Arbitrary file write in grpc phQueryMaster (CVSS 9.4) | (none in feed text) | FortiSIEM | **NO** | https://fortiguard.fortinet.com/psirt/FG-IR-26-180 |
| 2026-10-13 | FG-IR-26-185 — Auth bypass in web management interface (CVSS 9.6) | (none in feed text) | FortiSandbox | **NO** | https://fortiguard.fortinet.com/psirt/FG-IR-26-185 |
| 2026-10-13 | FG-IR-26-176 — Persistent DoS in Admin Profile (CVSS 4.8) | (none in feed text) | FortiSandbox | **NO** | https://fortiguard.fortinet.com/psirt/FG-IR-26-176 |
| 2026-10-13 | FG-IR-26-179 — Fabric API authentication bypass (CVSS 4.9) | (none in feed text) | FortiSandbox / FortiSandbox Cloud API | **NO** | https://fortiguard.fortinet.com/psirt/FG-IR-26-179 |
| 2026-10-13 | FG-IR-26-183 — Broken authorization (CVSS 3.4) | (none in feed text) | FortiSIEM GUI | **NO** | https://fortiguard.fortinet.com/psirt/FG-IR-26-183 |
| 2026-10-13 | FG-IR-26-178 — Arbitrary file read/write via SQL injection (CVSS 7.9) | (none in feed text) | FortiAnalyzer, FortiManager, FortiManager Cloud, FortiAnalyzer Cloud | **NO** | https://fortiguard.fortinet.com/psirt/FG-IR-26-178 |
| 2026-10-13 | FG-IR-26-177 — OS command injection in fgfmsd (CVSS 5.3) | (none in feed text) | FortiManager fgfmd | **NO** | https://fortiguard.fortinet.com/psirt/FG-IR-26-177 |
| 2026-10-13 | FG-IR-26-184 — SQL command injection in webmail (CVSS 7.7) | (none in feed text) | FortiMail webmail | **NO** | https://fortiguard.fortinet.com/psirt/FG-IR-26-184 |
| 2026-10-19 | Outbreak Alert — Atlassian Arbitrary File Access | CVE-2026-21589 | Atlassian Data Center products (third-party) | **NO** | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

## Outbreak Alerts — Fortinet products

No Outbreak Alert in the feed (any date, 20 items) concerns a Fortinet product. Zero items matched FortiOS / FortiGate / FortiAP / FortiProxy; the two alerts inside the window (Node2Shell 2026-10-27, PHP2Shell 2026-10-27) are third-party runtimes (Brotli/Node.js, PHP).

## Notes and exclusions

- **FG-IR-20-005** ("HTML injection in WebUI", FortiManager/FortiAnalyzer) appeared in the feed but its `pubDate` is 2021-09-21 — outside the 30-day window (only its revision date, 2026-10-13, is recent). Excluded from the tables for that reason; it also names no FortiOS/FortiGate product.
- Out-of-window FortiOS items present in ir.xml (e.g. FortiOS SSL-VPN web-mode, fnbamd, explicit proxy, captive portal entries) all carry `pubDate` before 2026-10-05 and are therefore out of scope for this daily check.
- No FortiOS, FortiGate or FortiProxy SSL-VPN advisory was published in the window beyond FG-IR-26-181 and FG-IR-26-182.

## Recommended follow-up (read-only agent — no change made to the firewall)

1. **FG-IR-26-182 / CVE-2026-106175 is the only genuine exposure for 7.4.11.** Low severity (CVSS 2.1), local authenticated attacker, impact limited to crashing the `newcli` daemon via crafted CLI commands — no remote or unauthenticated path, and the box is idle/healthy. Review the advisory and its CSAF/PSDB page with whoever owns patching: Fortinet's 7.4 guidance is "migrate to a fixed release" (the 7.4 branch is listed affected in full, with no fixed 7.4.x named in the CSAF), so the practical options are the current recommended 7.4.x patch release or a planned 7.6.x migration. Decide on the normal change window — nothing here warrants emergency action.
2. Keep CLI access restricted to trusted admin accounts/hosts, since exploitation of FG-IR-26-182 requires local authenticated CLI access.
3. FG-IR-26-181 (GUI DoS) is explicitly Not Applicable to 7.4; its FMWP virtual patch (FG-VD-61291.0day, db 26.073) matters only for 7.6.4–7.6.6 / 8.0.0 hosts.
4. Confirm whether a FortiMail (FG-IR-26-175, exploited in the wild, CVSS 9.8), FortiSIEM, FortiSandbox, FortiManager or FortiAnalyzer is anywhere in the estate — this check only covers the 80F.
5. No FortiAP firmware was supplied and no FortiAP advisory appeared in the PSIRT feed today; if managed FortiAPs exist, their firmware version needs to come from the controller/AP for future runs to match against FortiAP advisories.

---
*Sources: FortiGuard PSIRT IR RSS (`ir.xml`) and Outbreak Alert RSS (`outbreakalert.xml`) fetched with BusyBox wget on 2026-11-04; affected-version determinations from each advisory's CSAF JSON on filestore.fortinet.com. Every row above traces to those fetched feeds — no advisory or CVE id is outside them. This check was read-only: no firewall configuration or state was modified.*
