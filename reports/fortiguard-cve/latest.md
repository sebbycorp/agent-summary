# fortiguard-cve - 2026-11-25

**Verdict: ACTION NEEDED — 4 FortiOS advisories published in the last 30 days list FortiOS 7.4.0 through 7.4.12 as affected, which includes the running v7.4.11 build 2878 on fw-maniak-hq. Three are CRITICAL (CVSS 9.0–9.6). The fix for all four is FortiOS 7.4.13.**

## Target

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS version | **v7.4.11 build 2878** |
| VDOM checked | root |
| Health at check time | CPU 0% user / 0% system (100% idle, 8 cores); mem 1.51 GB used of 3.90 GB |
| Check date | 2026-11-25 (America/Toronto) |
| Window | last 30 days (2026-10-26 → 2026-11-25) |

*FortiAP note: no FortiAP firmware (e.g. FP231F v7.4.x) context was provided for this check, and the PSIRT feed contained **zero** FortiAP advisories today, so no FortiAP rows are listed. If managed FortiAPs are in scope, supply their model/firmware and I will re-run against them.*

## Feeds fetched this session

| Feed | URL | Status | Items |
|---|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | ✅ reachable | 50 |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | ✅ reachable | 20 |

Affected-version calls below were confirmed against the vendor CSAF documents on
`https://filestore.fortinet.com/fortiguard/psirt/…` (authoritative `known_affected` /
`known_not_affected` lists), not just the RSS summary text.

## Advisories affecting FortiOS / FortiGate (last 30 days)

| Date | Advisory | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 — Numeric truncation error in bgp daemon (CVSS 9.6 CRITICAL) | CVE-2026-126254 | FortiOS, FortiSASE | **YES** (CSAF: FortiOS >=7.4.0 <=7.4.12) → fix 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness in ospf daemon (CVSS 9.3 CRITICAL) | CVE-2026-126253 | FortiOS, FortiSASE | **YES** (>=7.4.0 <=7.4.12) → fix 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-273 — Unauthenticated adjacent command execution via HA / FGSP (CVSS 9.0 CRITICAL) | CVE-2026-126259 | FortiOS | **YES** (>=7.4.0 <=7.4.12, plus 7.0 all) → fix 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in ssh daemon, auth bypass (CVSS 7.4 HIGH) | CVE-2026-126255 | FortiOS, FortiPAM, FortiSASE | **YES** (>=7.4.0 <=7.4.12) → fix 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 — SSLVPN improper certificate validation, MITM on SSL-VPN portal↔backend (CVSS 7.7 HIGH) | CVE-2026-121528 | FortiOS SSLVPN | **NO** — CSAF affected range is FortiOS >=7.4.0 <=7.4.9; 7.4.10 is listed `known_not_affected`, so 7.4.11 is past the fix | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in FortiOS Remote RADIUS admin TFA (CVSS 5.9 MEDIUM) | CVE-2026-121530 | FortiOS | **NO** — CSAF lists "FortiOS 7.4 all versions" under `known_not_affected`; remediation says "FortiOS 7.4: Not Applicable" (affected: 7.6.0–7.6.6) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |

**No FortiProxy SSL-VPN advisories appeared in the feed during this window.**

## Management-plane items that reach our FortiGates

fw-maniak-hq is not the vulnerable product in these, but they matter if a FortiManager
manages this device — FG-IR-26-225 explicitly yields super-admin on **managed FortiGates**.

| Date | Advisory | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-225 — SSTI/template injection in FortiManager → super admin on managed FortiGates (CVSS 8.4 HIGH) | CVE-2026-126180 | FortiManager, FortiManager Cloud | **N/A to the FortiGate itself** — vulnerable product is FortiManager >=7.4.0 <=7.4.11. **check advisory** against your FortiManager version | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-10 | FG-IR-26-275 — FGFM elevation before registration password verified (CVSS 9.0 CRITICAL) | CVE-2026-126261 | FortiManager | **N/A to the FortiGate itself** — vulnerable: FortiManager >=7.4.0 <=7.4.5. **check advisory** | https://fortiguard.fortinet.com/psirt/FG-IR-26-275 |

## Other FortiGate-adjacent advisories reviewed — not applicable

