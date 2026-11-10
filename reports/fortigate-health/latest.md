# fortigate-health - 2026-11-10

**Verdict: HEALTHY** — all resource counters well below the 80% threshold; nothing to flag.

- Device: fw-maniak-hq — FortiGate 80F (FGT80FTK22061709), FortiOS v7.4.11 build 2878, VDOM root
- Sample time: 2026-11-10 14:00:06 UTC (09:00 EST, America/Toronto)
- Source: read-only `fg_system_status` + `fg_resource_usage` (no changes made)

## Resource usage

| Metric | Current | 1-min avg | 1-hour max | 24-hour max | Threshold 80% | Status |
|---|---|---|---|---|---|---|
| CPU (%) | 0 | 0 | 10 | 18 | No | OK |
| Memory (%) | 38 | 38 | 38 | 39 | No | OK |
| Memory (used / total) | 1,487,794,176 B / 3,897,774,080 B (1.39 GiB / 3.63 GiB) | — | — | — | — | OK |
| Disk (%) | 0 (log_disk_status: not_available) | 0 | 0 | 0 | No | OK |
| Sessions (IPv4) | 1,023 | 803 | 1,149 | 1,319 | — | OK |
| Sessions (IPv6) | 12 | 11 | 19 | 20 | — | OK |
| Session setup rate (IPv4, /s) | 16 | 9 | 38 | 81 | — | OK |
| Session setup rate (IPv6, /s) | 0 | 0 | 5 | 8 | — | OK |
| NPU offloaded sessions | 459 | 272 | 459 | 574 | — | OK |
| NPU offloaded sessions (IPv6) | 0 | 0 | 0 | 0 | — | OK |
| NTurbo sessions | 28 | 28 | 75 | 75 | — | OK |
| NTurbo sessions (IPv6) | 0 | 0 | 0 | 0 | — | OK |
| Disk log rate (/s) | 0 | 0 | 0 | 0 | — | OK |
| FortiAnalyzer log rate (/s) | 0 | 0 | 0 | 0 | — | OK |
| FortiCloud log rate (/s) | 20 | 14 | 46 | 67 | — | OK |
| FortiAnalyzer-to-cloud log rate (/s) | 0 | 0 | 0 | 0 | — | OK |

## Notes

- CPU per-core snapshot (8 cores): core 1 at 1% user / 99% idle, cores 2–8 at 100% idle; aggregate idle 100%, iowait 0%.
- Memory is very stable: 37–39% across the whole 24-hour window (free 1.89 GiB, freeable 0.35 GiB).
- Sessions are the busiest live counter at 1,023 (24 h peak 1,319) — a fraction of the 80F session capacity; ~45% of sessions are NPU-offloaded, so CPU stays near idle.
- Disk usage is reported as 0% and the status API returns `log_disk_status: not_available`, which is expected when no disk logging is in use; log traffic is flowing to FortiCloud (~13 events/s average) rather than to disk or a local FortiAnalyzer.
- No counter is at or above 80% — no watch or critical items today.
