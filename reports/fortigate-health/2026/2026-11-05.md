# fortigate-health - 2026-11-05

**Verdict: HEALTHY** — nothing above 80%; CPU 0%, memory 38%, disk 0%, sessions 782 (24 h peak 2215), setup rate 23/s.

- Device: fw-maniak-hq — FortiGate 80F (FGT80F), serial FGT80FTK22061709
- FortiOS: v7.4.11 build 2878, VDOM `root`
- Sample time: 2026-11-05 09:00 America/Toronto (14:00 UTC), 60 s of history in 1 s buckets
- Collection: read-only only (`fg_system_status`, `fg_resource_usage`, `fg_log_state`) — no configuration changes made

## Resource usage (now)

| Metric | Current | 1-min avg / max | 1-hour max | 24-hour avg / max | > 80%? |
| --- | --- | --- | --- | --- | --- |
| CPU usage (all 8 cores) | 0 % | 0 % / 0 % | 7 % | 0 % / 21 % | no |
| Memory usage | 38 % (1.38 GiB / 3.63 GiB) | 37 % / 37 % | 38 % | 37 % / 39 % | no |
| Disk usage | 0 % | 0 % / 0 % | 0 % | 0 % / 0 % | no |
| Sessions (IPv4) | 782 | 724 / 782 | 2090 | 1038 / 2215 | n/a |
| Sessions (IPv6) | 18 | 12 / 18 | 18 | 8 / 25 | n/a |
| Session setup rate (IPv4) | 23 /s | 6 /s / 23 /s | 61 /s | 4 /s / 98 /s | n/a |
| Session setup rate (IPv6) | 0 /s | 0 /s / 6 /s | 7 /s | 0 /s / 8 /s | n/a |
| NPU-accelerated sessions | 414 | 367 / 414 | 771 | 425 / 952 | n/a |
| NPU-accelerated sessions (IPv6) | 0 | 0 / 0 | 0 | 0 / 0 | n/a |
| NTurbo sessions | 11 | 11 / 13 | 36 | 15 / 112 | n/a |
| NTurbo sessions (IPv6) | 0 | 0 / 0 | 0 | 0 / 0 | n/a |
| Log rate — FortiCloud | 12 /s | 10 /s / 14 /s | 120 /s | 14 /s / 120 /s | n/a |
| Log rate — local disk / FortiAnalyzer | 0 /s | 0 / 0 | 0 | 0 / 0 | n/a |

Memory detail: total 3,897,774,080 B (~3.63 GiB), used 1,480,028,160 B (~1.38 GiB), free 2,045,059,072 B (~1.90 GiB), freeable 372,686,848 B (~0.35 GiB).

CPU detail (instantaneous): user 0 %, system 0 %, nice 0 %, iowait 0 %, idle 100 % on all 8 cores — 24 h peak of 21 % is the worst spike seen, no sustained load.

## Flags / observations

- **Nothing above 80 %.** Highest utilisation in any window: memory 39 % (24 h max); CPU 21 % (24 h max). No action required.
- Sessions and setup rate are comfortably low; the 24 h session peak (2215) and setup-rate peak (98 /s) came as short bursts, and traffic is offloading well (414 of 782 sessions in the NPU, 11 in NTurbo).
- **Disk usage reads 0 % because there is no local log disk** — `log_disk_status: not_available` and the log device `disk` reports `is_available: false`, `is_enabled: false` (disabled by default). This is expected for this device, not a healthy disk reading.
- Logging path is FortiCloud (enabled, default): 12 events/s currently, quota 500,000,000 B with 0 B reported used. FortiAnalyzer and FortiAnalyzer-cloud are disabled; local `disk_lograte` and `faz_lograte` are 0/s accordingly.
- IPv6 usage is negligible (18 sessions, 0 /s setup) — informational only.

## Verdict

HEALTHY — all counters well under the 80 % threshold; no watch or critical items. Next check: 2026-11-06.
