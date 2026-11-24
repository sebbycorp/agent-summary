# fortigate-health - 2026-11-24

**Verdict: HEALTHY** — no counter above 80%; CPU idle, memory ~39% used, sessions and setup rate well within limits.

Daily read-only health check of `fw-maniak-hq` (FortiGate 80F, FortiOS v7.4.11 build 2878, serial FGT80FTK22061709, VDOM root).
Sample time: 2026-11-24 14:00 UTC (09:00 America/Toronto, EST). Sources: `fg_resource_usage`, `fg_system_status` (plus `fg_log_state` for disk context).

## Resource usage

| Metric | Current | 1-min avg | 10-min max | 24-h max | Status |
| --- | --- | --- | --- | --- | --- |
| CPU usage (%) | 0% | 0% | 1% | 16% | OK |
| CPU cores (8x) | all 100% idle (user 0 / system 0 / iowait 0) | — | — | — | OK |
| Memory usage (%) | 38% | 38% | 38% | 39% | OK |
| Memory used / total | 1.41 GiB / 3.63 GiB (1,509,224,448 / 3,897,774,080 B) | — | — | — | OK |
| Memory free / freeable | 1.86 GiB free + 0.36 GiB freeable (2,001,985,536 / 386,564,096 B) | — | — | — | OK |
| Disk usage (%) | 0% (no local log disk — see note) | 0% | 0% | 0% | OK |
| Session count (IPv4) | 1,083 | 1,044 | 1,135 | 2,074 | OK |
| Session count (IPv6) | 17 | 17 | 19 | 22 | OK |
| Session setup rate (IPv4) | 11 sess/s | 7 sess/s | 23 sess/s | 116 sess/s | OK |
| Session setup rate (IPv6) | 0 sess/s | 0 sess/s | 0 sess/s | 8 sess/s | OK |
| NPU-offloaded sessions | 443 (v4) / 0 (v6) | 432 | 475 | 881 | OK |
| NTurbo sessions | 21 (v4) / 0 (v6) | 20 | 30 | 92 | OK |
| Disk log rate | 0 msg/s | 0 | 0 | 0 | OK |
| FortiAnalyzer log rate | 0 msg/s (FAD not enabled) | 0 | 0 | 0 | OK |
| FortiCloud log rate | 19 msg/s | 15 msg/s | 48 msg/s | 126 msg/s | OK |
| FortiAnalyzer-cloud log rate | 0 msg/s | 0 | 0 | 0 | OK |
| FortiCloud log quota | 0 B used of 500 MB quota | — | — | — | OK |

## Notes

- **Nothing above 80%.** Highest values seen today: memory 39% (24-h max), CPU 16% (24-h max), sessions 2,074 (24-h max), setup rate 116 sess/s (24-h max) — all far from the 80% watch threshold.
- **Disk**: the monitor disk counter reads 0% and the local disk log device is reported `not_available` / disabled-by-default (`log_disk_status: not_available`). Logging is going to FortiCloud (enabled, 19 msg/s), with FortiAnalyzer and FortiAnalyzer-cloud disabled. This is a configuration state, not a fault.
- ~41% of IPv4 sessions are NPU-offloaded (443 of 1,083) with 21 NTurbo-accelerated — traffic is being handled in hardware, consistent with the near-zero CPU.
- IPv6 presence is minimal (17 sessions) and IPv6 setup rate is effectively zero.
- No HA counters beyond VDOM `root` were returned by the tools.

## Actions taken

None. Read-only check — no configuration changes were made to the firewall.
