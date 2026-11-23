# fortiguard-cve - 2026-11-23

**VERDICT: ACTION NEEDED — 4 FortiOS PSIRT advisories published in the last 30 days explicitly list FortiOS 7.4.0 through 7.4.12 as affected, which includes our running v7.4.11. Three are rated CRITICAL (CVSS 9.0–9.6). All four are fixed in FortiOS 7.4.13 — plan an upgrade.**

## Asset

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| FortiOS | v7.4.11 build 2878 |
| Serial | FGT80FTK22061709 |
| VDOM | root |
| Report date | 2026-11-23 (America/Toronto) |
| Window scanned | 2026-10-24 → 2026-11-23 (last 30 days) |

Managed FortiAPs: no FortiAP firmware version was supplied in this run's context, and **neither feed contained a single FortiAP advisory** in the window (or anywhere in the fetched feed), so no FortiAP item is reported. FortiProxy SSL-VPN likewise had zero items.

## Feeds fetched this session (both reachable)

| Feed | URL | Items |
|---|---|---|
| PSIRT IR Advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | 50 |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreak-alert.xml | 20 |
| CSAF detail JSON (per advisory) | https://filestore.fortinet.com/fortiguard/psirt/csaf_*.json | 50/50 retrieved |

All 50 PSIRT items fell inside the 30-day window (feed covers 2026-10-26 → 2026-11-20). The RSS `<description>` text names the product but **does not** state affected version ranges, so each advisory's CSAF JSON was fetched and its `product_status.known_affected` / `known_not_affected` ranges were used to answer the "affects 7.4.11?" column. No row below is asserted without a fetched feed.

## FortiOS / FortiGate items in window — affects 7.4.11?

| Date | Advisory | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 — Numeric truncation error (BGP daemon), CVSS 9.6 CRITICAL | CVE-2026-126254 | FortiOS, FortiSASE | **YES** — `FortiOS >=7.4.0\|<=7.4.12`; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness (OSPF daemon), CVSS 9.3 CRITICAL | CVE-2026-126253 | FortiOS, FortiSASE | **YES** — `FortiOS >=7.4.0\|<=7.4.12`; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-273 — Unauthenticated adjacent command execution via HA, CVSS 9.0 CRITICAL | CVE-2026-126259 | FortiOS | **YES** — `FortiOS >=7.4.0\|<=7.4.12`; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 | FG-IR-26-269 — Improper neutralization in sshd (path traversal, auth bypass), CVSS 7.4 HIGH | CVE-2026-126255 | FortiOS, FortiPAM, FortiSASE | **YES** — `FortiOS >=7.4.0\|<=7.4.12`; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 — SSLVPN improper certificate validation (MiTM), CVSS 7.7 HIGH | CVE-2026-121528 | FortiOS SSL-VPN | **NO** — affected `FortiOS >=7.4.0\|<=7.4.9`, fixed in 7.4.10; our 7.4.11 is already past the ceiling | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in RADIUS admin TFA, CVSS 5.9 MEDIUM | CVE-2026-121530 | FortiOS | **NO** — affected only `FortiOS >=7.6.0\|<=7.6.6`; CSAF lists "FortiOS/ 7.4 all versions" as not affected | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection → low-privileged Device Manager admin obtains super-admin on **managed FortiGates**, CVSS 8.4 HIGH | CVE-2026-126180 | FortiManager / FortiManager Cloud | **CHECK advisory** — vulnerable product is FortiManager (`>=7.4.0\|<=7.4.11`, fixed 7.4.12), not FortiOS; fw-maniak-hq is exposed only if it is centrally managed by a FortiManager | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |

### Exploitation caveats worth reading before you panic

The four "YES" rows all name FortiOS, but the feed descriptions carry prerequisites that narrow real exposure on this box — worth checking against config rather than assuming worst case:

- **FG-IR-26-268** requires "a configured ibgp neighbor" to send malformed attributes. If fw-maniak-hq runs no BGP/ibgp peering, reachability of this flaw is low.
- **FG-IR-26-267** is in the OSPF daemon (unauthenticated attacker "may broadcast LSA"). Relevant only where OSPF is enabled.
- **FG-IR-26-273** is adjacent-network and needs "HA FGSP + standalone-config-sync, or autoscale" to execute commands as super admin via crafted HA packets. A standalone unit with no FGSP peer is largely out of scope — **verify HA/FGSP status before treating this as live**.
- **FG-IR-26-269** is in the FortiOS SSH daemon and lets an unauthenticated attacker bypass authentication with specially crafted credentials — this one has no configuration precondition, so it is the most broadly reachable of the four if administrative SSH is exposed.

Regardless of which prerequisites apply, the single remediation that clears all four is **FortiOS 7.4.13**.

## Other Fortinet products in window (not this FortiGate — informational)

45 of the 50 advisories target products other than FortiOS. They do not affect fw-maniak-hq, but they matter if these products sit anywhere in the estate:

