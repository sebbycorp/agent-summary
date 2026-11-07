# fortigate-health - 2026-11-07

**Verdict: HEALTHY** — CPU 0%, memory 38%, disk 0%, sessions ~1k (24h peak 2,475). No counter above 80%; nothing to flag.

- Device: fw-maniak-hq (FortiGate 80F, FGT80F) — serial FGT80FTK22061709
- FortiOS: v7.4.11 build 2878, VDOM: root
- Sampled: 2026-11-07 14:00:06 UTC (09:00 America/Toronto)
- Sources: read-only `fg_resource_usage` + `fg_system_status` (no changes made to the firewall)

## Resource usage

| Metric | Value | 24 h max | % of limit |
|---|---|---|---|
| CPU (total) | 0% — 8 cores, all 100% idle (user 0 / system 0 / iowait 0) | 17% | 0% |
| CPU 1-min avg / peak | 0% / 1% | — | — |
| CPU 1-hour avg / peak | 0% / 8% | — | — |
| Memory | 38% used — 1.38 GiB used / 3.63 GiB total (1,482,018,816 / 3,897,774,080 bytes) | 39% | 38% |
| Memory free / freeable | 1.90 GiB free (2,041,249,792 B) / 357 MiB freeable (374,505,472 B) | — | — |
| Disk | 0% — `log_disk_status: not_available` (no local log disk) | 0% | 0% |
| Sessions (IPv4) | 1,024 | 2,475 (avg 940) | not reported by tool |
| Sessions (IPv6) | 11 | 22 (avg 10) | n/a |
| Session setup rate (IPv4) | 3 /s (1-min avg 7 /s, 1-min peak 27 /s) | 378 /s | n/a |
| Session setup rate (IPv6) | 0 /s | 8 /s | n/a |
| NPU (hardware-offloaded) sessions | 414 IPv4 / 0 IPv6 | 1,569 (IPv4) | n/a |
| NTurbo sessions | 18 IPv4 / 0 IPv6 | 65 (IPv4) | n/a |
| Log rate — FortiCloud | 12 msg/s (24 h avg 11) | 240 | n/a |
| Log rate — local disk | 0 msg/s | 0 | n/a |
| Log rate — FortiAnalyzer | 0 msg/s | 0 | n/a |
| Log rate — FortiAnalyzer cloud | 0 msg/s | 0 | n/a |

## Threshold flags (>80%)

None. Highest observed utilization across all returned counters: memory 38% (24 h peak 39%); CPU 24 h peak 17%.

## Notes

- The resource-usage API returns no session-limit / max-session field, so session % of capacity cannot be stated from this data.
- Local disk logging and FortiAnalyzer logging are inactive (`disk_lograte`, `faz_lograte`, `faz_cloud_lograte` all 0); logging is flowing to FortiCloud at ~11-12 msg/s.
- Traffic profile is normal for this lab: ~1k concurrent IPv4 sessions with ~400-500 already NPU-offloaded; IPv6 usage is negligible.
