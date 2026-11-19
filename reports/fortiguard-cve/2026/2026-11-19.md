# fortiguard-cve - 2026-11-19

**Verdict: ACTION NEEDED - four FortiOS PSIRT advisories published 2026-11-10 list FortiOS 7.4.11 as affected (three CRITICAL), all fixed in FortiOS 7.4.13; upgrade fw-maniak-hq from v7.4.11.**

## Target

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| Running FortiOS | v7.4.11 (build 2878) |
| VDOM | root |
| Health at check time | CPU 100% idle, mem ~1.52 GB / 3.90 GB - no stress symptoms |

Feeds read this session (both reachable, no failures):

- PSIRT advisories: `https://filestore.fortinet.com/fortiguard/rss/ir.xml` (50 items, feed lastBuildDate Thu, 19 Nov 2026 14:15:07 +0000)
- Outbreak Alerts: `https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml` (20 items, feed lastBuildDate Thu, 19 Nov 2026 14:15:07 +0000)
- Per-advisory CSAF JSON from `https://filestore.fortinet.com/fortiguard/psirt/...` used to confirm exact `known_affected` / `known_not_affected` version ranges and vendor fixes.

Window applied: last 30 days = **2026-10-20 .. 2026-11-19** (America/Toronto).

## PSIRT advisories in window that name FortiOS / FortiGate / FortiAP / FortiProxy

| Date (pubDate) | Advisory | CVE id | Product | CVSS | Affects 7.4.11? | Link |
|---|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 - Numeric truncation error (bgp daemon) | CVE-2026-126254 | FortiOS, FortiSASE | 9.6 CRITICAL | **yes** - CSAF `known_affected` = FortiOS >=7.4.0\|<=7.4.12; fix: upgrade to 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 - Authentication bypass by primary weakness (ospf daemon) | CVE-2026-126253 | FortiOS, FortiSASE | 9.3 CRITICAL | **yes** - CSAF `known_affected` = FortiOS >=7.4.0\|<=7.4.12; fix: upgrade to 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-273 - Unauthenticated adjacent command execution via HA | CVE-2026-126259 | FortiOS | 9.0 CRITICAL | **yes** - CSAF `known_affected` = FortiOS >=7.4.0\|<=7.4.12; fix: upgrade to 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 | FG-IR-26-269 - Path traversal in ssh daemon (auth bypass via crafted credentials) | CVE-2026-126255 | FortiOS, FortiSASE, FortiPAM | 7.4 HIGH | **yes** - CSAF `known_affected` = FortiOS >=7.4.0\|<=7.4.12; fix: upgrade to 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 - SSLVPN improper certificate validation (MITM on SSL-VPN portal -> backend) | CVE-2026-121528 | FortiOS (SSL-VPN), FortiPAM, FortiSASE | 7.7 HIGH | **no** - CSAF `known_affected` = FortiOS >=7.4.0\|<=7.4.9, fixed in 7.4.10; 7.4.11 is above the fix level | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 - Broken access control in Remote Radius admin Two-Factor auth | CVE-2026-121530 | FortiOS (GUI admin TFA) | 5.9 MEDIUM | **no** - vendor fix states "FortiOS 7.4: Not Applicable"; affected = FortiOS >=7.6.0\|<=7.6.6 | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 - Template engine injection: low-privileged Device Manager admin obtains super admin on **managed FortiGates** | CVE-2026-126180 | FortiManager, FortiManager Cloud | 8.4 HIGH | **check advisory** - vulnerable product is FortiManager (>=7.4.0\|<=7.4.11), not FortiOS; impact lands on managed FortiGates. Confirm whether fw-maniak-hq is under FortiManager/FortiManager Cloud and its version | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-18 | FG-IR-26-278 - Java4Shell (Oracle Java SE / GraalVM, component: Compiler) | CVE-2026-114938 | Oracle Java SE / GraalVM (not a Fortinet product tree entry in CSAF) | 8.1 | **no** - no FortiOS/FortiGate/FortiAP/FortiProxy product entries in the advisory CSAF | https://fortiguard.fortinet.com/psirt/FG-IR-26-278 |
| 2026-11-02 | FG-IR-26-188 - Linux kernel RCE (ipv4 inet_opt/ireq_opt use-after-free) | CVE-2026-98432 | Linux kernel - CSAF product tree is empty (no Fortinet products listed) | 9.8 | **check advisory** - feed text does not state affected Fortinet products/versions; open the advisory to confirm FortiOS exposure | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-11-10 (rev; initial 2026-10-26) | FG-IR-26-187 - Node2Shell CVEs (Node.js 22.x/24.x/26.x) | CVE-2026-105482, CVE-2026-105483, CVE-2026-106069, CVE-2026-111497, CVE-2026-105481, CVE-2026-106072, CVE-2026-106074, CVE-2026-106070, CVE-2026-106071, CVE-2026-106073 | Node.js - CSAF product tree is empty (no Fortinet products listed) | 9.8 | **check advisory** - feed text does not state affected Fortinet products/versions; open the advisory to confirm FortiOS exposure | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |

