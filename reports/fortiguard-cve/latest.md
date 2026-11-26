# fortiguard-cve - 2026-11-26

**VERDICT: ACTION NEEDED — of the 7 FortiOS/FortiGate-related advisories published in the last 30 days, 4 affect the running FortiOS v7.4.11 build 2878 (three of them CVSS 9.0+); the single fix is FortiOS 7.4.13.**

## Asset under review

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | **v7.4.11, build 2878** |
| vDOM | root |
| CPU / memory | ~100% idle / 1.51 GB used of 3.90 GB |
| FortiAP firmware | Not reported — the user context for this check did not include managed FortiAP firmware (e.g. FP231F v7.4.x), so no FortiAP assessment is possible from this run |

**Feeds fetched this session (both reachable):**
- PSIRT advisories: `https://filestore.fortinet.com/fortiguard/rss/ir.xml` — 50 items
- Outbreak Alerts: `https://filestore.fortinet.com/fortiguard/rss/outbreakalert.xml` — 20 items
- Affected-version ranges confirmed from the matching Fortinet CSAF JSON on `https://filestore.fortinet.com/fortiguard/psirt/` for all 50 advisories (the RSS text alone does not state version ranges).

## FortiOS / FortiGate / FortiAP items in the last 30 days

| Date (pub, rev) | Advisory / alert | CVE id(s) | Product | Affects 7.4.11? | Link |
|---|---|---|---|---|---|
| 2026-11-10, rev 2026-11-13 | FG-IR-26-268 — Numeric truncation error in the BGP daemon (CVSS 9.6 CRITICAL) | CVE-2026-126254 | FortiOS, FortiSASE | **yes** — affected 7.4.0–7.4.12; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-268 |
| 2026-11-10, rev 2026-11-10 | FG-IR-26-267 — Authentication bypass by primary weakness in the OSPF daemon (CVSS 9.3 CRITICAL) | CVE-2026-126253 | FortiOS, FortiSASE | **yes** — affected 7.4.0–7.4.12; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-267 |
| 2026-11-10, rev 2026-11-16 | FG-IR-26-273 — Missing authentication for a critical function via HA / FGSP + standalone-config-sync / autoscale (CVSS 9.0 CRITICAL) | CVE-2026-126259 | FortiOS | **yes** — affected 7.4.0–7.4.12; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-273 |
| 2026-11-10, rev 2026-11-10 | FG-IR-26-269 — Path traversal in the SSH daemon, authentication bypass via crafted credentials (CVSS 7.4 HIGH) | CVE-2026-126255 | FortiOS, FortiSASE, FortiPAM | **yes** — affected 7.4.0–7.4.12; fixed 7.4.13 | https://fortiguard.fortinet.com/psirt/FG-IR-26-269 |
| 2026-11-10, rev 2026-11-12 | FG-IR-26-193 — SSL-VPN improper certificate validation, MITM on the SSL-VPN web portal → backend site (CVSS 7.7 HIGH) | CVE-2026-121528 | FortiOS SSL-VPN | **no** — affected 7.4.0–7.4.9, fixed in 7.4.10; 7.4.11 is already past the fix | https://fortiguard.fortinet.com/psirt/FG-IR-26-193 |
| 2026-11-10, rev 2026-11-10 | FG-IR-26-195 — Broken access control in FortiOS Remote RADIUS admin two-factor (TFA) authentication (CVSS 5.9 MEDIUM) | CVE-2026-121530 | FortiOS admin GUI / RADIUS TFA | **no** — CSAF lists only 7.6.0–7.6.6 as affected; "FortiOS 7.4 all versions" is listed as not affected | https://fortiguard.fortinet.com/psirt/FG-IR-26-195 |
| 2026-11-10, rev 2026-11-24 | FG-IR-26-225 — Template injection in FortiManager / FortiManager Cloud letting a low-privileged Device Manager admin gain super admin on **managed FortiGates** (CVSS 8.4 HIGH) | CVE-2026-126180 | FortiManager 7.4.0–7.4.11, FortiManager Cloud 7.4.1–7.4.11 (FortiOS itself is not in the CSAF affected list) | **check advisory** — FortiOS 7.4.11 is not listed as an affected product; exposure exists only if fw-maniak-hq is centrally managed by a FortiManager in the affected range (fixed in 7.4.12) | https://fortiguard.fortinet.com/psirt/FG-IR-26-225 |

### FortiAP / FortiProxy
**No** FortiAP or FortiProxy advisory appeared in either feed during the last 30 days — neither string occurs in the 50 PSIRT items, the 20 Outbreak Alerts, or any of the 50 CSAF documents. FortiAP firmware was not supplied in the user context, so no FortiAP comparison was possible.

### Outbreak Alerts (Fortinet products)
**None.** The three alerts inside the 30-day window — PHP2Shell RCE (2026-11-19), Refrigeration Control Systems Attack (2026-11-18) and Node2Shell RCE (2026-10-27) — cover PHP, Danfoss/Copeland/Dixell refrigeration controllers and Node.js/Brotli respectively. None names a Fortinet product, so none is flagged as a Fortinet-product exposure.

### Not relevant to this FortiGate (in-window, Fortinet product but not FortiOS/FortiGate/FortiAP/FortiProxy)
Confirmed via CSAF that FortiOS is **not** in the affected list for: FG-IR-26-188 Linux kernel RCE (CVE-2026-98432), FG-IR-26-278 Java4Shell (CVE-2026-114938), FG-IR-26-187 Node2Shell (CVE-2026-105482 et al.), FG-IR-26-279 NGINX (CVE-2026-123007 et al.), FG-IR-26-247 RADIUS certificate validation (FortiManager/FortiAnalyzer only), FG-IR-26-258 fastapi resource exhaustion (FortiAuthenticator).

## Recommended action (for the change window — this agent is read-only and changed nothing)
1. **Upgrade fw-maniak-hq to FortiOS 7.4.13** — this is the single fix that closes all four affected advisories (FG-IR-26-267, -268, -269, -273), three of which are CVSS 9.0+ and reachable by an unauthenticated attacker.
2. Interim risk reduction until the upgrade: restrict BGP/OSPF peerings to trusted neighbours (FG-IR-26-268, FG-IR-26-267), keep HA/FGSP management traffic on an isolated trusted link and restrict admin SSH access (FG-IR-26-273, FG-IR-26-269).
3. Confirm whether fw-maniak-hq is a FortiManager-managed device; if so, verify the FortiManager / FortiManager Cloud version against FG-IR-26-225 (fixed in 7.4.12).

*Sources: FortiGuard PSIRT RSS (ir.xml) and Outbreak Alert RSS fetched 2026-11-26 14:30 UTC; affected-version ranges taken from the vendor CSAF JSON per advisory. No advisory or CVE id in this report is paraphrased from memory — every row traces to a feed fetched in this session.*
