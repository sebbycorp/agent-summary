# fortiguard-cve - 2026-11-24

**Verdict: ACTION NEEDED** — 4 FortiOS advisories published in the last 30 days affect FortiOS 7.4.11 (our running version); the fixed release for all four is FortiOS 7.4.13, so plan an upgrade of fw-maniak-hq.

## Device under review

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| Running FortiOS | **v7.4.11, build 2878** |
| VDOM | root |
| Checked at | 2026-11-24 14:30 UTC (09:30 America/Toronto) |

## Feeds fetched this session

| Feed | URL | Status |
|---|---|---|
| PSIRT advisories (ir.xml) | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK (50 items) |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml (page: https://fortiguard.fortinet.com/outbreak-alert) | OK (20 items) |
| Per-advisory CSAF JSON | https://filestore.fortinet.com/fortiguard/psirt/csaf_*.json | OK (used for CVE IDs + affected version ranges) |

Both feeds were reachable. Affected/not-affected verdicts below come from the vendor CSAF `known_affected` / `known_not_affected` ranges, not from inference.

## PSIRT advisories (last 30 days) touching FortiOS / FortiGate / FortiAP

| Date | Advisory | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-273 — Unauthenticated adjacent command execution via HA (CVSS 9.0) | CVE-2026-126259 | FortiOS (HA FGSP + standalone-config-sync / autoscale) | **yes** — CSAF affected `FortiOS >=7.4.0\|<=7.4.12`; fix 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 | FG-IR-26-268 — Numeric truncation error in bgp daemon (CVSS 9.6) | CVE-2026-126254 | FortiOS, FortiSASE | **yes** — affected `FortiOS >=7.4.0\|<=7.4.12`; fix 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness in ospf daemon (CVSS 9.3) | CVE-2026-126253 | FortiOS, FortiSASE | **yes** — affected `FortiOS >=7.4.0\|<=7.4.12`; fix 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in ssh daemon, auth bypass (CVSS 7.4) | CVE-2026-126255 | FortiOS, FortiSASE, FortiPAM | **yes** — affected `FortiOS >=7.4.0\|<=7.4.12`; fix 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 — SSLVPN improper certificate validation, MITM (CVSS 7.7) | CVE-2026-121528 | FortiOS SSL-VPN | **no** — affected `FortiOS >=7.4.0\|<=7.4.9`; 7.4.10 already fixed, we run 7.4.11 | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in Remote RADIUS admin TFA (CVSS 5.9) | CVE-2026-121530 | FortiOS admin 2FA | **no** — CSAF lists `FortiOS/ 7.4 all versions` as known_not_affected (only 7.6.0–7.6.6 affected) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |

## Outbreak Alerts (last 30 days) — Fortinet products

**None.** No Outbreak Alert in the fetched feed concerns FortiOS, FortiGate, FortiAP or FortiProxy. Alerts published inside the window are third-party and listed for awareness only (verdict: not applicable to this FortiGate):

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-19 | PHP2Shell Remote Code Execution Vulnerabilities | none listed in feed | third-party PHP applications | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-11-18 | Refrigeration Control Systems Attack | none listed in feed | Danfoss / Copeland / Dixell ICS | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/refrigeration-control-systems-attack |
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Node.js / Brotli decoder | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |

## FortiManager / FortiAnalyzer items relevant to this FortiGate

Not FortiOS bugs, so they do not make 7.4.11 vulnerable, but they change the risk around a FortiGate managed by FortiManager:

| Date | Advisory | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-225 — Template engine injection; a low-privileged Device Manager admin can gain **super admin on managed FortiGates** (CVSS 8.4) | CVE-2026-126180 | FortiManager, FortiManager Cloud | no (FortiOS not listed; FortiManager >=7.4.0\|<=7.4.11 affected) | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-10 | FG-IR-26-275 — FGFM code execution before registration password verified (CVSS 9.0) | CVE-2026-126261 | FortiManager | no (FortiOS not listed; FortiManager >=7.4.0\|<=7.4.5 affected) | https://fortiguard.fortinet.com/psirt/FG-IR-26-275 |
| 2026-11-10 | FG-IR-26-229 — Auth bypass by spoofing in fortilogd, HA disruption (CVSS 8.2) | CVE-2026-126184 | FortiAnalyzer / FortiManager families | no (FortiOS not listed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-229 |
| 2026-11-10 | FG-IR-26-247 — Improper certificate validation in RADIUS (CVSS 7.5) | CVE-2026-126232 | FortiAnalyzer / FortiManager families | no (FortiOS not listed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-247 |

## Excluded from the FortiOS table, with reason

- **No FortiAP and no FortiProxy SSL-VPN advisories** appeared in either feed during the window (searched `FortiAP`, `FortiProxy`, `FortiGate` across all 50 PSIRT items and 20 alerts). No FortiAP firmware context was supplied for this check, so no FortiAP verdict is issued.
- The remaining ~40 PSIRT items in the window are FortiManager, FortiAnalyzer, FortiMail, FortiAuthenticator, FortiSASE, FortiPAM, FortiRecorder/FortiVoice or third-party component bulletins (FG-IR-26-188 Linux kernel, FG-IR-26-187 Node.js, FG-IR-26-279 NGINX, FG-IR-26-278 Java4Shell, FG-IR-26-258 fastapi). Their CSAF product trees do not list FortiOS/FortiGate (FG-IR-26-188, FG-IR-26-187, FG-IR-26-279, FG-IR-26-278 publish an empty product tree; FG-IR-26-258 lists only FortiAuthenticator), so they are not applicable to this device.

## Recommended actions (read-only report — nothing was changed on the firewall)

1. **Upgrade plan:** FortiOS 7.4.11 -> 7.4.13 addresses all four affected advisories (FG-IR-26-273, FG-IR-26-268, FG-IR-26-267, FG-IR-26-269). Highest priority is FG-IR-26-273 (9.0, HA/FGSP command execution as super admin) and FG-IR-26-267/268 (9.3/9.6, exposed if OSPF/BGP peers are configured).
2. **Short-term mitigation until the upgrade:** keep HA/FGSP and config-sync peers on a dedicated, trusted link; restrict which neighbours can peer via OSPF/BGP to known routers; restrict admin SSH source addresses.
3. **Virtual patch:** FG-IR-26-273 states a FortiGuard IPS virtual patch is available — confirm the IPS signature package on fw-maniak-hq is current.
4. If this FortiGate is managed by FortiManager, patch FortiManager to >= 7.4.12 because of FG-IR-26-225 (privilege escalation onto managed FortiGates).

_Report generated read-only by fortigate_cve_watch from the PSIRT and Outbreak Alert feeds fetched 2026-11-24. Every CVE ID and version range above was taken verbatim from those fetched sources._