| Product group | Count | Notes |
|---|---|---|
| FortiManager / FortiAnalyzer (+ Cloud, + BigData, + SASE-Sovereign) | 22 | Largest cluster; several auth/authz bypass and code-injection issues at CVSS 8.2–9.1 (e.g. FG-IR-26-244, FG-IR-26-235, FG-IR-26-228, FG-IR-26-232). FG-IR-26-275 (FGFM elevation before registration password verified, CVSS 9.0) is the one to note if any FortiGate is FGFM-managed. |
| FortiMail (+ FortiRecorder, FortiVoice) | 12 | FG-IR-26-199 path traversal CVSS 9.8, FG-IR-26-216 SQL injection CVSS 9.4, FG-IR-26-208 authz bypass CVSS 9.1 — all unauthenticated. |
| FortiAuthenticator | 5 | FG-IR-26-271 hard-coded password CVSS 9.0; FG-IR-26-255 LDAP injection CVSS 8.2. |
| FortiOS-adjacent (see table above) | 6 | The FortiOS/FortiGate rows. |
| Third-party component advisories (empty CSAF product tree) | 4 | See below. |

### Third-party component advisories — "check advisory"

Four advisories carry **no explicit Fortinet product list** (CSAF product tree is empty) and instead describe an upstream open-source component. Whether fw-maniak-hq is exposed depends on whether FortiOS bundles the affected component, which these feeds do not state:

| Date | Advisory | Component / CVEs | Affects 7.4.11? | Link |
|---|---|---|---|---|
| 2026-11-20 | FG-IR-26-279 — NGINX vulnerabilities, CVSS 8.1 | NGINX OSS; CVE-2026-113425, -118899, -120967, -120993, -121810, -123007, -123633, -123634, -123640, -125283 | **check advisory** | https://fortiguard.fortinet.com/psirt/FG-IR-26-279 |
| 2026-11-18 | FG-IR-26-278 — Java4Shell, CVSS 8.1 | Oracle Java SE / GraalVM; CVE-2026-114938 | **check advisory** | https://fortiguard.fortinet.com/psirt/FG-IR-26-278 |
| 2026-11-02 | FG-IR-26-188 — Linux kernel RCE, CVSS 9.8 | Linux kernel IPv4 (tcp_v4_syn_recv_sock / inet_opt RCU); CVE-2026-98432 | **check advisory** | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-26 | FG-IR-26-187 — Node2Shell CVEs, CVSS 9.8 | Node.js 22.x/24.x/26.x + Brotli decoder; CVE-2026-105481, -105482, -105483, -106069, -106070, -106071, -106072, -106073, -106074, -111497 | **check advisory** | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |

## Outbreak Alerts in window

The Outbreak Alert feed was fetched and scanned in full. **No Outbreak Alert names FortiOS, FortiGate, FortiAP or FortiProxy** — the only "Forti" string in the entire feed is "FortiGuard Labs" (12 occurrences). So there is **no Fortinet-product outbreak alert to act on**. The three alerts inside the 30-day window, listed for completeness:

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-19 | PHP2Shell Remote Code Execution Vulnerabilities | not stated in feed | PHP runtimes (FortiGuard monitoring) | **NO** (no Fortinet product implicated) | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-11-18 | Refrigeration Control Systems Attack | not stated in feed | Danfoss AK-SM/AK-SC, Copeland XWEB, Dixell XWEB | **NO** (no Fortinet product implicated) | https://fortiguard.fortinet.com/outbreak-alert/refrigeration-control-systems-attack |
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Node.js / Google Brotli decoder | **NO** (no Fortinet product implicated) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |

No Fortinet-product SSL-VPN outbreak campaign is active against this class of device in the window — the only SSL-VPN item is the PSIRT advisory FG-IR-26-193, which does not affect 7.4.11.

## Recommended actions (this box)

1. **Schedule the FortiOS 7.4.11 → 7.4.13 upgrade** — it is the single fixed release that clears FG-IR-26-267, -268, -269 and -273, including all three CRITICAL items.
2. Until patched, confirm the mitigating prerequisites actually apply here: BGP/ibgp peers (268), OSPF adjacency (267), HA FGSP / standalone-config-sync / autoscale (273). If none are configured, live exposure is materially narrower than the CVSS scores suggest.
3. Restrict administrative SSH (269) to trusted management subnets — it is the only one of the four with no configuration precondition.
4. If any FortiManager manages this FortiGate, track FG-IR-26-225 and FG-IR-26-275 on the FortiManager side (fixed 7.4.12 / 7.4.6 respectively), since both can yield super-admin on managed FortiGates.
5. Re-run this check against the FortiAP estate once a FortiAP firmware version is provided — nothing was reportable this run because the feeds contain no FortiAP items.

## Provenance

Every advisory ID, CVE ID, CVSS score, product name, version range and link above was taken from `ir.xml`, `outbreakalert.xml`, and the 50 per-advisory CSAF JSON documents fetched live in this session. Nothing was recalled from memory. Both feeds responded successfully — no feed was unreachable. This run was strictly read-only: only `fg_system_status` was called against fw-maniak-hq, and no firewall configuration was read or changed.
