# fortiguard-cve - 2026-11-18

**Verdict: ACTION NEEDED — 4 of the 6 FortiOS PSIRT advisories published in the last 30 days list FortiOS 7.4.0 through 7.4.12 as affected, so fw-maniak-hq (FortiGate 80F, FortiOS v7.4.11 build 2878) is exposed. Plan an upgrade to FortiOS 7.4.13.**

## Target

| Item | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F), serial FGT80FTK22061709 |
| FortiOS | v7.4.11 (build 2878), vdom root |
| Managed FortiAPs | FortiAP firmware (e.g. FP231F v7.4.x) — no FortiAP/FortiWiFi item appeared in either feed today, so no FortiAP action this run |
| Health | CPU idle, mem 38%, sessions ~743 — no anomaly that would hint at exploitation |

## Feeds fetched this session

| Feed | URL | Status |
|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK (50 items) |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml (page: https://fortiguard.fortinet.com/outbreak-alert) | OK (20 items) |

Per-advisory CSAF JSON files were fetched from https://filestore.fortinet.com/fortiguard/psirt/ to confirm exact `known_affected` / `known_not_affected` version ranges instead of guessing from the RSS blurb. No feed was unreachable.

Window applied: pubDate on or after 2026-10-19 (last 30 days).

## FortiOS / FortiGate / FortiAP items in the last 30 days

| Date | Advisory / Alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 — Numeric truncation error (bgpd), CVSS 9.6, rev 2026-11-13 | CVE-2026-126254 | FortiOS + FortiSASE | **yes** (affected 7.4.0–7.4.12; fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Auth bypass by primary weakness (ospfd), CVSS 9.3 | CVE-2026-126253 | FortiOS + FortiSASE | **yes** (affected 7.4.0–7.4.12; fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-273 — Unauthenticated command execution via HA (FGSP / standalone-config-sync / autoscale), CVSS 9.0, rev 2026-11-16 | CVE-2026-126259 | FortiOS | **yes** (affected 7.4.0–7.4.12; fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 | FG-IR-26-269 — Path traversal / auth bypass in ssh daemon, CVSS 7.4 | CVE-2026-126255 | FortiOS + FortiSASE + FortiPAM | **yes** (affected 7.4.0–7.4.12; fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 — SSL-VPN improper certificate validation (MiTM on SSL-VPN web portal ↔ backend), CVSS 7.7, rev 2026-11-12 | CVE-2026-121528 | FortiOS SSL-VPN | no (7.4.0–7.4.9 affected, fixed in 7.4.10 — 7.4.11 is already past the fix) | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in admin RADIUS two-factor (TFA) auth, CVSS 5.9 | CVE-2026-121530 | FortiOS admin GUI | no (CSAF lists only 7.6.0–7.6.6 affected; "FortiOS 7.4 all versions" is in known_not_affected) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection in FortiManager → super-admin on managed FortiGates, CVSS 8.4 | CVE-2026-126180 | FortiManager / FortiManager Cloud | check advisory (our FortiOS is the *target*, not the vulnerable product; only relevant if a FortiManager manages this gate — none reported by the API) | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-02 | FG-IR-26-188 — "Linux kernel RCE", CVSS 9.8 (TCP/IP inet_opt/ireq_opt use-after-free) | CVE-2026-98432 | Linux kernel (upstream); CSAF product tree is **empty** — no Fortinet product named | check advisory (feed text does not name FortiOS/FortiGate/FortiAP; confirm on the advisory page whether FortiOS ships the vulnerable kernel code) | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-26 | FG-IR-26-187 — "Node2Shell CVEs", CVSS 9.8 | CVE-2026-105481, -105482, -105483, -106069, -106070, -106071, -106072, -106073, -106074, -111497 | Node.js 22.x/24.x/26.x (Brotli decoder OOB write); CSAF product tree is **empty** | check advisory (feed text says Node.js only, not FortiOS/FortiGate/FortiAP) | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-10-27 | Outbreak Alert — Node2Shell Remote Code Execution | CVE-2026-111497 (Google Brotli decoder in Node.js) | Third-party (Node.js) — not a Fortinet product | no | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | Outbreak Alert — PHP2Shell Remote Code Execution Vulnerabilities | none stated in feed | Third-party (PHP) — not a Fortinet product | no | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Outbreak Alert — Atlassian Arbitrary File Access Vulnerability | none stated in feed | Third-party (Atlassian) — not a Fortinet product | no | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

Outbreak Alerts: no alert in the window names a Fortinet product (no Fortinet outbreak alert inside the 30-day window), so nothing on that side requires emergency action.

The other 43 PSIRT advisories published 2026-11-10 concern FortiManager, FortiAnalyzer, FortiMail, FortiAuthenticator, FortiVoice/FortiRecorder or FortiSASE only — none list FortiOS/FortiGate/FortiAP as the vulnerable product.

## Exposure summary for FortiOS v7.4.11

- **Confirmed affected (4):** FG-IR-26-268 (CVE-2026-126254, 9.6), FG-IR-26-267 (CVE-2026-126253, 9.3), FG-IR-26-273 (CVE-2026-126259, 9.0), FG-IR-26-269 (CVE-2026-126255, 7.4). All four fix at **FortiOS 7.4.13**.
- **Not affected (2):** FG-IR-26-193 (fixed in 7.4.10, we are on 7.4.11) and FG-IR-26-195 (7.4 explicitly listed as not affected).
- **Check advisory (3):** FG-IR-26-188 (Linux kernel), FG-IR-26-187 / Node2Shell Outbreak Alert (Node.js), FG-IR-26-225 (FortiManager-side).
- **FortiAP:** no FortiAP / FortiWiFi advisory in either feed today.

## Recommended next actions (read-only agent — nothing was changed on the firewall)

1. Schedule a maintenance window to upgrade fw-maniak-hq from **v7.4.11 → 7.4.13** (clears all four confirmed CVEs at once).
2. Until then, reduce exposure: keep HA/FGSP traffic on a dedicated isolated link, restrict BGP/OSPF neighbour relationships to trusted peers, and disable admin SSH from untrusted interfaces (FG-IR-26-273, -268, -267, -269 respectively).
3. Verify on the PSIRT pages whether FG-IR-26-188 and FG-IR-26-187 name FortiOS 7.4 in their product tables (both CSAF product trees came back empty today, so the feeds alone do not answer it).
4. If any FortiManager manages this gate, patch FortiManager for FG-IR-26-225 (CVE-2026-126180) — a compromised FMG yields super-admin on managed FortiGates.

---
*Generated 2026-11-18 by fortigate_cve_watch (read-only). Every row above was taken from the two FortiGuard feeds and the matching FortiGuard CSAF JSON fetched in this run; no advisory or CVE id was invented.*
