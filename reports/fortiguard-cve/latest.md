# fortiguard-cve - 2026-11-10

**Verdict: ACTION NEEDED (low urgency)** — of the two in-window FortiOS advisories, one applies to our running build: FG-IR-26-182 / CVE-2026-106175 lists "FortiOS 7.4 all versions" as *known affected*, so FortiOS 7.4.11 build 2878 on fw-maniak-hq **is affected** — but it is CVSS 2.1 (LOW), local, high-privilege CLI only, and Fortinet publishes no 7.4 fix (migrate to a fixed release / upgrade to 7.6.9+). No unauthenticated, SSL-VPN, or FortiAP advisory applies today.

## Device facts (read via fg_system_status)

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | **v7.4.11 build 2878** |
| VDOM | root |
| CPU / Memory | 8 cores 100% idle / ~1.49 GB used of 3.90 GB |
| FortiAP firmware | **Not assessed** — no FortiAP firmware version was supplied in the user context this run, and the PSIRT feed contained **zero** FortiAP advisories in the window. |

Check window: last 30 days = **2026-10-11 → 2026-11-10** (America/Toronto). Target version under test: **7.4.11**.

## Feeds fetched in this session

| Feed | URL | Status |
|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | ✅ reachable, 50 items → 12 in window |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml (page: https://fortiguard.fortinet.com/outbreak-alert) | ✅ reachable, 20 items → 1 in window |
| CSAF per-advisory | https://filestore.fortinet.com/fortiguard/psirt/csaf_*.json | ✅ used to resolve exact affected-version ranges + CVE IDs |

Both feeds were reachable; none had to be reported as unavailable. All rows below come from the feeds pulled above.

## In-window items concerning FortiOS / FortiGate / FortiAP / FortiProxy

| Date | Advisory / Alert | CVE IDs | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-13 | **FG-IR-26-182** — Double free in signal handler (CWE-415, CVSS 2.1 LOW) — local authenticated attacker crashes `newcli` via crafted CLI commands | CVE-2026-106175 | FortiOS, FortiProxy, FortiSwitch | **yes** — CSAF `known_affected` includes "FortiOS 7.4 all versions". No 7.4 fix ("FortiOS 7.4: Migrate to a fixed release"); fixed from FortiOS 7.6.9 | https://fortiguard.fortinet.com/psirt/FG-IR-26-182 |
| 2026-10-13 | **FG-IR-26-181** — GUI Denial of Service (CWE-667, CVSS 6.7 MEDIUM) — unauthenticated network attacker DoSes the HTTPS management interface via crafted HTTP requests | CVE-2026-106174 | FortiOS | **no** — CSAF states "FortiOS 7.4: Not Applicable" (affected: 7.6.4–7.6.6, 8.0.0; fixed 7.6.7 / 8.0.1). Virtual patch FG-VD-61291.0day in FMWP db 26.073 | https://fortiguard.fortinet.com/psirt/FG-IR-26-181 |
| 2026-10-27 | **Outbreak Alert: PHP2Shell Remote Code Execution Vulnerabilities** — FortiGuard Labs monitoring PHP RCE + related threat activity | CVE-2026-109618, CVE-2026-109619, CVE-2026-115560 | PHP (third-party); paired FG-IR-26-186 covers FortiSOAR PaaS | **no** — FortiOS/FortiGate/FortiAP not listed; FG-IR-26-186 CSAF names only FortiSOAR PaaS | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-26 | **FG-IR-26-187** — Node2Shell CVEs (third-party Node.js 22.x/24.x/26.x, incl. Brotli OOB-write RCE) | CVE-2026-105481, CVE-2026-105482, CVE-2026-105483, CVE-2026-106069, CVE-2026-106070, CVE-2026-106071, CVE-2026-106072, CVE-2026-106073, CVE-2026-106074, CVE-2026-111497 | Node.js (third-party) | **no** — CSAF `product_tree.branches` is empty; no Fortinet product listed as affected | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-10-21 | **FG-IR-26-186** — PHP2Shell core RCE patches (CVSS 5.3, DoS) | CVE-2026-109618, CVE-2026-109619, CVE-2026-115560 | FortiSOAR PaaS only (feed: "No Fortinet product meets the conditions required for exploitation" for CVE-2026-115560) | **no** — FortiOS not in `known_affected` | https://fortiguard.fortinet.com/psirt/FG-IR-26-186 |
| 2026-10-11 – 2026-11-10 | **FortiAP and FortiProxy SSL-VPN coverage check** | none | FortiAP / FortiProxy SSL-VPN | **no items** — full-feed scan found no FortiAP advisory in the PSIRT feed at all; the only Agentless SSL-VPN entry (FG-IR-26-150) is dated 2026-07-14, outside the window | — |

### Detail on the one applicable advisory — FG-IR-26-182 / CVE-2026-106175

- Vector: `CVSS:3.1/AV:L/AC:L/PR:H/UI:N/S:U/C:N/I:N/A:L/E:P/RL:O/RC:C` → **local** attack surface, **high privileges required**, availability-only impact. Not reachable by an unauthenticated remote attacker.
- Impact: crash of the `newcli` daemon from crafted CLI commands (double free in signal handler).
- Vendor guidance from CSAF: FortiOS 7.6 → upgrade to 7.6.9+; FortiOS 7.4 / 7.2 / 7.0 → **"Migrate to a fixed release"** (i.e. no backported 7.4.12-style fix is published).
- Practical posture for fw-maniak-hq: risk is confined to anyone who already holds super_ADMIN-equivalent CLI access (console/SSH), so it is a **patch-planning item, not an incident**. Mitigate by restricting CLI/admin access and admin-scope profiles until the 7.6.x migration.

## Other in-window Fortinet advisories — different product, no FortiGate impact

Published in the PSIRT feed during the window but not for FortiOS/FortiGate/FortiAP; listed for completeness (managed-platform relevance only).

| Date | Advisory | Product | Affects 7.4.11? | CVSS | Link |
|---|---|---|---|---|---|
| 2026-10-13 | FG-IR-26-180 — Arbitrary file write in grpc phQueryMaster | FortiSIEM | no (not FortiOS) | 9.4 | https://fortiguard.fortinet.com/psirt/FG-IR-26-180 |
| 2026-10-13 | FG-IR-26-185 — Improper access control / session mgmt in web mgmt interface | FortiSandbox | no (not FortiOS) | 9.6 | https://fortiguard.fortinet.com/psirt/FG-IR-26-185 |
| 2026-10-13 | FG-IR-26-178 — Multiple arbitrary file read/write via SQL injection | FortiAnalyzer / FortiManager (+Cloud) | no (not FortiOS) | 7.9 | https://fortiguard.fortinet.com/psirt/FG-IR-26-178 |
| 2026-10-13 | FG-IR-26-184 — SQL command injection in webmail | FortiMail | no (not FortiOS) | 7.7 | https://fortiguard.fortinet.com/psirt/FG-IR-26-184 |
| 2026-10-13 | FG-IR-26-177 — Arbitrary command execution in fgfmsd daemon | FortiManager (fgfmd) | no (not FortiOS) | 5.3 | https://fortiguard.fortinet.com/psirt/FG-IR-26-177 |
| 2026-10-13 | FG-IR-26-179 — Fabric API authentication bypass (hard-coded key) | FortiSandbox / FortiSandbox Cloud | no (not FortiOS) | 4.9 | https://fortiguard.fortinet.com/psirt/FG-IR-26-179 |
| 2026-10-13 | FG-IR-26-176 — Persistent DoS in admin profile | FortiSandbox | no (not FortiOS) | 4.8 | https://fortiguard.fortinet.com/psirt/FG-IR-26-176 |
| 2026-10-13 | FG-IR-26-183 — Broken authorization in GUI | FortiSIEM | no (not FortiOS) | 3.4 | https://fortiguard.fortinet.com/psirt/FG-IR-26-183 |

## Recommended actions (for the human operator — this agent took no action)

1. **Track FG-IR-26-182 / CVE-2026-106175**: confirm it applies (it does, per CSAF "FortiOS 7.4 all versions") and plan the FortiOS 7.4.11 → 7.6.9+ migration path; no 7.4-train fix is offered. Interim: lock down CLI/admin access (trusted hosts, admin profiles, no admin on untrusted interfaces).
2. **FG-IR-26-181 (unauthenticated HTTPS-mgmt DoS)** does not apply to 7.4.11 — no action; if FMWP/IPS is licensed, having the FG-VD-61291.0day virtual patch (FMWP 26.073) is still useful on any 7.6.4–7.6.6 / 8.0.0 neighbours.
3. Keep management GUI/CLI off untrusted networks — that is the common mitigation for both of the two FortiOS rows above.
4. Supply the managed **FortiAP firmware version** (e.g. FP231F v7.4.x) in the run context if FortiAP coverage is wanted; none was provided today and the feed had no FortiAP entries.

---
*Generated read-only by the fortigate_cve_watch agent. Sources fetched this session: filestore.fortinet.com PSIRT `ir.xml`, filestore.fortinet.com Outbreak Alert `outbreakalert.xml`, and per-advisory CSAF JSON. No firewall configuration was read beyond `fg_system_status` and nothing was changed.*
