# fortigate-health - 2026-11-15

**Verdict: HEALTHY** — no counter above 80%; CPU idle 100%, memory ~39%, sessions and log rates all nominal.

- **Device:** fw-maniak-hq · FortiGate 80F (FGT80F) · serial FGT80FTK22061709
- **Software:** FortiOS v7.4.11 build 2878 · VDOM `root`
- **Sample time:** 2026-11-15 09:00:08 EST (14:00:08 UTC)
- **Sources:** read-only `fg_resource_usage` + `fg_system_status` (no changes made to the firewall)

## Resource usage

| Metric | Current | 24 h min / max / avg | >80%? |
|---|---|---|---|
| CPU (8 cores, `fg_system_status`) | 0% (idle 100; user 0, system 0, nice 0, iowait 0) | — (instantaneous per-core snapshot) | No |
| CPU (monitor trend, `fg_resource_usage`) | 0% | 0 / 21 / 0 | No |
| Memory | 39% — 1.45 GiB used / 3.63 GiB total | 38 / 39 / 38 | No |
| Disk | 0% — *not reported* (`log_disk_status: not_available`) | 0 / 0 / 0 | No (N/A) |
| Sessions (IPv4) | 1 155 | 616 / 1 992 / 988 | n/a |
| Sessions (IPv6) | 11 | 3 / 22 / 10 | n/a |
| Session setup rate (IPv4) | 14 /s | 0 / 95 / 3 | n/a |
| Session setup rate (IPv6) | 0 /s | 0 / 8 / 0 | n/a |
| NPU (CP9) offloaded sessions | 432 | 251 / 902 / 374 | n/a |
| NPU IPv6 offloaded sessions | 0 | 0 / 0 / 0 | n/a |
| NTurbo sessions | 28 | 8 / 104 / 16 | n/a |
| NTurbo IPv6 sessions | 0 | 0 / 0 / 0 | n/a |
| Disk log rate | 0 /s | 0 / 0 / 0 | No |
| FortiAnalyzer log rate | 0 /s | 0 / 0 / 0 | No |
| FortiCloud log rate | 14 /s | 3 / 64 / 12 | No |
| FortiAnalyzer-via-FortiCloud log rate | 0 /s | 0 / 0 / 0 | No |

## Notes

- Memory: 1 552 556 032 B used of 3 897 774 080 B total (39.8%); 1 964 290 048 B free plus 380 928 000 B reclaimable. Excluding the freeable buffer, usage is ~44% of usable memory.
- CPU: all 8 cores report 100% idle at sample time; the 24 h monitor trend peaked at 21% (10-min window), average ~0%.
- Disk: the monitor endpoint returns 0 for disk and `fg_system_status` reports `log_disk_status: not_available`, so disk utilisation is not exposed (no local log disk — logging is offloaded to FortiCloud at ~14 msg/s).
- Sessions: ~1 155 IPv4 + 11 IPv6 live sessions, ~37% of the IPv4 table hardware-offloaded on the CP9/NPU (432) with 28 accelerated by NTurbo; setup rate 14 /s IPv4, 0 /s IPv6 (1-min trend avg 9 /s, peak 25 /s). The tool returns absolute counters only (no capacity percentage), so no session-threshold flag applies.

## Flags (>80%)

None. No metric crossed the 80% watch threshold in the current sample or in the 24 h history.
