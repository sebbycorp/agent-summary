# fortiguard-cve - 2026-11-07

**Verdict: REVIEW** — one low-severity advisory (FG-IR-26-182 / CVE-2026-106175) does affect FortiOS 7.4.x including our running 7.4.11 build 2878, but it is a local-authenticated CLI DoS with no published FortiOS 7.4 fix branch and no known exploitation; no critical/high FortiOS, FortiGate or FortiAP exposure in the last 30 days.

## Asset under review

| Field | Value | Source |
|---|---|---|
| Hostname | fw-maniak-hq | `fg_system_status` |
| Model | FortiGate 80F (FGT80F) | `fg_system_status` |
| Serial | FGT80FTK22061709 | `fg_system_status` |
| FortiOS | v7.4.11 build 2878 | `fg_system_status` |
| vDOM | root | `fg_system_status` |
| Load at check | CPU 0% idle 100%, mem 38%, sessions ~765 | `fg_resource_usage` |
| FortiAP firmware | Not returned by the read-only tools (e.g. FP231F v7.4.x not exposed) — see note below | — |

Report generated 2026-11-07 (America/Toronto — 09:30 EST; DST ended Sun 2026-11-01 so the zone is UTC-5. The sandbox clock is UTC and has no tzdata, so `TZ=America/Toronto date` echoes UTC 14:30, which is still 07 Nov locally). Check window: **2026-10-08 → 2026-11-07** (last 30 days). Read-only check — no configuration was changed on the firewall.

## Feeds fetched this session

| Feed | URL | Status |
|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK (49,323 bytes, 50 items, lastBuildDate Sat 07 Nov 2026 14:16:09 +0000) |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | **503 Service Unavailable on first attempt, succeeded on retry** (17,411 bytes) |
| CSAF JSON (affected-version detail) | https://filestore.fortinet.com/fortiguard/psirt/csaf_*.json | OK |

## Advisories in the last 30 days concerning our platform

Every row below was taken verbatim from the feeds fetched above; affected-version ranges were confirmed from each advisory's CSAF JSON.

| Date | Advisory / Alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-13 | FG-IR-26-182 — Double free in signal handler (CVSS 2.1 LOW) | CVE-2026-106175 | FortiOS, FortiProxy, FortiSwitch — CSAF `known_affected` lists **"FortiOS 7.4 all versions"** | **yes** | https://fortiguard.fortinet.com/psirt/FG-IR-26-182 |
| 2026-10-13 | FG-IR-26-181 — GUI Denial of Service (CVSS 6.7) | CVE-2026-106174 | FortiOS — CSAF `known_affected` = 8.0.0, >=7.6.0..<=7.6.6; **"FortiOS/ 7.4 all versions" is in `known_not_affected`**, remediation states "FortiOS 7.4: Not Applicable" | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-181 |
| 2026-11-02 | FG-IR-26-188 — Linux kernel RCE (CVSS 9.8) | CVE-2026-98432 | CSAF product tree is **empty** — no Fortinet product enumerated, so the feed text does not say whether FortiOS is affected | **check advisory** | https://fortiguard.fortinet.com/psirt/FG-IR-26-188 |
| 2026-10-21 | FG-IR-26-186 — PHP2Shell core RCE patches (CVSS 5.3) | CVE-2026-109618, CVE-2026-109619, CVE-2026-115560 | FortiOS not listed — CSAF enumerates FortiSOAR PaaS / FortiSOAR on-premise only; feed states "No Fortinet product meets the conditions required for exploitation" for CVE-2026-115560 | no | https://fortiguard.fortinet.com/psirt/FG-IR-26-186 |

### FG-IR-26-182 detail (the only "yes")

