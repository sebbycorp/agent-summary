# fortiguard-cve - 2026-11-13

**Verdict: ACTION NEEDED** — three FortiOS PSIRT advisories published 2026-11-10 list FortiOS 7.4.x up to 7.4.12 as affected (CVSS 9.6 / 9.3 / 7.4); all three are fixed in FortiOS 7.4.13, so fw-maniak-hq on v7.4.11 build 2878 requires an upgrade.

## Asset under review

| Field | Value | Source |
|---|---|---|
| Hostname | fw-maniak-hq | fg_system_status |
| Model | FortiGate 80F (FGT80F), serial FGT80FTK22061709 | fg_system_status |
| FortiOS | **v7.4.11 build 2878** | fg_system_status |
| vDOM | root | fg_system_status |
| Health at check | CPU 100% idle, mem 1.51 GB / 3.90 GB used | fg_system_status |
| Managed FortiAPs | No FortiAP firmware version supplied in this check's context | — |

Check window: **2026-10-14 → 2026-11-13** (America/Toronto), i.e. last 30 days.

## Feeds fetched this session

| Feed | URL | Status |
|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | ✅ reachable (47,140 B, 50 items) |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | ✅ reachable (17,411 B, 20 items) |
| Per-advisory CSAF JSON | https://filestore.fortinet.com/fortiguard/psirt/csaf_*.json | ✅ reachable — used to resolve CVE IDs and exact affected version ranges |

Note: the RSS `<description>` text carries no CVE IDs and no version ranges, so CVE IDs and the "affects 7.4.11?" verdicts below were read from the official CSAF JSON linked by each feed item (`product_tree` version ranges + `remediations`). Nothing below is inferred from memory.

## PSIRT advisories — FortiOS / FortiGate / FortiAP / FortiProxy, last 30 days

All 50 IR feed items carry pubDate **Tue, 10 Nov 2026** (feed generated Fri, 13 Nov 2026 14:15:09 -0800), so every item is inside the 30-day window. Of those, 4 name FortiOS/FortiProxy directly and 2 more are FortiManager advisories with direct FortiGate impact.

| Date | Advisory | CVE | CVSSv3 | Product | Affected version range (CSAF) | Affects 7.4.11? | Link |
|---|---|---|---|---|---|---|---|
| 2026-11-10 (rev. 2026-11-13) | FG-IR-26-268 — Numeric truncation error in FortiOS/FortiSASE bgp daemon; a configured ibgp neighbor can send malicious/malformed attributes to all connected routers | CVE-2026-126254 | 9.6 | FortiOS, FortiSASE | FortiOS >=7.4.0 <=7.4.12 | **yes** — fixed in 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness in FortiOS/FortiSASE ospf daemon; unauthenticated attacker can broadcast LSA | CVE-2026-126253 | 9.3 | FortiOS, FortiSASE | FortiOS >=7.4.0 <=7.4.12 | **yes** — fixed in 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in the ssh daemon; unauthenticated attacker bypasses authentication via specially crafted credentials | CVE-2026-126255 | 7.4 | FortiOS, FortiSASE, FortiProxy, FortiPAM | FortiOS >=7.4.0 <=7.4.12 | **yes** — fixed in 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in FortiOS Remote Radius Type admin two-factor (TFA) auth: GUI login with an invalid token when the RADIUS server returns an extra "Access-Challenge" | CVE-2026-121530 | 5.9 | FortiOS | FortiOS >=7.6.0 <=7.6.6 only; CSAF remediation states "FortiOS 7.4: Not Applicable" | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection in FortiManager: a low-privileged Device Manager admin obtains super admin privilege **on managed FortiGates** via injected commands | CVE-2026-126180 | 8.4 | FortiManager, FortiManager Cloud (impact on managed FortiGates) | FortiManager >=7.4.0 <=7.4.11 | **check advisory** — affects this FortiGate only if it is device-managed by a FortiManager on 7.4.x ≤7.4.11 (fix 7.4.12); FortiOS itself is not in the CSAF product tree | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-10 | FG-IR-26-275 — Improper authorization in FortiManager FGFM: an unauthenticated attacker able to reach the FGFM port can execute code/commands via crafted FGFM packets | CVE-2026-126261 | 9.0 | FortiManager, FortiManager Cloud | FortiManager 7.4 — upgrade to 7.4.6 or above | **check advisory** — no FortiOS range in the CSAF; relevant here only via FGFM reachability from this device; verify FGFM is restricted to trusted managers | https://fortiguard.fortinet.com/psirt/FG-IR-26-275 |

**FortiAP:** the IR feed contains **zero** items naming FortiAP / FortiAP OS in this window, so no FortiAP-firmware exposure can be reported from this session's feed data. (No FortiAP firmware version was provided in the user context either.)

**FortiProxy SSL-VPN:** no FortiProxy SSL-VPN-specific advisory in the window. FG-IR-26-269 is the only item naming FortiProxy and it concerns the **sshd daemon**, not SSL-VPN. In that advisory's CSAF, explicit version ranges are published for FortiOS, FortiSASE and FortiPAM only — FortiProxy appears in the description text but carries no FortiProxy version range, so FortiProxy exposure is **check advisory**.

## Outbreak Alerts — last 30 days

The Outbreak Alerts feed contains **no alert about Fortinet products**. The three items inside the 30-day window are third-party product alerts, listed for completeness:

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Google Brotli decoder / Node.js | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | none stated in feed | PHP applications | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Data Center (Bitbucket, Confluence, Jira, Bamboo, Crowd, Crucible, Fisheye) | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

## Recommended actions (read-only report — nothing was changed on the firewall)

1. **Plan a FortiOS upgrade 7.4.11 → 7.4.13** for fw-maniak-hq. That single step clears all three "yes" rows (FG-IR-26-268, FG-IR-26-267, FG-IR-26-269), including the two critical-scored routing-protocol flaws.
2. **Exposure mitigation until the upgrade**, if it cannot happen immediately:
   - FG-IR-26-268 / FG-IR-26-267 concern the **bgpd / ospfd** routing daemons — restrict or remove untrusted iBGP neighbours and limit OSPF areas/interfaces; confirm no BGP/OSPF exposure to untrusted networks.
   - FG-IR-26-269 concerns the **sshd** daemon — keep SSH admin access locked to trusted management CIDRs / out-of-band access only.
3. **FGFM hygiene** for the two "check advisory" rows: confirm the FGFM port is reachable only from authorised FortiManager(s), and if a FortiManager manages this device, verify it runs 7.4.12 or above (FG-IR-26-225) / 7.4.6 or above (FG-IR-26-275).
4. FG-IR-295 needs no action on 7.4.11 (vendor marks FortiOS 7.4 "Not Applicable").
5. Re-run this check after upgrading to confirm the three affected advisories no longer apply.

---
*Generated 2026-11-13 by fortigate_cve_watch (read-only). Every row above is taken from the two FortiGuard RSS feeds and the per-advisory CSAF JSON fetched during this session; no advisory or CVE ID was supplied from memory.*
