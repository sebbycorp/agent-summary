# fortiguard-cve - 2026-11-09

**Verdict: action needed (low urgency)** — one in-window advisory (FG-IR-26-182 / CVE-2026-106175) explicitly lists FortiOS 7.4 as affected, but it is LOW severity (CVSS 2.1) and needs a *local authenticated* CLI user; no Fortinet Outbreak Alert fired, and no remote-code / SSL-VPN issue in this window touches 7.4.11 — so plan a 7.4 maintenance release rather than an emergency upgrade.

## Asset under review

| Field | Value | Source |
|---|---|---|
| Hostname | fw-maniak-hq | `fg_system_status` |
| Model | FortiGate 80F (FGT80F) | `fg_system_status` |
| Serial | FGT80FTK22061709 | `fg_system_status` |
| Running FortiOS | **v7.4.11 build 2878** | `fg_system_status` |
| VDOM | root | `fg_system_status` |
| Load at check | CPU ~100% idle, mem 1.5 GB / 3.9 GB | `fg_system_status` |

**FortiAP:** no managed-FortiAP / FortiAP firmware context was supplied for this check, so no FortiAP firmware could be matched. The PSIRT feed contained **no FortiAP-specific advisory** at all in this window. The one advisory that does name FortiAP (FG-IR-26-186, PHP2Shell) lists FortiAP under *"Products confirmed NOT Impacted"*. If the FortiAPs are FP231F on a v7.4.x build, nothing in this window requires action against them.

## Feeds fetched in this session (both reachable)

| Feed | URL | Items |
|---|---|---|
| PSIRT advisories (ir.xml) | https://filestore.fortinet.com/fortiguard/rss/ir.xml | 50 |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | 20 |

Per-item CSAF JSON for the FortiOS candidates was pulled from `https://filestore.fortinet.com/fortiguard/psirt/csaf_*.json` to obtain the CVE IDs and the exact affected-version ranges, because the RSS `<description>` carries neither.

## In-scope items — last 30 days (2026-10-10 → 2026-11-09), FortiOS / FortiGate / FortiAP / FortiProxy

| Date | Advisory / Alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-13 | FG-IR-26-182 — Double free in signal handler (CVSS 2.1 LOW) | CVE-2026-106175 | FortiOS, FortiProxy, FortiSwitch | **yes** — CSAF `known_affected` includes "FortiOS 7.4 all versions"; fix guidance is "FortiOS 7.4: Migrate to a fixed release" | https://fortiguard.fortinet.com/psirt/FG-IR-26-182 |
| 2026-10-13 | FG-IR-26-181 — GUI Denial of Service (CVSS 6.7 MEDIUM) | CVE-2026-106174 | FortiOS | **no** — CSAF `known_not_affected` lists "FortiOS 7.4 all versions"; remediation states "FortiOS 7.4: Not Applicable" (affected: 8.0.0, >=7.6.4<=7.6.6) | https://fortiguard.fortinet.com/psirt/FG-IR-26-181 |
| 2026-10-21 | FG-IR-26-186 — PHP2Shell core RCE patches (CVSS 5.3; revised 2026-11-05) | CVE-2026-109618, CVE-2026-109619, CVE-2026-115560 | FortiSOAR / FortiSOAR PaaS — **FortiOS and FortiAP both on the "confirmed NOT Impacted" list** | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-186 |
| 2026-10-26 | FG-IR-26-187 — Node2Shell CVEs (CVSS 9.8) | CVE-2026-105481/105482/105483/106069/106070/106071/106072/106073/106074, CVE-2026-111497 | Node.js 22.x/24.x/26.x — CSAF product tree is empty (no Fortinet product listed) | **no** | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-11-02 | FG-IR-26-188 — Linux kernel RCE (CVSS 9.8) | CVE-2026-98432 | Linux kernel (TCP/IP `inet_opt`/`ireq_opt` UAF) — CSAF product tree is empty (no Fortinet product listed) | **check advisory** — the feed/CSAF name no Fortinet product or version range, so FortiOS 7.4.11 cannot be cleared from the feed text alone | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |

## Out-of-scope Fortinet products, same window (context only — none runs on this FortiGate)

These are published advisories for Fortinet products that are **not** FortiOS/FortiGate/FortiAP/FortiProxy, so they do not expose fw-maniak-hq. Listed so the table below is complete against the feed; no CVE id is printed for any item whose feed text does not carry one.

