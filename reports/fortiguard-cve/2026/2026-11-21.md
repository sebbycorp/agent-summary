# fortiguard-cve - 2026-11-21

**VERDICT: ACTION NEEDED** — FortiOS 7.4.11 (build 2878) on fw-maniak-hq is inside the affected range (`>=7.4.0|<=7.4.12`) of four FortiGuard PSIRT advisories published 2026-11-10, three rated CRITICAL (CVSS 9.0 / 9.3 / 9.6). Vendor fix is **FortiOS 7.4.13**; a virtual patch exists for the HA flaw.

## Target

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| Running version | **v7.4.11 build 2878** |
| VDOM | root |
| Load (read-only check) | CPU 0% util / 100% idle, mem ~1.51 GB used of 3.90 GB |

Check window: **2026-10-22 → 2026-11-21** (last 30 days, America/Toronto).
Feeds fetched this session (both reachable, HTTP 200):
`https://filestore.fortinet.com/fortiguard/rss/ir.xml` (50 items) and
`https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml` (12 items).
Affected-version ranges and CVE ids were taken from each advisory's CSAF JSON on filestore.fortinet.com, because the RSS descriptions carry only the prose summary and CVSS score.

## In-scope items (FortiOS / FortiGate / FortiAP / FortiProxy SSL-VPN)