| Date | Advisory | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-247 — Improper certificate validation in RADIUS | — | FortiSASE Sovereign, FortiManager, FortiAnalyzer (+Cloud) | **NO** — FortiOS not in product list | https://fortiguard.fortinet.com/psirt/FG-IR-26-247 |
| 2026-11-02 | FG-IR-26-188 — Linux kernel RCE (tcp_v4_syn_recv_sock) | CVE-2026-98432 | upstream Linux kernel; CSAF `product_tree` is **empty** (no Fortinet product enumerated) | **check advisory** — no FortiOS product listed, so not attributable to 7.4.11 from the feed | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-26 | FG-IR-26-187 — "Node2Shell" Node.js CVEs | CVE-2026-105481, -105482, -105483, -106069, -106070, -106071, -106072, -106073, -106074, -111497 | Node.js / Brotli; CSAF `product_tree` empty | **NO** — third-party runtime, no Fortinet product listed | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-11-20 | FG-IR-26-279 — NGINX vulnerabilities | CVE-2026-113425, -118899, -120967, -120993, -121810, -123007, -123633, -123634, -123640, -125283 | NGINX OSS; CSAF `product_tree` empty | **NO** — third-party component, no Fortinet product listed | https://fortiguard.fortinet.com/psirt/FG-IR-26-279 |
| 2026-11-18 | FG-IR-26-278 — Java4Shell | CVE-2026-114938 | Oracle Java SE / GraalVM; CSAF `product_tree` empty | **NO** — third-party component | https://fortiguard.fortinet.com/psirt/FG-IR-26-278 |
| 2026-11-10 | FG-IR-26-266 — SQL injection in collectoragent | CVE-2026-126252 | FortiAuthenticator 8.0.0–8.0.3, 6.6.0–6.6.10 | **NO** — FortiOS not listed | https://fortiguard.fortinet.com/psirt/FG-IR-26-266 |

The remaining 36 of the 50 advisories in the window (50 total − 14 listed individually above)
target FortiManager, FortiAnalyzer, FortiMail,
FortiAuthenticator, FortiVoice/FortiRecorder only — no FortiOS/FortiGate/FortiAP product
string, so they are out of scope for this FortiGate.

## Outbreak Alerts (last 30 days)

The feed was fetched successfully. **No Outbreak Alert in the last 30 days concerns Fortinet
products** — the string "Fortinet"/"FortiOS"/"FortiGate"/"FortiAP"/"FortiProxy" appears nowhere
in the feed body except the channel/branding text ("FortiGuard Labs", "FortiGuard IPS",
"FortiGuard telemetry"). No action is required on fw-maniak-hq from this feed; listed for
awareness only.

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-19 | PHP2Shell Remote Code Execution Vulnerabilities | not stated in feed | PHP applications (third-party) | **NO** — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-11-18 | Refrigeration Control Systems Attack | not stated in feed | Danfoss AK-SM/AK-SC, Copeland/Dixell XWEB ICS | **NO** — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/refrigeration-control-systems-attack |
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Google Brotli decoder / Node.js | **NO** — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/node2shell |

## Recommended actions (this device is READ-ONLY to me — nothing was changed)

1. **Plan a FortiOS 7.4.13 upgrade.** A single step to 7.4.13 clears all four affected
   advisories at once (FG-IR-26-268, FG-IR-26-267, FG-IR-26-273, FG-IR-26-269). Do not
   defer: three of them are CRITICAL and two (ospf/bgp daemon) are reachable by an
   adjacent/peering attacker, while FG-IR-26-269 is an unauthenticated SSH login bypass.
2. **Verify exposure surfaces before the window.** Confirm whether BGP/OSPF neighbours are
   configured, whether HA/FGSP + `standalone-config-sync` or autoscale is in use
   (FG-IR-26-273 precondition), and whether the SSL-VPN web portal is enabled — that narrows
   which of the four is actually reachable today.
3. **If a FortiManager manages this FortiGate, patch it too** — FG-IR-26-225 yields super-admin
   on managed FortiGates and FortiManager 7.4.0–7.4.11 is affected; we cannot see the
   FortiManager version from this device, so treat as **check advisory**.
4. FG-IR-26-193 (SSL-VPN MITM) and FG-IR-26-195 (RADIUS admin TFA) are **already fixed / not
   applicable** on 7.4.11 — no separate action, though they are covered by the 7.4.13 move anyway.

---
*Generated by fortigate_cve_watch. All rows sourced from the two FortiGuard feeds fetched in this
session plus their vendor CSAF JSON on filestore.fortinet.com; no advisory or CVE id was inferred.
This check is read-only — no firewall configuration or firmware was modified.*
