# fortiguard-cve - 2026-11-14

**Verdict: ACTION NEEDED — 3 FortiOS PSIRT advisories published in the last 30 days list FortiOS 7.4.11 (our running build) as "Known Affected", including two critical (CVSS 9.6 / 9.3) routing-daemon flaws with no virtual patch for OSPF.**

## Target device

| Item | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F), serial FGT80FTK22061709 |
| FortiOS | **v7.4.11 build 2878** |
| VDOM | root |
| Load at check time | CPU 0% (idle 100%), mem ~1.5 GB / 3.9 GB |
| FortiAP firmware | Managed FortiAPs (e.g. FP231F v7.4.x) — no FortiAP/FortiWLAN advisory appeared in either feed today, so nothing to check on the AP side; supply the AP model/firmware build if you want it tracked explicitly |

## Feeds fetched (this session, both reachable)

| Feed | URL | Items | In 30-day window (2026-10-15 → 2026-11-14) |
|---|---|---|---|
| FortiGuard PSIRT advisories (RSS) | https://filestore.fortinet.com/fortiguard/rss/ir.xml | 50 | 50 (49 published 2026-11-10, 1 on 2026-10-26) |
| FortiGuard Outbreak Alerts (RSS) | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | 20 | 3 (2026-10-27 ×2, 2026-10-19 ×1) |

The RSS `description` text does **not** contain CVE ids or fixed-version ranges for these advisories, so for each in-window item affecting our product family the matching CVRF document was fetched from `https://fortiguard.fortinet.com/psirt/cvrf/FG-IR-…` and the CVE id plus the `ProductStatuses / Known Affected` product-version list were read from there. That is the basis of every "affects 7.4.11?" answer below — nothing is inferred.

## FortiOS / FortiGate / FortiAP / FortiProxy items — last 30 days

