# fortigate-health - 2026-11-04

**Verdict: HEALTHY** — all resource counters are far below the 80% alert threshold. Nothing to flag.

## Snapshot (read-only)

| Metric | Value | Notes |
|---|---|---|
| Hostname / Model | fw-maniak-hq / FortiGate 80F (FGT80F) | serial FGT80FTK22061709 |
| FortiOS | v7.4.11 build 2878 | VDOM: root |
| CPU (current) | 0% | 8 cores, each 100% idle; 1-min avg 0%, 24-h max 23% |
| Memory (current) | 37% | used 1,477,636,096 B (1.38 GiB) / total 3,897,774,080 B (3.63 GiB); free 2,048,516,096 B (1.91 GiB), freeable 371,621,888 B (0.35 GiB); 24-h max 39% |
| Disk | N/A (reported 0%) | `log_disk_status: not_available` — no local disk logging on this unit, so disk % is not meaningful |
| Sessions (IPv4) | 818 | 1-min avg 838, 10-min max 1,052; 24-h max 7,602 |
| Sessions (IPv6) | 9 | 1-min avg 7; 24-h max 21 |
| NPU-offloaded sessions | 371 IPv4 / 0 IPv6 | 24-h max 1,379 |
| NTurbo sessions | 16 IPv4 / 0 IPv6 | 24-h max 55 |
| Session setup rate | 2/sec IPv4 | 1-min avg 3/sec, 10-min max 21/sec; 24-h max 192/sec |
| Session setup rate (IPv6) | 0/sec | 24-h max 9/sec |
| Log rate - FortiCloud | 9 events/unit | 1-min avg 12; 24-h max 170 |
| Log rate - local disk / FortiAnalyzer | 0 | disk logging not available |

- Sample window (most recent monitor point): 2026-11-04 09:00:03 EST (America/Toronto).

## >80% alerts
- None. CPU 0%, memory 37%, sessions and setup rate are a small fraction of the 80F's capacity.

## Notes
- Data taken only from `fg_system_status` and `fg_resource_usage`; no configuration was read back or modified.
- Memory is the only counter worth continuing to watch over time (37% now, 39% 24-h peak) — still well inside normal range.
