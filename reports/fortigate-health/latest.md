# fortigate-health - 2026-11-08

**Verdict: HEALTHY** — no resource counter above 80%; CPU idle, memory at 38% of 3.63 GiB, sessions ~1.1k with a setup rate of 2/s. Nothing to flag.

- **Device:** fw-maniak-hq — FortiGate 80F (FGT80F), serial FGT80FTK22061709
- **FortiOS:** v7.4.11 build 2878 · VDOM `root`
- **Sample time:** 2026-11-08 09:00:07 America/Toronto (14:00:07 UTC)
- **Sources:** read-only `fg_system_status` + `fg_resource_usage` (`/api/v2/monitor/system/resource/usage`). No changes made to the firewall.

## Resource usage

| Metric | Now | 24 h min / avg / max | Status |
|---|---|---|---|
| CPU (aggregate, 8 cores) | 0 % | 0 / 0 / 18 % | OK |
| CPU 1-min / 10-min / 1-hour peak | 0 % / 1 % / 16 % | — | OK |
| Memory (used / total) | 38 % — 1.40 GiB / 3.63 GiB (1,505,714,176 / 3,897,774,080 B) | 37 / 37 / 39 % | OK |
| Memory incl. `freeable` (cache) | 48.3 % used+cache (freeable 0.35 GiB; truly free 1.88 GiB) | — | OK |
| Disk | 0 % (no value reported) | 0 / 0 / 0 % | N/A — see note |
| Sessions (IPv4) | 1,058 | 598 / 982 / 1,758 | OK |
| Sessions (IPv6) | 7 | 3 / 10 / 23 | OK |
| Session setup rate (IPv4) | 2 /s | 0 / 3 / 112 | OK |
| Session setup rate (IPv6) | 0 /s | 0 / 0 / 8 | OK |
| NPU-offloaded sessions (v4 / v6) | 353 / 0 | 19–725 / 0 | OK |
| NTurbo sessions (v4 / v6) | 35 / 0 | 0–180 / 0 | OK |
| Log rate — disk | 0 /s | 0 / 0 / 0 | N/A |
| Log rate — FortiAnalyzer | 0 /s | 0 / 0 / 0 | N/A |
| Log rate — FortiCloud | 50 /s | 4 / 13 / 68 | OK (burst) |
| Log rate — FAZ-to-cloud | 0 /s | 0 / 0 / 0 | N/A |

## Flags (threshold > 80 %)

- **None.** Highest reading in any window: memory 39 % (peak) and CPU 18 % (24 h spike). Both well clear of the 80 % flag line.

## Notes / interpretation

- **Disk 0 % is not a green light** — `fg_system_status` reports `log_disk_status: not_available`, i.e. this unit is not logging to disk (logs are going to FortiCloud, which shows a live `forticloud_lograte` of 13–50 events/s). Disk usage is therefore *not measurable*, not genuinely zero.
- Recent session profile is quiet and stable: 1-min window averaged 1,201 sessions (1,227 peak) vs. the 24 h peak of 1,758; the IPv4 peak is only ~1.8× the current value, so no session-pressure trend.
- Setup rate is negligible (2/s now, 112/s 24 h peak) — no flood/scan-like behaviour in these counters.
- NPU holds ~33 % of sessions offloaded (353 of 1,058); CPU near-idle is consistent with healthy hardware offload.
- 10-min history shows one transient setup-rate spike to 77/s and a session dip to 846 — normal churn for this device, not sustained.
- Read-only check: no policy, interface, route, VPN, or DHCP state was read into this report as an assumption, and nothing was written to the FortiGate.
