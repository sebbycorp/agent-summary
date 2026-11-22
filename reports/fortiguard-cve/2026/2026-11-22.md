# fortiguard-cve - 2026-11-22

**Verdict: ACTION NEEDED — FortiOS 7.4.11 falls inside the affected range of 4 new FortiOS PSIRT advisories (incl. 3 CRITICAL, one publicly exploited); upgrade to FortiOS 7.4.13.**

## Target

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | **v7.4.11 build 2878** |
| VDOM | root |
| Health at check time | CPU ~100% idle, mem 1.52 GB / 3.90 GB — no performance pressure |

## Feeds checked (this session)

| Feed | URL | Status | Items |
|---|---|---|---|
| PSIRT IR Advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK (50,949 B) | 50 items, all within window |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml (page: https://fortiguard.fortinet.com/outbreak-alert) | OK (18,079 B) | 20 items, 3 within window |

Window: **2026-10-23 → 2026-11-22** (last 30 days). Both feeds reachable; neither was unreachable.
Per-advisory CVE ids and affected-version ranges were taken from the matching CSAF JSON on filestore.fortinet.com (`/fortiguard/psirt/csaf_*_fg-ir-*.json`) — not inferred.

## FortiOS / FortiGate / FortiAP items in the last 30 days

All published Tue, 10 Nov 2026. "Affects 7.4.11?" is decided against the CSAF `known_affected` range vs. the running v7.4.11.

| Date | Advisory | CVE | Product | Affected FortiOS range (CSAF) | Affects 7.4.11? | CVSS v3 | Link |
|---|---|---|---|---|---|---|---|
| 2026-11-10 (rev. 11-16) | FG-IR-26-273 — Unauthenticated adjacent command execution via HA (CWE-306) | CVE-2026-126259 | FortiOS | >=7.4.0 – <=7.4.12 | **yes** | 9.0 CRITICAL | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 (rev. 11-13) | FG-IR-26-268 — Numeric truncation error in bgp daemon (CWE-197) | CVE-2026-126254 | FortiOS, FortiSASE | >=7.4.0 – <=7.4.12 | **yes** | 9.6 CRITICAL | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness in ospf daemon (CWE-305) | CVE-2026-126253 | FortiOS, FortiSASE | >=7.4.0 – <=7.4.12 | **yes** | 9.3 CRITICAL | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 (rev. 11-12) | FG-IR-26-269 — Path traversal in ssh daemon (CWE-24) | CVE-2026-126255 | FortiOS, FortiSASE, FortiPAM | >=7.4.0 – <=7.4.12 | **yes** | 7.4 HIGH | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 (rev. 11-12) | FG-IR-26-193 — SSLVPN improper certificate validation (CWE-295) | CVE-2026-121528 | FortiOS SSLVPN | >=7.4.0 – <=7.4.9 (fixed in 7.4.10) | **no** | 7.7 HIGH | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in admin RADIUS TFA (CWE-287) | CVE-2026-121530 | FortiOS | only >=7.6.0 – <=7.6.6; CSAF lists "FortiOS 7.4 all versions" as **known_not_affected** | **no** | 5.9 MEDIUM | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection → super admin on managed FortiGates (CWE-1336) | CVE-2026-126180 | FortiManager, FortiManager Cloud (**FortiOS not in `known_affected`**) | n/a for FortiOS; FortiManager >=7.4.0 – <=7.4.11 | **no** (FortiGate itself is the *target*, not the vulnerable product) | 8.4 HIGH | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |

**No FortiAP or FortiProxy SSL-VPN advisories appeared in either feed in this window** — grep for "FortiAP" and "FortiProxy" returned 0 hits across both feeds fetched today, so any managed FortiAPs (FortiAP firmware, e.g. FP231F v7.4.x) have no new PSIRT action from this check. No FortiOS version field for the APs was collected by this read-only toolset; re-check if AP firmware is in scope.

## Outbreak Alerts in the last 30 days

No Outbreak Alert in the window concerns Fortinet products. None mention FortiOS, FortiGate, FortiAP or FortiProxy.

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-19 | PHP2Shell Remote Code Execution Vulnerabilities | none stated | PHP application servers | no (not a Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-11-18 | Refrigeration Control Systems Attack | none stated | Danfoss / Copeland / Dixell refrigeration controllers | no | https://fortiguard.fortinet.com/outbreak-alert/refrigeration-control-systems-attack |
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Node.js / Google Brotli decoder | no | https://fortiguard.fortinet.com/outbreak-alert/node2shell |

## Third-party PSIRT items in the window (no FortiOS/FortiGate/FortiAP impact stated)

Listed for completeness. These four do **not** name FortiOS/FortiGate/FortiAP/FortiProxy in the feed text, and their CSAF `product_tree` lists **no Fortinet product** — so no FortiGate action is required. Kept as *check advisory* only because the feed text does not affirmatively scope them.

| Date | Advisory | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|
| 2026-11-20 | FG-IR-26-279 — NGINX vulnerabilities (10 CVEs, e.g. CVE-2026-123007, CVE-2026-113425) | NGINX OSS — feed explicitly states "None of Fortinet products use NGINX Plus" | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-279 |
| 2026-11-18 | FG-IR-26-278 — Java4Shell (CVE-2026-114938) | Oracle Java SE / GraalVM — Fortinet product impact not stated in feed | check advisory | https://fortiguard.fortinet.com/psirt/FG-IR-26-278 |
| 2026-11-02 | FG-IR-26-188 — Linux kernel RCE (CVE-2026-98432) | Linux kernel (ipv4 TCP) — Fortinet product impact not stated in feed | check advisory | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-26 | FG-IR-26-187 — Node2Shell CVEs (CVE-2026-105482/105483/106069/111497/…) | Node.js — Fortinet product impact not stated in feed | check advisory | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |

Note: FG-IR-26-275 (FGFM elevation before registration password is verified, CVSS 9.0) names **FortiManager FGFM** only — FortiOS is not in its affected list — so it is not an exposure for this FortiGate unless a FortiManager manages it: https://fortiguard.fortinet.com/psirt/FG-IR-26-275

## Recommended actions (FortiGate-side; read-only check — nothing was changed)

1. **Plan a maintenance window to upgrade fw-maniak-hq from FortiOS 7.4.11 to 7.4.13** — this is the single fixed release that clears all four affected advisories (FG-IR-26-273, FG-IR-26-268, FG-IR-26-267, FG-IR-26-269). FortiOS 7.4.13 is listed as `known_not_affected` in each CSAF.
2. **Priority justification:** FG-IR-26-268 (bgpd, CVSS 9.6) carries exploit code "F" in its vector and FG-IR-26-267 (ospfd, CVSS 9.3) carries exploit PoC "P" — both are the highest-risk items. FG-IR-26-273 (CVSS 9.0, super-admin command execution) is only HA-segment **adjacent**, so risk depends on trust of the HA/FGSP L2 segment.
3. **Interim mitigations until patched** (from the advisory text itself):
   - FG-IR-26-273: on fixed versions set `config system ha` → `set password`, `set encryption enable`, `set authentication enable`; for standalone-config-sync also set `config system standalone-cluster` `psksecret`; for autoscale set `config system autoscale` `psksecret`. **Virtual patch FG-VD-61445.0day is available in FMWP db update 26.082** — enabling IPS/inline-web-proxy virtual patching is the fastest compensating control if 80F licensing allows.
   - FG-IR-26-268 / FG-IR-26-267: restrict who can peer as a configured iBGP neighbour and restrict OSPF to trusted interfaces/areas; ensure BGP/OSPF neighbour authentication is configured.
   - FG-IR-26-269: restrict administrative SSH access (SSH listen-on ranges, admin profiles, out-of-band mgmt VDOM) since it is an unauthenticated sshd path-traversal.
4. **No action** for FG-IR-26-193 (fixed in 7.4.10, we are on 7.4.11) or FG-IR-26-195 (FortiOS 7.4 all versions explicitly not affected).
5. **If a FortiManager manages this FortiGate:** check the FortiManager version — FG-IR-26-225 affects FortiManager >=7.4.0 – <=7.4.11 and lets a low-privileged Device Manager admin gain super admin on *managed FortiGates*; upgrade FortiManager to 7.4.12+.
6. No Outbreak Alert in this window requires FortiGate-side changes.

---
*Generated 2026-11-22 (America/Toronto) by fortigate_cve_watch — read-only. Every row above comes from `ir.xml` / `outbreakalert.xml` fetched in this session plus the referenced CSAF JSON files; no advisory or CVE id was invented.*
