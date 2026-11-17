# fortiguard-cve - 2026-11-17

**VERDICT: ACTION NEEDED — 4 PSIRT advisories in the last 30 days list FortiOS 7.4.11 as Known Affected (3 rated critical, CVSS 9.0–9.6); all four are fixed in FortiOS 7.4.13. Plan an upgrade of fw-maniak-hq (80F) from v7.4.11.**

## Target

| Item | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | **v7.4.11 build 2878** (VDOM root) |
| Load at check | CPU 100% idle, mem 1.52 GB / 3.90 GB used |
| FortiAP firmware | not reported — no FortiAP context supplied, and **no FortiAP advisory appeared in either feed** this period |

## Feeds checked

| Feed | URL | Result | Items | lastBuildDate |
|---|---|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | ✅ reachable | 50 | Tue, 17 Nov 2026 14:15:07 +0000 |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | ✅ reachable | 20 | Tue, 17 Nov 2026 14:15:07 +0000 |

Both feeds were fetched successfully this session; neither was unreachable.
Window applied: **2026-10-18 → 2026-11-17** (America/Toronto), per the feeds' `pubDate`.
Affected/not-affected verdicts were not guessed from RSS prose: the RSS descriptions carry no version ranges, so each of the 50 advisories was resolved through its own `cvrf:link` CVRF document (`https://fortiguard.fortinet.com/psirt/cvrf/FG-IR-…`) and the `Known Affected` product list was matched literally against `FortiOS-7.4.11` / `FortiAP-*` / `FortiProxy-*`. The affected-range text and fixes below are quoted from the linked advisory pages.

## FortiOS / FortiGate / FortiAP items in the last 30 days

| Date | Advisory / Alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 — Numeric truncation error (bgp daemon) — CVSS 9.6 | CVE-2026-126254 | FortiOS, FortiSASE | **yes** (affected 7.4.0–7.4.12 → upgrade to 7.4.13+) | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness (ospf daemon) — CVSS 9.3 | CVE-2026-126253 | FortiOS, FortiSASE | **yes** (affected 7.4.0–7.4.12 → upgrade to 7.4.13+) | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-273 — Unauthenticated adjacent command execution via HA — CVSS 9.0 | CVE-2026-126259 | FortiOS | **yes** (affected 7.4.0–7.4.12 → upgrade to 7.4.13+) | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in ssh daemon, auth bypass via crafted credentials — CVSS 7.4 | CVE-2026-126255 | FortiOS, FortiSASE, FortiPAM | **yes** (affected 7.4.0–7.4.12 → upgrade to 7.4.13+) | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 — SSLVPN improper certificate validation (MITM on SSL-VPN portal↔backend) — CVSS 7.7 | CVE-2026-121528 | FortiOS SSL-VPN | **no** (affected 7.4.0–7.4.9, fixed in 7.4.10 — already covered by 7.4.11) | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in Remote-RADIUS admin 2FA — CVSS 5.9 | CVE-2026-121530 | FortiOS GUI / RADIUS TFA | **no** (advisory states FortiOS 7.4 "Not affected"; affected range is 7.6.0–7.6.6) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection in FortiManager lets a low-privileged Device Manager admin gain **super admin on managed FortiGates** — CVSS 8.4 | not stated in feed/CVRF | FortiManager, FortiManager Cloud | **check advisory** — FortiOS is not in the CVRF product list, so 7.4.11 is not itself affected; exposure only if this unit is managed by FortiManager 7.4.0–7.4.11 / 7.6.x | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-10-26 (rev. 2026-11-16) | FG-IR-26-187 — "Node2Shell CVEs" — CVSS 9.8 | CVE-2026-105482, -105483, -105481, -106069, -106070, -106071, -106072, -106073, -106074, CVE-2026-111497 | Node.js 22.x / 24.x / 26.x (third-party runtime) | **check advisory** — the CVRF contains no FortiOS/FortiGate/FortiAP/FortiProxy product entry and no version table, so FortiOS exposure is not stated; verify against the advisory page | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |

## Outbreak Alerts

No Outbreak Alert in the feed concerns Fortinet products. Alerts that fall inside the 30-day window all describe third-party runtimes/applications, not FortiOS/FortiGate/FortiAP:

| Date | Alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Google Brotli decoder / Node.js | **check advisory** (via FG-IR-26-187 above — FortiOS not stated as affected) | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | not stated in feed | PHP applications | **no** — no Fortinet product named in the alert text | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Bitbucket/Confluence/Jira/Bamboo/Crowd/Crucible/Fisheye | **no** — no Fortinet product named in the alert text | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

## Exposure summary

- **Exposed now (4):** FG-IR-26-268, FG-IR-26-267, FG-IR-26-273, FG-IR-26-269 — all list `FortiOS-7.4.11` in CVRF `Known Affected`, all give the same remedy: **7.4.0 through 7.4.12 affected, upgrade to 7.4.13 or above**.
- **Already remediated (2):** FG-IR-26-193 (fixed at 7.4.10) and FG-IR-26-195 (FortiOS 7.4 explicitly "Not affected").
- **Needs a manual look (2):** FG-IR-26-187 / Node2Shell (no Fortinet product list published in the CVRF) and FG-IR-26-225 (relevant only if FortiManager manages this gate).

## Recommended actions (for the change-owning team — this check is read-only, nothing was modified)

1. **Priority: schedule FortiOS 7.4.13** for fw-maniak-hq. One upgrade closes all four affected advisories, including the three ≥9.0 CVSS ones. Check the 80F upgrade path and known issues for 7.4.13 at https://docs.fortinet.com/upgrade-tool before the window.
2. **Interim mitigations until 7.4.13**, matched to the realistic attack surface on an 80F:
   - FG-IR-26-273 (9.0, HA): if the unit is **not** in an HA/FGSP pair with `standalone-config-sync` or autoscale, this is not reachable — confirm `get system ha` status. Restrict the HA/FGSP network to trusted peers only.
   - FG-IR-26-268 / FG-IR-26-267 (9.6 / 9.3, BGP/OSPF): if no dynamic routing is configured, exposure is nil — confirm BGP/OSPF configuration; otherwise restrict BGP/OSPF neighbour adjacency to authenticated, trusted peers (TCP MD5 password, prefix-listed neighbours, routing control on the WAN-facing side).
   - FG-IR-26-269 (7.4, ssh daemon): restrict `admin-ssh` access to management source IPs / out-of-band VRF, disable SSH admin if unused, and enable login rate-limiting plus MFA (TFA) on SSH.
3. Confirm whether FortiManager manages this FortiGate; if yes, check FG-IR-26-225 and patch FortiManager, since a compromised FortiManager gains super admin on the gate.
4. Track FG-IR-26-187 — it is still being revised (revision 10 dated 2026-11-16, "Products not impacted list updated"), so re-check the advisory page next cycle for a FortiOS statement.

---
*Generated read-only by fortigate-cve_watch on 2026-11-17 (America/Toronto). Sources fetched this session: `https://filestore.fortinet.com/fortiguard/rss/ir.xml`, `https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml`, the per-advisory CVRF documents at `https://fortiguard.fortinet.com/psirt/cvrf/FG-IR-…`, and the advisory pages at `https://fortiguard.fortinet.com/psirt/FG-IR-…`. No advisory, CVE id, version range, or link was invented; every row traces to a fetched feed or a document linked directly from it.*