| Date | Advisory | CVE id | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-13 | FG-IR-26-180 — Arbitrary file write in grpc phQueryMaster (CVSS 9.4) | not in feed — check advisory | FortiSIEM | no (not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-180 |
| 2026-10-13 | FG-IR-26-185 — Improper access control / session mgmt in web mgmt UI (CVSS 9.6) | not in feed — check advisory | FortiSandbox GUI | no (not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-185 |
| 2026-10-13 | FG-IR-26-178 — Multiple arbitrary file read/write via SQL injection (CVSS 7.9) | not in feed — check advisory | FortiAnalyzer, FortiManager, FortiManager Cloud, FortiAnalyzer Cloud | no (not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-178 |
| 2026-10-13 | FG-IR-26-184 — SQL command injection in webmail (CVSS 7.7) | not in feed — check advisory | FortiMail webmail | no (not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-184 |
| 2026-10-13 | FG-IR-26-177 — Arbitrary command execution in fgfmsd daemon (CVSS 5.3) | not in feed — check advisory | FortiManager fgfmd | no (not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-177 |
| 2026-10-13 | FG-IR-26-179 — Fabric API authentication bypass (CVSS 4.9) | not in feed — check advisory | FortiSandbox, FortiSandbox Cloud API | no (not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-179 |
| 2026-10-13 | FG-IR-26-176 — Persistent DoS in Admin Profile (CVSS 4.8) | not in feed — check advisory | FortiSandbox | no (not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-176 |
| 2026-10-13 | FG-IR-26-183 — Broken authorization (CVSS 3.4) | not in feed — check advisory | FortiSIEM GUI | no (not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-183 |

## Outbreak Alerts (fortiguard.fortinet.com/outbreak-alert) — last 30 days

**No Fortinet-product Outbreak Alert in the window.** Three alerts fall inside 2026-10-10 → 2026-11-09 and all target third-party software, so none requires action on this FortiGate:

| Date | Alert | CVE id | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Google Brotli decoder / Node.js HTTP clients | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | not stated in feed | PHP applications | no — FortiOS/FortiAP confirmed NOT Impacted by FG-IR-26-186 | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Data Center (Bitbucket, Confluence, Jira, Bamboo, Crowd, Crucible, Fisheye) | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

The oldest Outbreak Alert still in the feed is dated 2026-02-03 (Versa Concerto SD-WAN); the newest Fortinet-relevant alert content in the feed is informational only.

## Analysis / recommended follow-up

- **FG-IR-26-182 (CVE-2026-106175) is the only row that touches us.** Attack surface is a **local, authenticated, high-privilege CLI user** issuing crafted commands to crash `newcli` (CVSS vector `AV:L/AC:L/PR:H/UI:N/S:U/C:N/I:N/A:L`). No remote or unauthenticated path, no confidentiality/integrity impact. Risk to fw-maniak-hq is low while administrative access is restricted; the practical exposure is a malicious or compromised admin account. Fortinet's guidance for 7.4 is "Migrate to a fixed release", i.e. there is no 7.4.11 build that carries the fix — so the close-out action is to move to the current fixed 7.4 maintenance release (or to 7.6.x) at the next change window, not an emergency patch.
- **FG-IR-26-181 (CVE-2026-106174) cleared.** Remote DoS against the HTTPS management GUI, but the CSAF places 7.4 and 7.2 in `known_not_affected` and marks FortiOS 7.4 "Not Applicable". Only 8.0.0 and 7.6.4–7.6.6 are affected. No action; note the virtual patch FMWP db 26.073 exists if the unit is ever moved to an affected branch.
- **No SSL-VPN / IPsec / administrative-API remote-code advisory against FortiOS 7.4 appeared in this window**, and there is no Fortinet Outbreak Alert — so nothing here indicates active exploitation targeting this model.
- **FG-IR-26-188 stays open as "check advisory".** It is a CVSS 9.8 use-after-free in the Linux kernel IPv4 SYN-ACK path. Fortinet published the advisory but the fetched CSAF lists **no affected Fortinet product**, so the feed text neither confirms nor denies FortiOS exposure. Do not treat it as cleared — verify the FortiOS status on the advisory page, and confirm the FortiGuard IPS/AV signature packages are current on the unit in the meantime.
- Confirm the FortiGuard agent on the unit is updating normally — IPS/AV/app-control currency is the mitigation for the informative third-party CVEs (Node2Shell, PHP2Shell, HTTP/2-class issues) that Fortinet has cleared as non-impacting but still ships signatures for.

*Generated read-only. Sources: `fg_system_status`, FortiGuard RSS `ir.xml`, FortiGuard RSS `outbreakalert.xml`, and FortiGuard-hosted CSAF JSON, all fetched 2026-11-09 (America/Toronto). No advisory, CVE id, or version range in this report was taken from memory; both feeds were reachable and no feed was skipped. No change was made to the firewall.*