## Outbreak Alerts in window (Fortinet-product check)

| Date (pubDate) | Alert | CVE id | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-18 | Refrigeration Control Systems Attack | none listed in feed | Danfoss AK-SM/AK-SC, Copeland XWEB, Dixell XWEB, other industrial refrigeration/temperature controllers | **no** - not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/refrigeration-control-systems-attack |
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Node.js / Google Brotli decoder | **check advisory** - active exploitation of a third-party runtime; alert text does not state FortiOS exposure (see FG-IR-26-187) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | none listed in feed text | PHP applications | **no** - not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/php2shell |

No FortiOS / FortiGate / FortiAP / FortiProxy **Outbreak Alert** was published in the last 30 days, and neither feed contained any FortiAP item (0 occurrences of "FortiAP" in `ir.xml` and `outbreakalert.xml`). Managed FortiAP firmware was not supplied in this run's context, so no FortiAP firmware comparison was performed.

## Assessment and recommended actions

1. **Upgrade path (primary action): FortiOS 7.4.11 -> 7.4.13.** This single step closes all four confirmed-affected advisories (FG-IR-26-268, FG-IR-26-267, FG-IR-26-273, FG-IR-26-269). No other 7.4.x advisory in the window requires a different target release.
2. **Priority drivers.** FG-IR-26-268 (9.6) and FG-IR-26-267 (9.3) sit in the routing daemons - verify BGP/iBGP neighbours and OSPF areas on this box are limited to trusted peers, and restrict the FGSP/HA and FGFM attack surface of FG-IR-26-273 (9.0) by ensuring HA/FGSP peer and `standalone-config-sync`/autoscale traffic is confined to dedicated, non-routable links until patched.
3. **FG-IR-26-269 (sshd, 7.4)** - if administrative SSH is reachable, restrict `set admin-ssh-*` / admin access to management VDOMs and trusted hosts as an interim control.
4. **FG-IR-26-193 (SSL-VPN)** - already remediated at 7.4.11, so no action; if SSL-VPN web-mode portal-to-backend is used, note that from 7.4.10 onward the certificate check is always enabled and cannot be disabled.
5. **FG-IR-26-225** - confirm whether fw-maniak-hq is centrally managed. If it is, the managing FortiManager/FortiManager Cloud must be at 7.4.12+/7.6.8+/8.0.2+; the FortiGate itself needs no separate fix.
6. **Open items (feed text insufficient)** - FG-IR-26-188 (Linux kernel RCE) and FG-IR-26-187 (Node2Shell) publish an empty CSAF product tree, so FortiOS exposure cannot be confirmed or ruled out from the feeds fetched here. Review the advisory pages before deciding they are irrelevant.

## Method / provenance notes

- Every row above was extracted programmatically from the two RSS feeds fetched in this session; nothing was taken from memory.
- CVE ids, CVSS scores, `known_affected` / `known_not_affected` ranges and "Upgrade to ..." text were cross-checked against each advisory's CSAF JSON on filestore.fortinet.com.
- "Affects 7.4.11?" decision rule: `yes` only when FortiOS appears in CSAF `known_affected` with a range covering 7.4.11; `no` when FortiOS 7.4 is in `known_not_affected`, listed as "Not Applicable", fixed below 7.4.11, or when the vulnerable product is not a Fortinet product in the advisory; `check advisory` when the feed/CSAF names no Fortinet product or version range.
- Read-only run: no configuration, firmware or policy change was made to the firewall. Only status/usage endpoints were queried.
