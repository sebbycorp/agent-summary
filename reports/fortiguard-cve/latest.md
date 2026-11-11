# fortiguard-cve - 2026-11-11

**Verdict: ACTION NEEDED** — FortiOS v7.4.11 build 2878 on fw-maniak-hq (FGT80F, serial FGT80FTK22061709) is listed as affected by FG-IR-26-267 (CVSS 9.3, CVE-2026-126253) and FG-IR-26-269 (CVSS 7.4, CVE-2026-126255); both are fixed in FortiOS 7.4.13. Plan an upgrade to 7.4.13+ (read-only check: nothing was changed on the firewall).

## Device under review (source: fg_system_status)

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | v7.4.11 build 2878 |
| vDOM | root |
| Load at check | CPU 100% idle, mem ~1.49 GB used / 3.90 GB |

## Feeds fetched in this session (both reachable)

- PSIRT advisories: `https://filestore.fortinet.com/fortiguard/rss/ir.xml` — 50 items, feed build 2026-11-11 14:15 UTC, all items dated 2026-11-10 (all inside the 30-day window).
- Outbreak Alerts (the https://fortiguard.fortinet.com/outbreak-alert page): `https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml` — 20 items, 3 inside the last 30 days.
- Affected-version ranges and CVE ids were read from the matching CSAF JSON files on `https://filestore.fortinet.com/fortiguard/psirt/` for FG-IR-26-267, FG-IR-26-269, FG-IR-26-195 and FG-IR-26-225.

## PSIRT advisories (last 30 days) mentioning FortiOS / FortiGate / FortiAP / FortiProxy

| Date | Advisory | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness in FortiOS/FortiSASE ospf daemon (CVSS 9.3 CRITICAL) | CVE-2026-126253 | FortiOS, FortiSASE | **yes** — CSAF lists `FortiOS >=7.4.0 <=7.4.12` as known affected; fix = FortiOS 7.4.13+ | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in ssh daemon allowing unauthenticated authentication bypass (CVSS 7.4 HIGH) | CVE-2026-126255 | FortiOS, FortiSASE, FortiProxy, FortiPAM | **yes** (conditional) — CSAF lists `FortiOS >=7.4.0 <=7.4.12` as known affected; fix = FortiOS 7.4.13+. Advisory note: "the product is impacted only if GPC OS Login is enabled" — verify that setting | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-195 — Improper authentication in FortiOS Remote Radius Type admin two-factor (TFA) (CVSS 5.9 MEDIUM) | CVE-2026-121530 | FortiOS | **no** — CSAF marks `FortiOS 7.4 all versions`, `7.2 all versions` and `8.0 all versions` as known *not* affected (remediation "FortiOS 7.4: Not Applicable"); only FortiOS 7.6.0–7.6.6 require 7.6.7+ | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection in FortiManager: low-privileged Device Manager admin can obtain super-admin on **managed FortiGates** (CVSS 8.4 HIGH) | CVE-2026-126180 | FortiManager, FortiManager Cloud (attack path against managed FortiGates) | **check advisory** — FortiOS is not the vulnerable product; exposure exists only if this gate is device-managed by a FortiManager on 7.4.0–7.4.11 (fix: FortiManager 7.4.12+) | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |

No item in the fetched PSIRT feed mentioned **FortiAP**, and no managed-FortiAP firmware context was supplied, so no FortiAP rows are reported.

## Outbreak Alerts (last 30 days)

No Fortinet-product outbreak alert was published in the window — a keyword scan (Fortinet / FortiOS / FortiGate / FortiAP / FortiProxy / FortiClient / FortiManager) across all alert descriptions returned 0 hits; the three recent alerts concern third-party software only.

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Google Brotli decoder / Node.js runtimes | no — FortiOS not named in the alert | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | none stated in the feed item | PHP application servers | no — FortiOS not named in the alert | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Data Center (Bitbucket, Confluence, Jira, Bamboo, Crowd, Crucible, Fisheye) | no — FortiOS not named in the alert | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

## Recommended actions (for a change window — not performed by this read-only check)

1. Schedule an upgrade of fw-maniak-hq from FortiOS 7.4.11 build 2878 to **7.4.13 or later** — this closes FG-IR-26-267 (CVE-2026-126253, ospfd authentication bypass, CVSS 9.3) and FG-IR-26-269 (CVE-2026-126255, sshd path traversal / authentication bypass).
2. Until patched: restrict OSPF to trusted interfaces/peers, and for FG-IR-26-269 confirm whether **GPC OS Login** is enabled (if it is off, exposure is nil).
3. If this FortiGate is device-managed, check the FortiManager version against FG-IR-26-225 (CVE-2026-126180) and move to FortiManager 7.4.12+ if it is on 7.4.x below 7.4.12.
4. No outbreak alert requires action on this FortiGate this period.

_Report generated 2026-11-11 (America/Toronto) by the read-only FortiGuard CVE watch. Every row above comes only from the two feeds fetched in this run; nothing on the firewall was modified._
