# fortiguard-cve - 2026-11-06

**Verdict: ACTION NEEDED** — FortiOS 7.4.11 is listed as affected by FG-IR-26-182 (CVE-2026-106175, CVSS 2.1, local-authenticated CLI denial of service, "FortiOS 7.4 all versions"); plan a 7.4.x maintenance release. No remotely exploitable, unauthenticated or in-the-wild FortiOS / FortiGate / FortiAP issue was published in the last 30 days.

## Target

| Item | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | v7.4.11 build 2878 |
| vDOM queried | root |
| Health at check time | CPU 100% idle, mem 1.48 GB / 3.90 GB used — no load symptoms |

Feeds read in this session (both reachable):

- PSIRT IR advisories — `https://filestore.fortinet.com/fortiguard/rss/ir.xml` (50 items, `lastBuildDate` Fri, 06 Nov 2026 14:15:07 +0000)
- Outbreak Alerts — `https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml` (20 items, `lastBuildDate` Fri, 06 Nov 2026 14:15:07 +0000)

Window: 2026-10-07 to 2026-11-06 (30 days, America/Toronto). Per-advisory version ranges confirmed in the CSAF JSON the feed links to (`known_affected` / `known_not_affected` / `remediations`).

## Advisories and alerts affecting this FortiGate

| Date | Advisory id / alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-13 | FG-IR-26-182 "Double free in signal handler" (CVSS 2.1 LOW) | CVE-2026-106175 | **FortiOS** (also FortiProxy, FortiSwitch) — CSAF `known_affected: FortiOS 7.4 all versions`; fix note "FortiOS 7.4: Migrate to a fixed release" | **yes** | https://fortiguard.fortinet.com/psirt/FG-IR-26-182 |
| 2026-10-13 | FG-IR-26-181 "GUI Denial of Service" (CVSS 6.7 MEDIUM) | CVE-2026-106174 | FortiOS — CSAF `known_affected: FortiOS 8.0.0, >=7.6.4 or <=7.6.6`; `known_not_affected: FortiOS 7.4 all versions`; remediation "FortiOS 7.4: Not Applicable" | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-181 |
| 2026-10-21 (revised 2026-11-05) | FG-IR-26-186 "PHP2Shell core RCE vulnerability patches" (CVSS 5.3) | CVE-2026-109618, CVE-2026-109619, CVE-2026-115560 | FortiSOAR PaaS / FortiSOAR on-premise (not FortiOS); feed states CVE-2026-115560 "does not affect any Fortinet products" | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-186 |
| 2026-10-26 (revised 2026-11-05) | FG-IR-26-187 "Node2Shell CVEs" (CVSS 9.8) | CVE-2026-105482, CVE-2026-105483, CVE-2026-106069, CVE-2026-111497, CVE-2026-105481, CVE-2026-106072, CVE-2026-106074, CVE-2026-106070, CVE-2026-106071, CVE-2026-106073 | Node.js 26.x / 24.x / 22.x — CSAF product tree lists no Fortinet product; feed text does not state FortiOS impact | check advisory | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |
| 2026-11-02 | FG-IR-26-188 "Linux kernel RCE" (CVSS 9.8) | CVE-2026-98432 | Linux kernel (TCP/IPv4 `inet_opt`/`ireq_opt` RCU use-after-free) — CSAF product tree lists no Fortinet product; feed text does not state FortiOS impact | check advisory | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-27 | Outbreak Alert "Node2Shell Remote Code Execution" | CVE-2026-111497 | Node.js / Google Brotli decoder (third-party runtime) | no | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | Outbreak Alert "PHP2Shell Remote Code Execution Vulnerabilities" | CVE-2026-109618, CVE-2026-109619 (plus unpatched PHP session upload progress CVE-2026-115560) | PHP applications (third-party); FortiGuard monitoring, no FortiOS/FortiGate component named | no | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Outbreak Alert "Atlassian Arbitrary File Access Vulnerability" | CVE-2026-21589 | Atlassian Data Center products (Bitbucket, Confluence, Jira, Bamboo, Crowd, Crucible, Fisheye) | no | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

Every row above is an `<item>` taken from the two feeds fetched in this session; no advisory or CVE id was added from memory.

## Notes

