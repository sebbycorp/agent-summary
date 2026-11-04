# fortiguard-cve - 2026-11-04

**Verdict: Review** — two FortiOS advisories were published in the last 30 days, but neither feed entry states affected FortiOS version ranges, so confirm on the advisory page whether FortiOS 7.4.11 is fixed; no actively-exploited FortiOS / FortiGate / FortiAP vulnerability and no Fortinet-product Outbreak Alert in the window.

## Device under watch

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | **v7.4.11 build 2878** |
| vDOM | root |
| Health | CPU ~0% user / 100% idle, mem 1.48 GB used of 3.90 GB — normal |

Checked against: FortiOS 7.4.11 (build 2878). FortiAP firmware was not supplied in this run's context and is not reported by `fg_system_status`; note that **zero** FortiAP advisories appear in the PSIRT feed window, so no FortiAP-side action is indicated.

## Feeds fetched this session

| Feed | URL | Status | Items | Feed built |
|---|---|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK (49,263 B) | 50 | Wed, 04 Nov 2026 14:15:08 +0000 |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | OK (17,411 B) | 20 | Wed, 04 Nov 2026 14:15:07 +0000 |

Both feeds reachable — no gaps this run. Window analysed: **2026-10-05 → 2026-11-04** (30 days, America/Toronto).

## In-scope items: FortiOS / FortiGate / FortiAP / FortiProxy SSL-VPN (last 30 days)

| Date | Advisory id / alert | CVE ids | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-13 | FG-IR-26-181 — "GUI Denial of Service" (CVSSv3 6.7) | none listed in feed | FortiOS | **check advisory** — feed text names FortiOS but gives no version range | https://fortiguard.fortinet.com/psirt/FG-IR-26-181 |
| 2026-10-13 | FG-IR-26-182 — "Double free in signal handler" (CVSSv3 2.1) | none listed in feed | FortiOS, FortiProxy, FortiSwitch | **check advisory** — feed text names FortiOS but gives no version range | https://fortiguard.fortinet.com/psirt/FG-IR-26-182 |

Advisory detail taken verbatim from the feed description:

- **FG-IR-26-181** — "An Improper Locking vulnerability [CWE-667] in FortiOS may allow an unauthenticated attacker with network access to the device to cause a denial of service of the HTTPS management interface via crafted HTTP requests." (Revised 2026-10-13.) Exposure of the HTTPS GUI to untrusted networks is the risk driver — confirm 7.4.11 build 2878 is in the fixed list.
- **FG-IR-26-182** — "A Double Free vulnerability [CWE-415] in FortiOS, FortiProxy and FortiSwitch may allow a local authenticated attacker to crash the newcli daemon via crafted CLI commands." (Revised 2026-10-13.) Low severity, requires local authenticated CLI access.

## Outbreak Alerts concerning Fortinet products (last 30 days)

**None.** The 3 alerts published in the window all concern third-party software and do not name FortiOS, FortiGate, FortiAP or FortiProxy (verified by keyword scan: the whole feed contains 0 occurrences of "FortiOS", "FortiGate", "FortiAP", "FortiProxy"):

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 (Brotli) | Node.js | no — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | CVE-2026-109618, CVE-2026-109619 | PHP | no — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Data Center products | no — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

## Other PSIRT advisories in the window (published, but not FortiOS/FortiGate/FortiAP)

Listed for completeness so the record is transparent — none of these feed entries names FortiOS, FortiGate, FortiAP or FortiProxy, so none is flagged against 7.4.11:

| Date | Advisory id | CVE ids | Product (per feed) | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-02 | FG-IR-26-188 | CVE-2026-98432 | Linux kernel (no Fortinet product named in feed) | check advisory | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-26 | FG-IR-26-187 | CVE-2026-105482, CVE-2026-105483, CVE-2026-106069, CVE-2026-111497, CVE-2026-105481, CVE-2026-106072, CVE-2026-106074, CVE-2026-106070, CVE-2026-106071, CVE-2026-106073 | Node.js 22.x/24.x/26.x | no (per feed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-10-21 | FG-IR-26-186 | CVE-2026-109618, CVE-2026-109619 | PHP core | no (per feed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-186 |
| 2026-10-13 | FG-IR-26-180 | none in feed | FortiSIEM | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-180 |
| 2026-10-13 | FG-IR-26-183 | none in feed | FortiSIEM GUI | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-183 |
| 2026-10-13 | FG-IR-26-179 | none in feed | FortiSandbox / FortiSandbox Cloud API | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-179 |
| 2026-10-13 | FG-IR-26-185 | none in feed | FortiSandbox GUI | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-185 |
| 2026-10-13 | FG-IR-26-178 | none in feed | FortiAnalyzer, FortiManager, FortiManager Cloud, FortiAnalyzer Cloud | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-178 |
| 2026-10-13 | FG-IR-26-176 | none in feed | FortiSandbox | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-176 |
| 2026-10-13 | FG-IR-26-184 | none in feed | FortiMail webmail | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-184 |
| 2026-10-13 | FG-IR-26-177 | none in feed | FortiManager fgfmd daemon | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-177 |

## Watch item just outside the 30-day window

| Date | Advisory id | CVE ids | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-01 (revised 2026-10-07) | FG-IR-26-175 — path traversal + NULL-byte, CVSSv3 9.8, feed states **"reported to be exploited in the wild"** | none listed in feed | feed does not name the product | **check advisory** | https://fortiguard.fortinet.com/psirt/FG-IR-26-175 |

Published 4 days before the window cut-off and revised inside it; the feed description does not name the affected product, so open the link and confirm whether it concerns FortiOS 7.4.x before dismissing it.

## Notes on method / limits

- Every row above is taken from the two feeds fetched in this run; no advisory or CVE id is invented. Where the feed supplies no CVE id, the cell says "none listed in feed" rather than guessing.
- Neither in-scope advisory's feed text includes an affected-version range (e.g. "FortiOS 7.4.0 through 7.4.x") — the RSS description carries the CVSS score, CWE and prose only. Both are therefore "check advisory", not "yes"/"no".
- This check was strictly read-only: `fg_system_status` only. No firewall configuration, firmware or runtime state was modified.

## Recommended next actions (for the firewall owner, not performed here)

1. Open FG-IR-26-181 and FG-IR-26-182 and confirm the fixed-release table against FortiOS 7.4.11 build 2878.
2. Restrict the HTTPS management interface to trusted mgmt subnets/ACLs — this is the practical mitigation for FG-IR-26-181 regardless of fix status.
3. Check FG-IR-26-175's product scope given the in-the-wild note.
