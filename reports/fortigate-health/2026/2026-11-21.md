# fortigate-health - 2026-11-21

**Verdict: HEALTHY** - all resource counters are well below the 80% threshold (highest: memory at 39%).

## Device context

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | v7.4.11 build 2878 |
| VDOM | root |
| Snapshot time | 2026-11-21 09:00 EST (America/Toronto) |
| Sources | `fg_resource_usage`, `fg_system_status` (read-only) |

## Resource usage

| Metric | Value | Notes / history |
|---|---|---|
| CPU (aggregate) | **0%** | idle 100%; per-core max 1%; 30-min max 18%, 24-h max 20% |
| Memory | **39%** - 1.42 GiB used / 3.63 GiB total | free 1.85 GiB + freeable 367 MiB; 24-h band 38-40% |
| Disk | **0%** | `log_disk_status: not_available` (no local disk logging) |
| Sessions (IPv4) | **1,194** | 1-min avg 1,189; 24-h max 2,063 |
| Sessions (IPv6) | **10** | 24-h max 21 |
| Sessions (total) | **1,204** | |
| Session setup rate (IPv4) | **4 /s** | 1-min avg 8 (peak 22); 24-h max 85 |
| Session setup rate (IPv6) | 0 /s | 24-h max 8 |
| NPU-offloaded sessions | 532 (v6: 0) | 24-h max 1,145 |
| NTurbo sessions | 24 (v6: 0) | 24-h max 84 |
| Disk log rate | 0 /s | no local disk logging |
| FortiAnalyzer log rate | 0 /s | FAZ (and FAZ-over-cloud) idle |
| FortiCloud log rate | 14 /s | 24-h avg 12, max 62 |

## >80% flags

- **None.** No counter reported by `fg_resource_usage` reached 80% at snapshot time or in the 24-hour history (memory peaked at 40%, CPU at 20%).

## Notes

- Snapshot is a point-in-time read; session count fluctuates in the 615-2,063 range over 24 h, which is normal for this lab.
- Disk usage reads 0% because the FortiGate reports `log_disk_status: not_available` (logging is to FortiCloud, not a local disk) - this is an observation, not a fault.
- No configuration changes were made; this check used read-only tools only.