| Date (pubDate) | Advisory | CVE id | Product (as stated in advisory) | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 (rev 2026-11-13) | FG-IR-26-268 — Numeric truncation error, FortiOS/FortiSASE **bgp daemon**; CVSS 9.6, virtual patch: **yes** | CVE-2026-126254 | FortiOS, FortiSASE (Known Affected: FortiOS 7.2.0–8.0.1 incl. 7.4.0–**7.4.12**) | **YES** | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness, FortiOS/FortiSASE **ospf daemon**; CVSS 9.3, virtual patch: **no** | CVE-2026-126253 | FortiOS, FortiSASE (Known Affected: FortiOS 7.0.1–8.0.1 incl. 7.4.0–**7.4.12**) | **YES** | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in **ssh daemon**, unauthenticated auth bypass; CVSS 7.4, virtual patch: **no** | CVE-2026-126255 | FortiOS, FortiSASE, FortiProxy, FortiPAM (Known Affected: FortiOS 7.2.0–8.0.1 incl. 7.4.0–**7.4.12**) | **YES** | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 (rev 2026-11-12) | FG-IR-26-193 — **SSL-VPN** improper certificate validation → MitM on the portal↔backend channel; CVSS 7.7 | CVE-2026-121528 | FortiOS SSLVPN (Known Affected: 6.4.0–**7.4.9**, 7.6.0–7.6.4 — 7.4.10/11/12 not listed) | **NO** (7.4.11 not in the affected list; fixed earlier in the 7.4 branch) | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in Remote **RADIUS** admin two-factor (TFA) GUI login; CVSS 5.9 | CVE-2026-121530 | FortiOS (Known Affected: **7.6.0–7.6.6 only**) | **NO** (7.4 branch not listed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection in FortiManager GUI: a low-privileged Device Manager admin can obtain **super admin on managed FortiGates**; CVSS 8.4 | CVE-2026-126180 | FortiManager, FortiManager Cloud (7.2.0–8.0.1) — no FortiOS/FortiGate product entry | **CHECK advisory** — FortiOS 7.4.11 itself is not the affected product, but this box is a *target* of it if it is registered to a FortiManager | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-10 | FG-IR-26-275 — FGFM: improper authorization **before the registration password is verified** → code/command execution from anyone who can reach the FGFM port; CVSS 9.0 | not published in the feed/CVRF | FortiManager FGFM — no FortiOS/FortiGate product entry | **CHECK advisory** — relevant to this FortiGate only via its FGFM (541/541-tcp) exposure to a FortiManager; confirm the FGFM interface is restricted | https://fortiguard.fortinet.com/psirt/FG-IR-26-275 |
| 2026-10-26 (rev 2026-11-13) | FG-IR-26-187 — "Node2Shell" Node.js CVE bundle; CVSS 9.8 | CVE-2026-105481 / -105482 / -105483 / -106069 / -106070 / -106071 / -106072 / -106073 / -106074 / -111497 | Node.js 22.x/24.x/26.x — **no FortiOS, FortiGate or FortiAP product entry** | **NO** (third-party component, not FortiOS) | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |

### Outbreak Alerts — last 30 days

No Outbreak Alert in the window concerns a Fortinet product. The three in-window items are all third-party, and none of them touches FortiOS / FortiGate / FortiAP / FortiProxy:

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 (Brotli / Node.js) | Node.js | NO — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | none stated in the feed | PHP applications | NO — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Data Center products | NO — not a Fortinet product | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

The remaining 17 Outbreak Alerts in the feed pre-date the 30-day window (oldest 2026-02-03).

## Items reviewed and excluded as out of scope

The 2026-11-10 PSIRT batch also contains ~40 advisories for **FortiMail**, **FortiManager / FortiManager Cloud**, **FortiAnalyzer / FortiAnalyzer Cloud**, **FortiAuthenticator**, **FortiRecorder**, **FortiVoice** and **FortiSASE-Sovereign** (e.g. FG-IR-26-199, -200, -216 FortiMail unauthenticated RCE/SQLi at CVSS 9.8/9.8/9.4; FG-IR-26-229, -231, -232, -233, -235, -236, -237, -239, -241, -244, -246, -247, -249, -250, -251 FortiManager/FortiAnalyzer family; FG-IR-26-255, -256, -258, -263, -266, -271 FortiAuthenticator). None of them lists FortiOS, FortiGate, FortiAP or FortiProxy in its Known Affected product set, so they are not exposures for this FortiGate — **but if fw-maniak-hq is centrally managed by a FortiManager, the FortiManager/FortiAnalyzer cluster above (plus FG-IR-26-225 and FG-IR-26-275) becomes relevant and should be checked against the FortiManager's own version.**

## Recommended actions (this agent is read-only — nothing was changed)

1. **Plan a FortiOS upgrade off 7.4.11.** FG-IR-26-267, -268 and -269 all list FortiOS 7.4.0 through **7.4.12** as affected, so the fix for this batch sits above 7.4.12 — moving to the current 7.4.x maintenance release (or a supported later branch per your upgrade policy) is the only complete remediation. Check the FortiManager compatibility matrix before scheduling.
2. **FG-IR-26-267 (CVSS 9.3, OSPF, no virtual patch) is the priority.** Its vector is `AV:A/…/PR:N` — adjacent-network, no privileges. If OSPF is not in use, confirm no OSPF interface/area is configured (`router ospf`); if it is in use, restrict OSPF neighbours to trusted interfaces/peers until patched.
3. **FG-IR-26-268 (CVSS 9.6, BGP)** — the CVRF states a virtual patch exists (IPS signature; revision "Added IPS package info" on 2026-11-13): verify the IPS signature database is current and in **native/default-mode block**, and restrict iBGP/eBGP peers to configured neighbours only.
4. **FG-IR-26-269 (CVSS 7.4, admin SSH auth bypass)** — ensure SSH admin access is restricted to management interfaces and trusted admin hosts, since there is no virtual patch.
5. FG-IR-26-193 (SSL-VPN) and FG-IR-26-195 (RADIUS admin TFA) do **not** apply to 7.4.11 per the CVRF affected lists — no action beyond normal patching.
6. Re-run this check daily; the 2026-11-10 batch is only four days old and PSIRT is still revising these entries (FG-IR-26-193 and FG-IR-26-268 were both revised after initial publication).

---
*Generated read-only by fortiguard_cve_watch on 2026-11-14 (America/Toronto). Sources fetched this session: https://filestore.fortinet.com/fortiguard/rss/ir.xml, https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml and the CVRF documents https://fortiguard.fortinet.com/psirt/cvrf/FG-IR-26-187, -193, -195, -225, -267, -268, -269, -275. Both feeds were reachable; every advisory and CVE id above appears verbatim in that content.*
