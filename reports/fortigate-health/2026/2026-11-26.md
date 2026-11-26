# fortigate-health - 2026-11-26

**Verdict: HEALTHY** — all resource counters are far below the 80% alert threshold; nothing to flag.

Device: fw-maniak-hq · FortiGate 80F (FGT80F) · serial FGT80FTK22061709 · FortiOS v7.4.11 build 2878 · VDOM root
Sample time: 2026-11-26 14:00:05 UTC (09:00 EST, America/Toronto) · source: `fg_system_status`, `fg_resource_usage` (read-only)

| Metric | Current | 1-min avg | 24-h avg | 24-h max | Status |
|---|---|---|---|---|---|
| CPU usage (all 8 cores) | 0% | 0% | 0% | 18% | OK |
| Memory usage | 38% | 38% | 38% | 39% | OK |
| Memory used / total | 1.41 GiB / 3.63 GiB (1,513,156,608 / 3,897,774,080 B) | — | — | — | OK |
| Memory free / freeable | 1.86 GiB / 0.36 GiB | — | — | — | OK |
| Disk usage | n/a — `log_disk_status: not_available` (no local disk logging) | — | — | — | N/A |
| Sessions (IPv4) | 627 | 618 | 966 | 1,819 | OK |
| Sessions (IPv6) | 9 | 8 | 9 | 22 | OK |
| Session setup rate (sec^-1) | 5 | 4 | 3 | 210 | OK |
| Session setup rate v6 (sec^-1) | 0 | 0 | 0 | 7 | OK |
| NPU-offloaded sessions | 345 | 338 | 371 | 910 | OK |
| NPU-offloaded sessions v6 | 0 | 0 | 0 | 0 | OK |
| NTurbo sessions | 16 | 14 | 17 | 78 | OK |
| NTurbo sessions v6 | 0 | 0 | 0 | 0 | OK |
| Disk log rate (msg/s) | 0 | 0 | 0 | 0 | OK |
| FortiAnalyzer log rate | 0 | 0 | 0 | 0 | OK |
| FortiCloud log rate (msg/s) | 10 | 8 | 13 | 118 | OK |
| FortiAnalyzer-to-Cloud log rate | 0 | 0 | 0 | 0 | OK |

## Notes / >80% flags

- None — no counter reached 80%.
- CPU near-idle: 1-min 0%, 10-min max 1%, 30-min/1-hour max 9%, 12-hour max 17%, 24-hour max 18%.
- Memory flat at 38% across all sampling windows (peak 39%); ~2.2 GiB available including freeable memory.
- Session load is normal home-lab scale: 627 now vs. 24-hour peak of 1,819; setup rate steady at ~3-5 sec^-1 with a 24-hour burst peak of 210.
- Disk: the 80F reports `log_disk_status: not_available`, so the API exposes no disk-usage percentage. Not a fault — nothing to measure. `disk_lograte` is 0, consistent with no local disk logging.

Report generated read-only; no changes were made on the firewall.
