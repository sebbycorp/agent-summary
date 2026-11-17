# fortigate-health - 2026-11-17

**Verdict: HEALTHY** — no counter above 80%; CPU 0% (all 8 cores 100% idle), memory 39%, sessions and log rates all nominal.

- **Device:** fw-maniak-hq · FortiGate 80F (FGT80F) · serial FGT80FTK22061709
- **Software:** FortiOS v7.4.11 build 2878 · VDOM `root`
- **Sample time:** 2026-11-17 09:00:07 EST (14:00:07 UTC)
- **Sources:** read-only `fg_resource_usage` + `fg_system_status` (no changes made to the firewall)

## Resource usage

| Metric | Current | 24 h min / max / avg | >80%? |
|---|---|---|---|
| CPU (8 cores, `fg_system_status`) | 0% (idle 100; user 0, system 0, nice 0, iowait 0) | — (instantaneous per-core snapshot) | No |
| CPU (monitor trend, `fg_resource_usage`) | 0% | 0 / 18 / 0 | No |
| Memory | 39% — 1.41 GiB used / 3.63 GiB total | 38 / 39 / 38 | No |
| Disk | 0% — *not reported* (`log_disk_status: not_available`) | 0 / 0 / 0 | No (N/A) |
| Sessions (IPv4) | 1 042 | 610 / 2 815 / 951 | n/a |
| Sessions (IPv6) | 16 | 3 / 21 / 8 | n/a |
| Session setup rate (IPv4) | 0 /s | 0 / 94 / 3 | n/a |
| Session setup rate (IPv6) | 0 /s | 0 / 7 / 0 | n/a |
| NPU (CP9) offloaded sessions | 500 | 275 / 1 854 / 407 | n/a |
| NPU IPv6 offloaded sessions | 0 | 0 / 0 / 0 | n/a |
| NTurbo sessions | 30 | 17 / 82 / 26 | n/a |
| NTurbo IPv6 sessions | 0 | 0 / 0 / 0 | n/a |
| Disk log rate | 0 /s | 0 / 0 / 0 | No |
| FortiAnalyzer log rate | 0 /s | 0 / 0 / 0 | No |
| FortiCloud log rate | 24 /s | 3 / 97 / 12 | No |
| FortiAnalyzer-via-FortiCloud log rate | 0 /s | 0 / 0 / 0 | No |

## Notes

- Memory: 1 519 169 536 B used of 3 897 774 080 B total (38.98%); 1 996 398 592 B free plus 382 205 952 B (364.5 MiB) reclaimable. Excluding the freeable buffer, usage is ~43% of usable memory.
- CPU: all 8 cores report 100% idle at sample time; the 24 h monitor trend peaked at 18% (10-min window), average ~0%.
- Disk: the monitor endpoint returns 0 for disk and `fg_system_status` reports `log_disk_status: not_available`, so disk utilisation is not exposed (no local log disk — logging is offloaded to FortiCloud at ~24 msg/s).
- Sessions: ~1 042 IPv4 + 16 IPv6 live sessions; ~48% of the IPv4 table is hardware-offloaded on the CP9/NPU (500) with 30 accelerated by NTurbo. Setup rate sampled 0 /s IPv4 and 0 /s IPv6 at the instant of capture (1-min trend avg 4 /s, peak 13 /s). The tool returns absolute counters only (no capacity percentage), so no session-threshold flag applies.

## Flags (>80%)

None. No metric crossed the 80% watch threshold in the current sample or in the 24 h history.
