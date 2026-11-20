# fortiguard-cve - 2026-11-20

**VERDICT: ACTION NEEDED — FortiOS 7.4.11 on fw-maniak-hq is listed as affected by 3 CRITICAL and 1 HIGH PSIRT advisory (all fixed in FortiOS 7.4.13); plan an upgrade to 7.4.13 or apply the listed virtual patches/mitigations.**

## Asset under watch (read-only facts)

| Item | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | **v7.4.11 build 2878** |
| vDOM | root |
| Load at check time | CPU ~0% idle 100%, mem ~39% used, sessions ~910 |

Managed FortiAPs: no FortiAP firmware version was supplied in this check's context, and neither feed published a FortiAP (FP231F) or FortiProxy SSL-VPN advisory in the window, so no AP/FortiProxy row is listed.

## Feeds fetched this session

| Feed | URL | Status |
|---|---|---|
| PSIRT IR Advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK — 50 items |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | OK — 19 alerts |

Per-advisory CVE ids and exact version ranges were verified against the CSAF JSON published at `filestore.fortinet.com/fortiguard/psirt/` for each advisory.

Window reviewed: 2026-10-21 → 2026-11-20 (last 30 days). Feed items in this window carry pubDates of 2026-11-18, 2026-11-10, 2026-11-02, 2026-10-27 and 2026-10-26 — all inside the window.

## Table 1 — FortiOS / FortiGate advisories from the PSIRT feed (last 30 days)

| Date | Advisory | CVE id | CVSS | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 — Numeric truncation error (bgp daemon) | CVE-2026-126254 | 9.6 CRITICAL | FortiOS / FortiSASE | **YES** — CSAF affected FortiOS 7.4.0 through 7.4.12; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness (ospf daemon) | CVE-2026-126253 | 9.3 CRITICAL | FortiOS / FortiSASE | **YES** — affected FortiOS 7.4.0 through 7.4.12; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-273 — Unauthenticated adjacent command execution via HA (revised 2026-11-16) | CVE-2026-126259 | 9.0 CRITICAL | FortiOS (HA FGSP / standalone-config-sync / autoscale, FGCP) | **YES** — affected FortiOS 7.4.0 through 7.4.12; fixed 7.4.13; virtual patch FG-VD-61445.0day (FMWP db 26.082) | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 | FG-IR-26-269 — Improper neutralization in sshd (path traversal, auth bypass) | CVE-2026-126255 | 7.4 HIGH | FortiOS / FortiSASE / FortiPam | **YES** — affected FortiOS 7.4.0 through 7.4.12; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 — SSLVPN improper certificate validation (MITM on SSL-VPN portal↔backend) | CVE-2026-121528 | 7.7 HIGH | FortiOS SSLVPN | **NO** — affected FortiOS 7.4.0 through 7.4.9, fixed in 7.4.10; 7.4.11 already past the fix | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in RADIUS TFA admin login | CVE-2026-121530 | 5.9 MEDIUM | FortiOS Remote Radius TFA | **NO** — affected only FortiOS 7.6.0 through 7.6.6; CSAF lists "FortiOS/ 7.4 all versions" as known-not-affected | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection in FortiManager → super admin on **managed FortiGates** | CVE-2026-126180 | 8.4 HIGH | FortiManager / FortiManager Cloud (7.4.0 through 7.4.11) | **check advisory** — FortiOS build itself not listed as affected; exposure only via the FortiManager managing this FortiGate | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |

## Table 2 — Outbreak Alerts (last 30 days)

No Outbreak Alert in the window is about a Fortinet product, so there is no Fortinet exposure to report from this feed. Alerts published in the window and their relevance:

| Date | Alert | Product concerned | Affects 7.4.11? | Link |
|---|---|---|---|---|
| 2026-11-19 | PHP2Shell Remote Code Execution Vulnerabilities | PHP applications (third-party) | No (not FortiOS/FortiGate/FortiAP) | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-11-18 | Refrigeration Control Systems Attack | Danfoss / Copeland / Dixell ICS-OT | No (not FortiOS/FortiGate/FortiAP) | https://fortiguard.fortinet.com/outbreak-alert/refrigeration-control-systems-attack |
| 2026-10-27 | Node2Shell Remote Code Execution | Node.js / Brotli (CVE-2026-111497) | No (not FortiOS/FortiGate/FortiAP) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |

## Excluded from the tables (in window, but not a FortiOS/FortiGate/FortiAP/FortiProxy product item)

PSIRT feed also carried generic third-party / library advisories whose CSAF product trees contain no FortiOS, FortiGate, FortiAP or FortiProxy entry: FG-IR-26-278 "Java4Shell" (CVE-2026-114938, Oracle Java SE), FG-IR-26-188 "Linux kernel RCE" (CVE-2026-98432, IPv4 TCP), FG-IR-26-187 "Node2Shell CVEs" (Node.js), plus FortiAnalyzer / FortiMail / FortiAuthenticator / FortiManager-only items (e.g. FG-IR-26-258, FG-IR-26-271, FG-IR-26-190, FG-IR-26-196/-197/-198, FG-IR-26-200, FG-IR-26-210, FG-IR-26-216, FG-IR-26-224, FG-IR-26-255, FG-IR-26-266, FG-IR-26-275 "FGFM elevation before registration password is verified" / CVE-2026-126261 — FortiManager only, no FortiOS entry).

## Recommended actions (advisory text only — nothing was changed on the device)

1. **Upgrade FortiOS 7.4.11 → 7.4.13** — clears all four affected advisories at once (FG-IR-26-268, FG-IR-26-267, FG-IR-26-273, FG-IR-26-269). 7.4.13 is the vendor fix release for the whole 7.4.0–7.4.12 range.
2. Until upgraded, apply FG-IR-26-273 mitigations if HA / FGSP / standalone-config-sync / autoscale is in use: `config system ha` → `set password`, `set encryption enable`, `set authentication enable`; `config system standalone-cluster` → `set encryption enable`, `set psksecret`; `config system autoscale` → `set psksecret`. Virtual Patch FG-VD-61445.0day (FMWP db update 26.082) covers it with FortiGuard IPS/IPS-OT enabled.
3. Reduce exposure for FG-IR-26-267 / FG-IR-26-268: restrict OSPF/BGP peering to trusted interfaces/neighbors only (both are unauthenticated or configured-neighbor attacks against the daemons).
4. Confirm administrative SSH (FG-IR-26-269) is not reachable from untrusted networks, and keep admin access on the dedicated management interface.
5. If this FortiGate is centrally managed: check the FortiManager version against FG-IR-26-225 (fix FortiManager 7.4.12).

*Report generated read-only. Sources: FortiGuard PSIRT RSS (ir.xml) and Outbreak Alerts RSS (outbreakalert.xml), plus the vendor CSAF JSON per advisory — all fetched on 2026-11-20.*
