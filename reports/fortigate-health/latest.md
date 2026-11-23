# fortigate-health - 2026-11-23

**Verdict: HEALTHY** — no resource counter above 80%; CPU fully idle, memory flat at 38%, sessions and setup rate well within FortiGate 80F limits.

- Device: fw-maniak-hq (FortiGate 80F, FGT80F)
- Firmware: FortiOS v7.4.11 build 2878
- Serial: FGT80FTK22061709
- VDOM: root
- Sample time: 2026-11-23 14:00:06 UTC (09:00:06 EST, America/Toronto)
- Sources: read-only `fg_system_status` + `fg_resource_usage` (no changes made)

## Resource usage

| Metric | Now | 1-min avg/max | 30-min max | 24-hour max | Status |
|---|---|---|---|---|---|
| CPU (8 cores, aggregate) | 0% | 0 / 0 | 5 | 19 | OK |
| Memory used % | 38% | 38 / 38 | 38 | 39 | OK |
| Memory used (bytes) | 1,508,921,344 (1.41 GiB) | — | — | — | OK |
| Memory total (bytes) | 3,897,774,080 (3.63 GiB) | — | — | — | OK |
| Memory free / freeable | 2,002,747,392 / 386,105,344 (1.87 / 0.36 GiB) | — | — | — | OK |
| Disk | 0% | 0 / 0 | 0 | 0 | OK (log_disk_status: not_available) |
| Sessions (IPv4) | 1,121 | 1,059 / 1,121 | 1,297 | 2,208 (min 633) | OK |
| Sessions (IPv6) | 11 | 10 / 16 | 20 | 21 | OK |
| NPU-offloaded sessions | 475 | 449 / 475 | 613 | 799 (min 286) | OK |
| NPU-offloaded sessions (v6) | 0 | 0 / 0 | 0 | 0 | OK |
| NTurbo sessions | 17 | 15 / 17 | 27 | 122 (min 9) | OK |
| NTurbo sessions (v6) | 0 | 0 / 0 | 0 | 0 | OK |
| Session setup rate (IPv4) | 10 /s | 6 / 14 | 47 | 105 | OK |
| Session setup rate (IPv6) | 0 /s | 0 / 0 | 8 | 8 | OK |
| FortiCloud log rate | 17 msg/s | 17 / 38 | 48 | 97 | OK |
| Local disk log rate | 0 /s | 0 / 0 | 0 | 0 | OK |
| FortiAnalyzer log rate | 0 /s | 0 / 0 | 0 | 0 | OK |
| Faz-cloud log rate | 0 /s | 0 / 0 | 0 | 0 | OK |

## Per-core CPU (fg_system_status)

| Core 0-6 | Core 7 |
|---|---|
| user 0 / system 0 / idle 100 / iowait 0 | user 1 / system 0 / idle 99 / iowait 0 |

Aggregate: user 0%, system 0%, nice 0%, idle 100%, iowait 0%.

## Flags (>80%)

- None. No CPU, memory, disk, session, setup-rate, or log-rate counter exceeded the 80% threshold.

## Notes

- `log_disk_status` reports `not_available` on this unit, so disk utilisation reads 0% and local/FortiAnalyzer log rates are 0; logging is flowing to FortiCloud (`forticloud_lograte` 17 msg/s, 24h peak 97).
- CPU is idle across all 8 cores right now; the trailing 24-hour peak was 19%, and the trailing-hour peak only 5%.
- Memory is essentially flat: 38% now, 24-hour band 38-39%, with 1.87 GiB free plus 0.36 GiB reclaimable.
- IPv4 sessions at 1,121 sit mid-band (24h range 633-2,208); about 42% of them are NPU-offloaded (475). IPv6 sessions and IPv6 setup rate are negligible.
- Setup rate of 10 new sessions/s is near the 1-minute peak (14/s) and far below the 24-hour peak of 105/s.
- Read-only check: only `fg_resource_usage` and `fg_system_status` were called. No address, policy, route, VPN, DHCP, or firmware change was attempted on the firewall.
