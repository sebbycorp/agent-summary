# fortiguard-cve - 2026-11-27

**VERDICT: ACTION NEEDED — FortiOS v7.4.11 is inside the affected range of 4 in-window FortiOS advisories (incl. CVSS 9.6 BGP and CVSS 9.0 HA command execution); all four are fixed only in FortiOS 7.4.13.**

## Target

| Item | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| Running FortiOS | **v7.4.11 (build 2878)** |
| VDOM | root |
| Health at check | CPU 100% idle, mem 1.51 GiB / 3.90 GiB used |
| FortiAP firmware | not supplied in this run's context; no FortiAP entries appeared in either feed today, so no AP-side action |

## Feeds fetched this session

| Feed | URL | Status | Items | lastBuildDate |
|---|---|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | reachable (50,949 B) | 50 | Fri, 27 Nov 2026 14:15:07 +0000 |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml (page: https://fortiguard.fortinet.com/outbreak-alert) | reachable (18,079 B) | 20 | Fri, 27 Nov 2026 14:15:07 +0000 |

Affected-version ranges and CVE IDs below were confirmed against the CSAF JSON for each advisory, also pulled from `https://filestore.fortinet.com/fortiguard/psirt/…json`. No advisory or CVE ID in this report was written from memory.

## PSIRT advisories — last 30 days (2026-10-28 → 2026-11-27) mentioning FortiOS / FortiGate / FortiAP

| Date | Advisory | CVE(s) | CVSS | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|---|
| 2026-11-10 (rev. 11-16) | FG-IR-26-273 — Unauthenticated adjacent command execution via HA | CVE-2026-126259 | 9.0 | FortiOS | **yes** (CSAF: FortiOS >=7.4.0 <=7.4.12) | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 (rev. 11-13) | FG-IR-26-268 — Numeric truncation error in bgp daemon | CVE-2026-126254 | 9.6 | FortiOS, FortiSASE | **yes** (FortiOS >=7.4.0 <=7.4.12) | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness in ospf daemon | CVE-2026-126253 | 9.3 | FortiOS, FortiSASE | **yes** (FortiOS >=7.4.0 <=7.4.12) | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in ssh daemon (auth bypass) | CVE-2026-126255 | 7.4 | FortiOS, FortiSASE, FortiPAM | **yes** (FortiOS >=7.4.0 <=7.4.12) | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 (rev. 11-12) | FG-IR-26-193 — SSLVPN improper certificate validation (MITM on portal↔backend) | CVE-2026-121528 | 7.7 | FortiOS SSL-VPN | **no** (CSAF known_affected = FortiOS >=7.4.0 <=7.4.9; remediation "FortiOS 7.4: Upgrade to 7.4.10 or above" — 7.4.11 is already past it) | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in Remote RADIUS admin TFA | CVE-2026-121530 | 5.9 | FortiOS (admin GUI / RADIUS TFA) | **no** (known_affected = FortiOS >=7.6.0 <=7.6.6; CSAF states "FortiOS 7.4: Not Applicable") | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 (rev. 11-24) | FG-IR-26-225 — Template engine injection → super admin on **managed FortiGates** | CVE-2026-126180 | 8.4 | FortiManager, FortiManager Cloud (FortiGate named as escalation victim) | **check advisory** — FortiOS 7.4.11 is not in the product list; affected = FortiManager >=7.4.0 <=7.4.11 (fix 7.4.12). Relevant only if fw-maniak-hq is FortiManager-managed | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-10 | FG-IR-26-275 — FGFM elevation before registration password verified | CVE-2026-126261 | 9.0 | FortiManager (FGFM = FortiGate↔FortiManager protocol) | **check advisory** — CSAF lists only FortiManager >=7.2.0 <=7.2.8 / >=7.4.0 <=7.4.5 / >=7.6.0 <=7.6.7 / 8.0.0 (fix 7.4.6). FortiOS 7.4.11 not listed; review if this FortiGate accepts FGFM/TCP 541 from untrusted peers | https://fortiguard.fortinet.com/psirt/FG-IR-26-275 |

**FortiAP / FortiProxy: 0 matches** for "FortiAP" and "FortiProxy" across all 50 PSIRT items — no FortiAP or FortiProxy SSL-VPN advisory in the last 30 days.

### Items in the window that do NOT concern FortiOS/FortiGate/FortiAP (excluded from action)

The 2026-11-10 PSIRT batch is otherwise FortiManager / FortiAnalyzer / FortiMail / FortiAuthenticator: FG-IR-26-190, -196, -197, -198, -199, -204, -205, -208, -210, -211, -212, -216, -218, -220, -221, -223, -224, -227, -228, -229, -230, -232, -233, -235, -236, -237, -239, -241, -244, -246, -247, -249, -250, -255, -256, -258, -266, -271. Third-party component advisories FG-IR-26-279 (NGINX, 2026-11-20), FG-IR-26-278 (Java4Shell, 2026-11-18), FG-IR-26-188 (Linux kernel, 2026-11-02) and FG-IR-26-187 (Node2Shell) do not name FortiOS/FortiGate/FortiAP/FortiProxy in their feed text; FG-IR-26-187 (2026-10-26) also falls just outside the 30-day window. No rows generated for them.

## Outbreak Alerts — last 30 days

| Date | Alert | CVE(s) | Product relevance | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-19 | PHP2Shell Remote Code Execution Vulnerabilities | not itemised in feed text | PHP runtimes / web servers — no Fortinet product named | no (not a FortiOS/FortiGate issue) | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-11-18 | Refrigeration Control Systems Attack | not itemised in feed text | Danfoss / Copeland / Dixell ICS — no Fortinet product named | no (not a FortiOS/FortiGate issue) | https://fortiguard.fortinet.com/outbreak-alert/refrigeration-control-systems-attack |

Keyword scan of the whole outbreak feed returned **0** occurrences of "FortiOS", "FortiGate", "FortiAP" and "FortiProxy" (the 12 "FortiGuard" hits are the usual "FortiGuard Labs is monitoring…" boilerplate). **No Fortinet-product Outbreak Alert in the last 30 days.** Older alerts in the feed (Node2Shell 2026-10-27, Atlassian 2026-10-19, and everything before) are outside the window and name no Fortinet product.

## Findings and recommended actions

1. **Patch gap — 4 confirmed-affected FortiOS advisories.** FG-IR-26-273, -268, -267 and -269 all list FortiOS >=7.4.0 <=7.4.12 as affected with **7.4.13** as the fixed release; we run **7.4.11**. Plan a maintenance window to upgrade fw-maniak-hq to **FortiOS 7.4.13 or later**.
2. **Highest urgency — FG-IR-26-273 (CVSS 9.0):** unauthenticated super-admin command execution via crafted HA packets (HA FGSP + standalone-config-sync, or autoscale). Advisory guidance: `config system ha` → `set password <secret>`, `set encryption enable`, `set authentication enable`; for standalone-config-sync/autoscale also set the `psksecret`. A **virtual patch is available: "FG-VD-61445.0day." in FMWP db update 26.082** — confirm IPS signature delivery is current on this box as interim cover.
3. **FG-IR-26-268 (9.6) / FG-IR-26-267 (9.3)** are BGP and OSPF daemon flaws. Real exposure depends on routing config — verify whether this FortiGate runs BGP with ibgp peers or OSPF on untrusted segments; if it does not, practical risk drops until patch time.
4. **FG-IR-26-269 (7.4)** is a pre-auth SSH-daemon authentication bypass — restrict SSH/HTTPS admin access to trusted management networks until patched.
5. **No action** on FG-IR-26-193 / FG-IR-26-195 — 7.4.11 is already outside their affected ranges per the CSAF data.
6. **Check if applicable:** if fw-maniak-hq is FortiManager-managed, confirm the FortiManager is >= 7.4.6 (FG-IR-26-275) and >= 7.4.12 (FG-IR-26-225). FG-IR-26-225 lets a low-privileged FortiManager Device Manager admin escalate to super admin on managed FortiGates, so patching the FortiGate alone does not close it.
7. No Fortinet Outbreak Alert this period — no active-exploitation escalation from FortiGuard Labs against FortiOS / FortiGate / FortiAP / FortiProxy SSL-VPN.

---
*Generated read-only by fortigate_cve_watch on 2026-11-27 (America/Toronto). Sources: FortiGuard PSIRT RSS `ir.xml` and Outbreak Alert RSS `outbreakalert.xml` from filestore.fortinet.com, fetched 2026-11-27 14:30 UTC, plus per-advisory CSAF JSON. No changes were made to the firewall.*