- **Vector:** `CVSS:3.1/AV:L/AC:L/PR:H/UI:N/S:U/C:N/I:N/A:L` — base score **2.1 (LOW)**.
- **Impact:** local authenticated attacker crashes the `newcli` daemon via crafted CLI commands → denial of service on the CLI only. No confidentiality or integrity impact, network-accessible attack not applicable (AV:L, PR:H).
- **Fix guidance from CSAF:** `FortiOS 7.4: Migrate to a fixed release` (7.6 branch fixed in upcoming 7.6.9). Fortinet has not published a patched 7.4.x build for this issue, so there is no in-branch upgrade for an 7.4.11 device.
- **Recommendation:** track only. Mitigation is operational, not a firmware change — restrict CLI/administrative access to trusted management networks and admin profiles, which is already the standard control for a local-authenticated, high-privilege bug. Re-check if a 7.4.12+ build is released.

## Outbreak Alerts in the window

No Outbreak Alert published in the last 30 days concerns a Fortinet product.

| Date | Alert | CVE id(s) | Subject | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Out-of-bounds write in Google's Brotli decoder / Node.js — not a Fortinet product | no | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | none listed in the feed text | PHP application RCE; FortiGuard Labs monitoring only | no | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Data Center products (Bitbucket, Confluence, Jira, Bamboo, Crowd, Crucible, Fisheye) — not a Fortinet product | no | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

## In-window items reviewed and excluded (not our platform)

Other PSIRT advisories published 2026-10-08 → 2026-10-26 name products this FortiGate does not run: FG-IR-26-187 (Node.js CVEs), FG-IR-26-180, FG-IR-26-183, FG-IR-26-179, FG-IR-26-185, FG-IR-26-176, FG-IR-26-166 (FortiSIEM / FortiSandbox), FG-IR-26-178, FG-IR-26-171, FG-IR-26-177 (FortiAnalyzer / FortiManager), FG-IR-26-184 (FortiMail webmail), FG-IR-26-164 (FortiSOAR), FG-IR-26-165 (FortiClient Windows), FG-IR-26-168, FG-IR-26-170, FG-IR-26-173, FG-IR-26-169, FG-IR-26-172, FG-IR-26-174, FG-IR-26-151.

Worth flagging even though it is outside the 30-day window and outside this product: **FG-IR-26-175** (2026-10-01, CVSS 9.8, path traversal, *"reported to be exploited in the wild"*) affects **FortiMail only** (CSAF: `FortiMail >=7.4.0|<=7.4.8`) — **no FortiOS exposure** for fw-maniak-hq. https://fortiguard.fortinet.com/psirt/FG-IR-26-175

## FortiAP note

The `ir.xml` feed fetched in this session contains **zero** occurrences of "FortiAP", so no FortiAP-specific PSIRT advisory was published in the window. The FortiAP firmware level of the managed APs (e.g. FP231F v7.4.x) is **not exposed** by the read-only FortiGate status tools available to this agent, so it could not be confirmed against an advisory baseline — please supply the FortiAP model/firmware (or grant the read-only FortiManager/FortiLAN read APIs) if AP-side CVE matching is required.

## Actions

1. **No emergency change window needed.** Nothing in the last 30 days is remotely exploitable against FortiOS 7.4.11 on this device.
2. **Track FG-IR-26-182 / CVE-2026-106175** (affects FortiOS 7.4 all versions, no 7.4 fix published yet). Confirm the admin plane stays restricted: HTTPS/CLI administration limited to trusted management interfaces/subnets and to non-trivial admin profiles — this is the practical control for a local-authenticated, high-privilege CLI bug.
3. **Verify FG-IR-26-188 / CVE-2026-98432** (CVSS 9.8, empty CSAF product tree) by opening the advisory — the feed does not state whether FortiOS is affected.
4. Keep FortiGuard IPS current — a vendor virtual patch is referenced for FG-IR-26-181, and virtual patching is the practical compensating control while the 7.4 branch is end-of-fix for several of these issues.

---
*Generated read-only by agent `fortigate_cve_watch`. No firewall configuration was read beyond system/resource status and no changes were made. All advisory rows originate from feeds fetched in this session; no advisory or CVE id was invented.*
