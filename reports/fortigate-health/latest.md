# fortigate-health - 2026-11-22

**Verdict: HEALTHY** — no resource counter above 80%; CPU idle, memory at 38%, sessions and setup rate well within FortiGate 80F limits.

- Device: fw-maniak-hq (FortiGate 80F, FGT80F)
- Firmware: FortiOS v7.4.11 build 2878
- Serial: FGT80FTK22061709
- VDOM: root
- Sample time: 2026-11-22 14:00:08 UTC (09:00:08 EST, America/Toronto)
- Sources: read-only `fg_system_status` + `fg_resource_usage` (no changes made)

## Resource usage

| Metric | Now | 1-min avg/max | 30-min max | 24-hour max | Status |
|---|---|---|---|---|---|
| CPU (8 cores, aggregate) | 0% | 0 / 0 | 8 | 18 | OK |
| Memory used % | 38% | 38 / 38 | 38 | 39 | OK |
| Memory used (bytes) | 1,506,086,912 (1.40 GiB) | — | — | — | OK |
| Memory total (bytes) | 3,897,774,080 (3.63 GiB) | — | — | — | OK |
| Memory free / freeable | 2,006,073,344 / 385,613,824 (1.87 / 0.36 GiB) | — | — | — | OK |
| Disk | 0% | 0 / 0 | 0 | 0 | OK (log_disk_status: not_available) |
| Sessions (IPv4) | 1,057 | 1,035 / 1,057 | 1,643 | 1,923 (min 586) | OK |
| Sessions (IPv6) | 19 | 18 / 19 | 19 | 22 | OK |
| NPU-offloaded sessions | 381 | 389 / 413 | 594 | 878 | OK |
| NPU-offloaded sessions (v6) | 0 | 0 / 0 | 0 | 0 | OK |
| NTurbo sessions | 23 | 24 / 27 | 33 | 65 | OK |
| NTurbo sessions (v6) | 0 | 0 / 0 | 0 | 0 | OK |
| Session setup rate (IPv4) | 7 /s | 5 / 11 | 54 | 96 | OK |
| Session setup rate (IPv6) | 0 /s | 0 / 0 | 0 | 7 | OK |
| FortiCloud log rate | 17 msg/s | 21 / 46 | 50 | 79 | OK |
| Local disk log rate | 0 /s | 0 / 0 | 0 | 0 | OK |
| FortiAnalyzer log rate | 0 /s | 0 / 0 | 0 | 0 | OK |
| Faz-cloud log rate | 0 /s | 0 / 0 | 0 | 0 | OK |

## Flags (>80%)

- None. No CPU, memory, disk, session, or log-rate counter exceeded the 80% threshold.

## Notes

- `log_disk_status` reports `not_available` on this unit, so disk utilisation reads 0% and local log rates are 0; logging is flowing to FortiCloud (forticloud_lograte 17 msg/s).
- CPU peaked at 18% over the trailing 24 hours and is currently fully idle across all 8 cores.
- Memory is flat at 38% (24-hour max 39%) with 1.87 GiB free plus 0.36 GiB freeable.
- IPv4 session count sits near the low end of its daily band (current 1,057 vs 24h peak 1,923); roughly 36% of sessions are NPU-offloaded.
- Read-only check: no address, policy, route, VPN, or DHCP configuration was inspected for change and nothing was written to the firewall.