- **FG-IR-26-182 is the only confirmed hit.** It is a double-free in the `newcli` daemon reachable only from a *local, authenticated* CLI session (CVSS 3.1 `AV:L/PR:H/C:N/I:N/A:L`, exploit code "POC", reported confidence "confirmed"). Impact is a CLI daemon crash — no confidentiality/integrity loss, no network vector. FortiOS 7.4 is listed as "all versions affected" with the guidance "Migrate to a fixed release", so the fix is a 7.4.x maintenance build; there is nothing to hot-patch and no urgency for an outage-risking upgrade. Track it in the next planned 7.4 maintenance window.
- **FG-IR-26-181** looks alarming (CVSS 6.7, unauthenticated, network, HTTPS management interface DoS) but the CSAF explicitly lists FortiOS 7.4 as *not affected* and the remediation says "FortiOS 7.4: Not Applicable". It also ships a FortiGuard virtual patch (IPS `FG-VD-61291.0day`, FMWP db update 26.073) for affected 7.6/8.0 branches only.
- **No Outbreak Alert about a Fortinet product** in the 30-day window. The three newest alerts (Node2Shell, PHP2Shell, Atlassian) target third-party software; the other Fortinet items in the window are on non-FortiGate product lines (see below).
- **No FortiAP items at all.** Neither feed contains the string "FortiAP" (checked across both raw XML files), so nothing can be matched against the managed FortiAP firmware (e.g. FP231F v7.4.x) from these feeds this cycle — no FortiAP firmware version was supplied in the request context either. If AP firmware tracking matters here, the FortiAP PSIRT list needs to be checked separately.
- **Other Fortinet products in the window (not this device, informational):** FG-IR-26-175 FortiMail path traversal, CVSS 9.8, **reported exploited in the wild** (initial 2026-10-01, revised 2026-10-07; affected FortiMail >=7.2.0 to 8.0.1, fixed 7.4.9 / 7.6.7 / 8.0.2) — https://fortiguard.fortinet.com/psirt/FG-IR-26-175 . Also on 2026-10-13: FG-IR-26-180 (FortiSIEM, 9.4), FG-IR-26-185 (FortiSandbox GUI, 9.6), FG-IR-26-178 (FortiAnalyzer/FortiManager SQLi, 7.9), FG-IR-26-177 (FortiManager fgfmd, 5.3), FG-IR-26-184 (FortiMail webmail, 7.7), FG-IR-26-183 (FortiSIEM GUI, 3.4), FG-IR-26-176 (FortiSandbox, 4.8), FG-IR-26-179 (FortiSandbox / Cloud API, 4.9). None of these name FortiOS, FortiGate, FortiAP or FortiProxy SSL-VPN.
- **FortiOS items just outside the 30-day window** (for continuity, not counted above): FG-IR-26-173 and FG-IR-26-174 (2026-09-08), FG-IR-26-160 / FG-IR-26-161 / FG-IR-26-162 (2026-08-12), FG-IR-26-143 "Restricted CLI escape using Lua" (2026-06-09).

## Recommended actions (read-only check — nothing was changed on the firewall)

1. Schedule fw-maniak-hq from FortiOS 7.4.11 build 2878 to the current FortiOS 7.4 maintenance GA release in the next change window to clear CVE-2026-106175 (FG-IR-26-182). Confirm the fixed 7.4 build number in the advisory fix table before booking the change.
2. Keep CLI administrative access restricted to trusted management networks/accounts — CVE-2026-106175 requires an authenticated local CLI session, so tight admin-profile and admin-scope controls are the effective interim mitigation.
3. Verify IPS/FortiGuard is up to date and in deny mode so the FG-VD-61291.0day virtual patch coverage and Node2Shell/PHP2Shell signatures stay current for any FortiOS-fronted or offloaded HTTP inspection.
4. If FortiAPs remain in scope for this watch, request the AP model/firmware (e.g. FP231F v7.4.x) so future runs can report AP-side exposure; nothing was matchable today.

---
Generated by agent `fortigate_cve_watch` on 2026-11-06 (America/Toronto) from FortiGuard RSS fetched with BusyBox wget. Read-only: only `fg_system_status` was called against the FortiGate; no configuration was read back or modified.