| Date | Advisory / Alert | CVE | CVSS | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 – Numeric truncation error (bgpd) | CVE-2026-126254 | 9.6 CRITICAL | FortiOS, FortiSASE | **YES** (affected `>=7.4.0<=7.4.12`, fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 – Authentication bypass by primary weakness (ospfd) | CVE-2026-126253 | 9.3 CRITICAL | FortiOS, FortiSASE | **YES** (affected `>=7.4.0<=7.4.12`, fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-273 – Unauthenticated adjacent command execution via HA | CVE-2026-126259 | 9.0 CRITICAL | FortiOS | **YES** (affected `>=7.4.0<=7.4.12`, fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 | FG-IR-26-269 – Path traversal in sshd → authentication bypass | CVE-2026-126255 | 7.4 HIGH | FortiOS, FortiSASE, FortiPAM | **YES** (affected `>=7.4.0<=7.4.12`, fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 – SSLVPN improper certificate validation | CVE-2026-121528 | 7.7 HIGH | FortiOS SSL-VPN | **NO** (affected `>=7.4.0<=7.4.9`, fixed in 7.4.10) | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 – Broken access control in admin RADIUS TFA | CVE-2026-121530 | 5.9 MEDIUM | FortiOS | **NO** (affected `>=7.6.0<=7.6.6`; CSAF lists "FortiOS 7.4 all versions" as not affected) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 – Template-engine injection, Device Manager admin → super admin on managed FortiGates | CVE-2026-126180 | 8.4 HIGH | FortiManager, FortiManager Cloud | **NO** (FortiOS not in CSAF product tree — risk is via a managing FortiManager, not the gate's own OS; FortiManager fix 7.4.12) | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-11-10 | FG-IR-26-275 – FGFM elevation before registration password verified | CVE-2026-126261 | 9.0 CRITICAL | FortiManager | **check advisory** (FortiOS not listed; relevant only if this gate is centrally managed / FGFM port reachable — FortiManager fix 7.4.6) | https://fortiguard.fortinet.com/psirt/FG-IR-26-275 |
| 2026-11-18 | FG-IR-26-278 – Java4Shell (Oracle Java SE / GraalVM compiler) | CVE-2026-114938 | 8.1 HIGH | Third-party Java; CSAF product tree empty | **check advisory** (advisory text does not name FortiOS/FortiGate) | https://fortiguard.fortinet.com/psirt/FG-IR-26-278 |
| 2026-11-02 | FG-IR-26-188 – Linux kernel RCE (ipv4 inet_opt/ireq_opt RCU use-after-free) | CVE-2026-98432 | 9.8 CRITICAL | Third-party Linux kernel; CSAF product tree empty | **check advisory** (FortiOS/FortiGate not enumerated in the feed text — confirm on the advisory page whether FortiOS-based products incl. FortiGate 80F are listed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-26 | FG-IR-26-187 – Node2Shell CVEs (Brotli / Node.js) | not stated in feed | n/a | Third-party Node.js; CSAF product tree empty | **check advisory** (no FortiOS/FortiGate statement in feed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-10-27 | **Outbreak Alert** – Node2Shell Remote Code Execution | CVE-2026-111497 | n/a | Google Brotli decoder / Node.js | **check advisory** (alert text names no Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-11-18 | **Outbreak Alert** – Refrigeration Control Systems Attack | none listed | n/a | Danfoss / Copeland / Dixell ICS equipment | **NO** (no Fortinet product) | https://fortiguard.fortinet.com/outbreak-alert/refrigeration-control-systems-attack |

**No FortiAP advisory appeared in either feed inside the window** (FortiAP is mentioned 0 times in the fetched IR feed), and **no Outbreak Alert about Fortinet products** was published. The managed FortiAP firmware was not checked: the read-only status tool reports the FortiGate only, and no AP firmware version was supplied in the user context — so no FortiAP/FP231F row can be asserted.

## Detail on the four "YES" rows

1. **FG-IR-26-268 / CVE-2026-126254** (CVSS 9.6, `AV:N/AC:L/PR:L/UI:N/S:C/C:N/I:H/A:H`) — numeric truncation in the FortiOS BGP daemon; a configured iBGP neighbour can send malicious/malformed attributes to all connected routers. Impact: denial of service. CSAF `known_affected`: `FortiOS >=7.4.0|<=7.4.12`; fixed **7.4.13**. Exposure depends on BGP being configured with neighbour peers.
2. **FG-IR-26-267 / CVE-2026-126253** (CVSS 9.3) — authentication bypass by primary weakness [CWE-305] in the FortiOS OSPF daemon lets an unauthenticated attacker broadcast LSAs via a specially crafted request. `known_affected`: `FortiOS >=7.4.0|<=7.4.12`; fixed **7.4.13**. Exposure depends on OSPF being enabled.
3. **FG-IR-26-273 / CVE-2026-126259** (CVSS 9.0, `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`) — missing authentication for a critical function in FortiOS: an unauthenticated attacker with HA FGSP + standalone-config-sync, or autoscale, can execute commands as super admin via crafted HA packets. FortiGate HA clusters on FGCP (active-passive and active-active) are also affected on vulnerable pre-fix versions, with an HA-segment-adjacent attack vector. Mitigation from the advisory: on fixed versions set `config system ha` → `set password <secret>`, `set encryption enable`, `set authentication enable`, plus `psksecret` for standalone-cluster/autoscale. Virtual patch **"FG-VD-61445.0day."** available in FMWP db update **26.082**. Fixed **7.4.13**.
4. **FG-IR-26-269 / CVE-2026-126255** (CVSS 7.4) — path traversal [CWE-24] in the FortiOS / FortiSASE / FortiPAM SSH daemon allowing an unauthenticated attacker to bypass authentication via specially crafted credentials. `known_affected`: `FortiOS >=7.4.0|<=7.4.12`; fixed **7.4.13**.

## Recommended actions (not applied — this agent is read-only)

1. Plan an upgrade of fw-maniak-hq from **v7.4.11 build 2878 → FortiOS 7.4.13** (a single upgrade closes all four YES rows).
2. Until upgraded, confirm FortiGuard **IPS / FMWP is at least db update 26.082** so virtual patch "FG-VD-61445.0day." is active against FG-IR-26-273.
3. Verify the HA / FGSP / autoscale segment is trusted, and that the **FGFM port (541) and SSH (22) are not exposed to untrusted networks**; restrict OSPF/BGP peering to trusted neighbours.
4. If this gate is centrally managed, confirm the FortiManager instance is on **7.4.12+** (FG-IR-26-225) and **7.4.6+** (FG-IR-26-275).
5. No action needed on FG-IR-26-193 (already fixed in 7.4.10) or FG-IR-26-195 (FortiOS 7.4 explicitly not affected).

## Caveats

- The `ir.xml` feed is capped at **50 items** and its newest page is ordered partly by title, so it ends mid-way through the 2026-11-10 batch (last item FG-IR-26-210). FortiOS/FortiAP advisories from that batch that sort after the cut-off would not be visible in this pull; a full sweep of https://fortiguard.fortinet.com/psirt is advised before closing the upgrade ticket.
- Rows marked **check advisory** are third-party-component advisories whose CSAF product tree was empty (`"branches": []`), so no FortiOS version range is published in the feed and none was inferred here.
- No advisory or CVE id in this report was invented; every row traces to `ir.xml` / `outbreakalert.xml` fetched in this session plus the linked CSAF JSON on filestore.fortinet.com.

*Generated 2026-11-21T14:36Z (America/Toronto: 2026-11-21) by fortigate_cve_watch — read-only; no configuration change was made to the firewall.*
