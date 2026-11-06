# fortigate-health - 2026-11-06

**Verdict: HEALTHY** — nothing above 80%; CPU 0%, memory 37% (1.38 GiB / 3.63 GiB), disk 0%, sessions 817 (24 h peak 4955), setup rate 9/s.

- Device: fw-maniak-hq — FortiGate 80F (FGT80F), serial FGT80FTK22061709
- FortiOS: v7.4.11 build 2878, VDOM `root`
- Sample time: 2026-11-06 09:00 America/Toronto (14:00 UTC) — latest counter bucket
- Collection: read-only only (`fg_system_status`, `fg_resource_usage`) — no configuration changes made

## Resource usage (now)

| Metric | Current | 1-min avg / max | 1-hour max | 24-hour avg / max | > 80%? |
| --- | --- | --- | --- | --- | --- |
| CPU usage (all 8 cores) | 0 % | 0 % / 0 % | 9 % | 0 % / 18 % | no |
| Memory usage | 37 % (1.38 GiB / 3.63 GiB) | 37 % / 37 % | 38 % | 37 % / 39 % | no |
| Disk usage | 0 % | 0 % / 0 % | 0 % | 0 % / 0 % | no |
| Sessions (IPv4) | 817 | 803 / 830 | 1416 | 1040 / 4955 | n/a |
| Sessions (IPv6) | 18 | 15 / 19 | 21 | 10 / 23 | n/a |
| Session setup rate (IPv4) | 9 /s | 5 /s / 15 /s | 71 /s | 4 /s / 219 /s | n/a |
| Session setup rate (IPv6) | 0 /s | 0 /s / 0 /s | 9 /s | 0 /s / 10 /s | n/a |
| NPU-accelerated sessions (IPv4) | 397 | 403 / 413 | 650 | 444 / 1293 | n/a |
| NPU-accelerated sessions (IPv6) | 0 | 0 / 0 | 0 | 0 / 0 | n/a |
| NTurbo sessions (IPv4) | 20 | 19 / 20 | 64 | 16 / 114 | n/a |
| NTurbo sessions (IPv6) | 0 | 0 / 0 | 0 | 0 / 0 | n/a |
| Log rate — FortiCloud | 13 /s | 11 /s / 17 /s | 43 /s | 13 /s / 196 /s | n/a |
| Log rate — local disk | 0 /s | 0 / 0 | 0 | 0 / 0 | n/a |
| Log rate — FortiAnalyzer | 0 /s | 0 / 0 | 0 | 0 / 0 | n/a |
| Log rate — FortiAnalyzer cloud | 0 /s | 0 / 0 | 0 | 0 / 0 | n/a |

Memory detail: total 3,897,774,080 B (~3.63 GiB), used 1,480,065,024 B (~1.38 GiB), free 2,044,104,704 B (~1.90 GiB), freeable 373,604,352 B (~0.35 GiB).

CPU detail (instantaneous): user 0 %, system 0 %, nice 0 %, iowait 0 %, idle 100 % on all 8 cores. 10-min/30-min peak 2 %, 1-hour peak 9 %, 24-hour peak 18 % — no sustained load.

## Flags / observations

- **Nothing above 80 %.** Highest utilisation in any window is memory at 39 % (24 h max); worst CPU spike in 24 h is 18 %. No watch or critical items.
- Sessions and setup rate are low right now (817 sessions, 9 new/s). The 24 h session peak of **4955** and setup-rate peak of **219 /s** were short bursts, not sustained load (30-min session average is 1041).
  - Note vs. yesterday's check: the 24 h session peak rose from 2215 → 4955 and the setup-rate peak from 98/s → 219/s. Still well within 80F capacity, but worth watching whether that burst recurs.
- Offload is healthy: 397 of 817 IPv4 sessions are NPU-accelerated, 20 in NTurbo.
- **Disk usage reads 0 % because there is no local log disk** — `log_disk_status: not_available`. This is an absent device, not a healthy disk reading.
- All local/FortiAnalyzer log rates are 0 /s; the only active logging path is FortiCloud at 13 events/s.
- IPv6 usage is negligible (18 sessions, 0 /s setup) — informational only.

## Verdict

HEALTHY — all counters well under the 80 % threshold; nothing to flag. Next check: 2026-11-07.
