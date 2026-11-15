# fortiguard-cve - 2026-11-15

**Verdict: ACTION NEEDED — 3 of the 50 PSIRT advisories published in the last 30 days affect the running FortiOS 7.4.11 (all three are fixed in 7.4.13); plan an upgrade to FortiOS 7.4.13.**

## Target

| Item | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | **v7.4.11 build 2878** |
| vDOM checked | root |
| FortiAP firmware | not supplied in this run — `fg_system_status` reports only the FortiGate, so no FortiAP/FW firmware was compared. No FortiAP item appeared in either feed this window. |

## Feeds fetched (this session)

| Feed | URL | Status | Items | Last build |
|---|---|---|---|---|
| PSIRT IR advisories | https://filestore.fortinet.com/fortiguard/rss/ir.xml | OK (200) | 50 | Sun, 15 Nov 2026 14:15:07 +0000 |
| Outbreak Alerts | https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml | OK (200) | 20 | Sun, 15 Nov 2026 14:15:07 +0000 |

Both feeds were reachable. Supporting detail (affected version ranges + CVE ids) was taken from each advisory's CSAF JSON on `filestore.fortinet.com` and from its page on `fortiguard.fortinet.com`. No advisory or CVE id in this report was taken from any source other than these fetched feeds.

Window applied: **2026-10-16 → 2026-11-15** (last 30 days).

## Items from the last 30 days concerning FortiOS / FortiGate / FortiAP

| Date (feed) | Advisory id / alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10 | FG-IR-26-268 — Numeric truncation error (bgpd), CVSS 9.6 | CVE-2026-126254 | FortiOS, FortiSASE | **yes** (7.4.0–7.4.12 → fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness (ospfd), CVSS 9.3 | CVE-2026-126253 | FortiOS, FortiSASE | **yes** (7.4.0–7.4.12 → fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10 | FG-IR-26-269 — Path traversal in ssh daemon, unauth auth bypass, CVSS 7.4 | CVE-2026-126255 | FortiOS, FortiSASE, FortiProxy, FortiPAM | **yes** (7.4.0–7.4.12 → fix 7.4.13) | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10 | FG-IR-26-193 — SSLVPN improper certificate validation (MITM on SSL-VPN web portal), CVSS 7.7 | CVE-2026-121528 | FortiOS (SSL-VPN) | **no** (7.4.0–7.4.9 only; fixed in 7.4.10, we run 7.4.11) | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10 | FG-IR-26-195 — Broken access control in Remote RADIUS admin TFA, CVSS 5.9 | CVE-2026-121530 | FortiOS (GUI/RADIUS TFA) | **no** (advisory lists 7.6.0–7.6.6 only; no 7.4 range) | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10 | FG-IR-26-225 — Template-engine injection in FortiManager; low-priv Device Manager admin → super admin **on managed FortiGates**, CVSS 8.4 | none published in feed text | FortiManager, FortiManager Cloud (impact on managed FortiGates) | **check advisory** (FortiOS 7.4.11 not listed as an affected product in the feed text; the advisory's CSAF file returned 404, so the 7.4 range could not be confirmed — review the link; relevant if this gate is FortiManager-managed) | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |
| 2026-10-26 | FG-IR-26-187 — Node2Shell CVEs, CVSS 9.8 | CVE-2026-105481/-105482/-105483/-106069/-106070/-106071/-106072/-106073/-106074, CVE-2026-111497 | Node.js 26.x / 24.x / 22.x (third-party runtime — feed text names no FortiOS/FortiGate version) | **no** for FortiOS 7.4.11 (informational only) | https://fortiguard.fortinet.com/psirt/FG-IR-26-187 |

### Outbreak Alerts in window (none concern Fortinet products)

| Date | Alert | CVE | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-10-27 | Node2Shell Remote Code Execution | CVE-2026-111497 | Google Brotli decoder / Node.js | **no** — third-party component; no Fortinet product named | https://fortiguard.fortinet.com/outbreak-alert/node2shell |
| 2026-10-27 | PHP2Shell Remote Code Execution Vulnerabilities | none named in feed text | PHP applications | **no** — no Fortinet product named | https://fortiguard.fortinet.com/outbreak-alert/php2shell |
| 2026-10-19 | Atlassian Arbitrary File Access Vulnerability | CVE-2026-21589 | Atlassian Data Center (Bitbucket, Confluence, Jira, Bamboo, Crowd, Crucible, Fisheye) | **no** — no Fortinet product named | https://fortiguard.fortinet.com/outbreak-alert/atlassian-arbitrary-file-access |

No Outbreak Alert about a Fortinet product was published in the window.

## Assessment

- **Confirmed exposure (3 items, one fix).** FG-IR-26-268 (CVSS 9.6), FG-IR-26-267 (CVSS 9.3) and FG-IR-26-269 (CVSS 7.4) all state "FortiOS 7.4.0 through 7.4.12 → Upgrade to 7.4.13 or above". Running 7.4.11 build 2878 is inside that range, so all three apply. Two are routing-daemon issues (bgpd/ospfd) and one is an unauthenticated SSH-daemon authentication bypass — the SSH item and the high-scoring routing items are the drivers here.
- **Single remediation.** Upgrading 7.4.11 → **7.4.13** clears all three; there is no separate fix level to chase per advisory.
- **Not applicable, no action.** FG-IR-26-193 (SSL-VPN cert validation, 7.4.0–7.4.9, fixed in 7.4.10) and FG-IR-26-195 (7.6.x only) do not apply to 7.4.11.
- **Check FG-IR-26-225 if this gate is FortiManager-managed.** The vulnerability is in FortiManager, but the impact is privilege escalation to super admin on managed FortiGates. Its CSAF file 404'd, so the FortiOS range could not be confirmed from the feed alone.
- **Posture at check time:** CPU idle 100%, memory ~1.53 GB used of ~3.90 GB — no resource pressure that would block a maintenance window.

## Recommended next steps (read-only agent — nothing was changed on the firewall)

1. Schedule an upgrade to FortiOS **7.4.13** (or the latest 7.4.x patch ≥ 7.4.13) for fw-maniak-hq.
2. Until then, limit who can reach the SSH daemon and any BGP/OSPF neighbour relationships (management-plane ACLs, restrict FGFM/admin access).
3. Confirm whether this gate is managed by FortiManager; if so, review FG-IR-26-225 separately.
4. Re-run this check after the upgrade to verify the three "yes" rows clear.

---
*Generated 2026-11-15 (America/Toronto) by the read-only FortiGuard CVE watch. Source feeds: FortiGuard PSIRT IR advisories and FortiGuard Outbreak Alerts, both fetched 2026-11-15 14:30 UTC.*
