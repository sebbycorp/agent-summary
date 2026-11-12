# fortiguard-cve - 2026-11-12

**VERDICT: ACTION NEEDED — FortiOS 7.4.11 falls inside the affected range (>=7.4.0, <=7.4.12) of CRITICAL FG-IR-26-267 (CVSS 9.3) and HIGH FG-IR-26-269 (CVSS 7.4); both are fixed only in FortiOS 7.4.13.**

## Asset under review

| Field | Value | Source |
|---|---|---|
| Hostname | fw-maniak-hq | fg_system_status |
| Model | FortiGate 80F (FGT80F) | fg_system_status |
| FortiOS | v7.4.11 build 2878 | fg_system_status |
| Serial | FGT80FTK22061709 | fg_system_status |
| VDOM / health | root; CPU 0% (all 8 cores idle), mem ~1.49 GB / 3.89 GB | fg_system_status |
| Managed FortiAPs | No FortiAP firmware context supplied in this run's user context, and no FortiAP advisory appeared in the window | — |

**Read-only run:** no configuration change was made to the firewall. Only `fg_system_status` and two public FortiGuard feeds were used.

## Feeds fetched in this session

| Feed | URL | Result |
|---|---|---|
| PSIRT advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK — 47,068 B, 50 items, all dated 2026-11-10 (rolling feed) |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | OK — 17,411 B, 20 items |

Both feeds were reachable; neither was unreachable. Advisory product/version ranges and CVE ids were confirmed from the vendor CSAF JSON on `https://filestore.fortinet.com/fortiguard/psirt/` (the RSS `<description>` itself carries **no** CVE ids and **no** version ranges).

Window applied: **2026-10-13 → 2026-11-12** (last 30 days), report date in America/Toronto.

## In-window items touching FortiOS / FortiGate / FortiAP

| Date | Advisory / Alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-267 — Authentication Bypass by Primary Weakness in FortiOS/FortiSASE OSPF daemon (CRITICAL, CVSS 9.3) | CVE-2026-126253 | FortiOS, FortiSASE | **yes** — CSAF `known_affected` = FortiOS >=7.4.0<=7.4.12; fixed in 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in ssh daemon, unauthenticated auth bypass (HIGH, CVSS 7.4) | CVE-2026-126255 | FortiOS, FortiSASE, FortiProxy, FortiPAM | **yes** — CSAF `known_affected` = FortiOS >=7.4.0<=7.4.12; fixed in 7.4.13. CSAF note: "product is impacted only if GPC OS Login is enabled" — verify that setting | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-195 — Improper Authentication in FortiOS Remote RADIUS Type Admin TFA (MEDIUM, CVSS 5.9) | CVE-2026-121530 | FortiOS | **no** — vendor_fix states "FortiOS 7.4: Not Applicable" (affected: 7.6.0–7.6.6) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template engine injection in FortiManager/FortiManager Cloud; low-priv Device Manager admin gains **super admin on managed FortiGates** (HIGH) | CVE-2026-126180 | FortiManager, FortiManager Cloud | **no** (FortiOS not a vulnerable product) — **check advisory** if fw-maniak-hq is centrally managed: FortiManager >=7.4.0<=7.4.11 affected | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-10 | FG-IR-26-275 — Improper authorization in FortiManager FGFM: unauthenticated attacker reaching the FGFM port executes code/commands (CRITICAL, CVSS 9.0) | CVE-2026-126261 | FortiManager | **no** (FortiOS not listed) — **check advisory**: FGFM is the FortiGate↔FortiManager plane; restrict FGFM (541/TCP) reachability to this FortiGate | https://fortiguard.fortinet.com/psirt/FG-IR-26-275 |

### Outbreak Alerts (Fortinet products) — none in window

The three alerts published inside the 30-day window all concern **third-party** software; none names FortiOS, FortiGate, FortiAP or FortiProxy as the vulnerable product (their only "fortinet" strings are feed boilerplate/URLs). Listed for completeness:

| Date | Alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution (OOB write in Google Brotli decoder, incl. Node.js) | CVE-2026-111497 | Brotli / Node.js (third-party) | no | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | none stated in feed | PHP (third-party) | no | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | none stated in feed | Atlassian (third-party) | no | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

**No FortiAP advisory was present:** of the 50 PSIRT items in the feed, only FortiOS, FortiSASE, FortiProxy, FortiPAM, FortiManager, FortiAnalyzer, FortiMail, FortiAuthenticator and FortiVoice appear — no FortiAP item.

## Recommended follow-up (not executed — this agent is read-only)

1. **Plan a FortiOS 7.4.11 → 7.4.13 upgrade** for fw-maniak-hq: it is the single fixed release clearing both FG-IR-26-267 and FG-IR-26-269.
2. Until patched, treat OSPF as the priority exposure (FG-IR-26-267, unauthenticated, CVSS 9.3): if OSPF is not required, disable it or restrict it to trusted interfaces/peers.
3. For FG-IR-26-269, confirm whether **GPC OS Login** is enabled — the vendor states the product is impacted only when it is.
4. FG-IR-26-195 needs no action on 7.4.x (vendor: Not Applicable).
5. If fw-maniak-hq is managed by FortiManager, verify that FortiManager is at >=7.4.12 (FG-IR-26-225) / >=7.4.6 (FG-IR-26-275), and keep FGFM reachable only from authorized FortiManagers.

*Generated by agent `fortigate_cve_watch`. Every advisory, CVE id and version range above was read from the two FortiGuard feeds and their vendor CSAF files fetched in this session; nothing was inferred from memory.*
