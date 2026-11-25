# fortigate-health - 2026-11-25

**Verdict: HEALTHY** — all resource counters well under 80%; nothing to flag.

## Context

| Field | Value |
| --- | --- |
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | v7.4.11 build 2878 |
| VDOM | root |
| Sample time | 2026-11-25 14:00:07 UTC (09:00:07 EST, America/Toronto) |
| Source | fg_system_status + fg_resource_usage (monitor API), fg_log_state |

## Resource usage (current)

| Metric | Current | 1-min avg | 10-min max | 24-hr max | Status |
| --- | --- | --- | --- | --- | --- |
| CPU (aggregate, 8 cores) | 0% | 0% | 0% | 20% | OK |
| Memory | 38% (1.41 GiB used / 3.63 GiB total) | 38% | 38% | 39% | OK |
| Disk | 0% — no disk-based logging | 0% | 0% | 0% | OK (N/A) |
| Sessions (IPv4) | 597 | 579 | 811 | 1,899 | OK |
| Sessions (IPv6) | 9 | 9 | 18 | 21 | OK |
| Session setup rate (IPv4) | 0 /s | 2 /s (max 9) | 17 /s | 98 /s | OK |
| Session setup rate (IPv6) | 0 /s | 0 /s | 0 /s | 8 /s | OK |

## Other counters returned by the tool

| Counter | Current | 24-hr avg | 24-hr max | Status |
| --- | --- | --- | --- | --- |
| NPU (offloaded) sessions IPv4 | 336 | 418 | 885 | OK |
| NPU (offloaded) sessions IPv6 | 0 | 0 | 0 | OK |
| NTurbo sessions IPv4 | 29 | 17 | 73 | OK |
| NTurbo sessions IPv6 | 0 | 0 | 0 | OK |
| Disk log rate | 0 | 0 | 0 | OK (disk logging disabled) |
| FortiAnalyzer log rate | 0 | 0 | 0 | OK (FAZ disabled) |
| FortiCloud log rate | 12 /s | 14 /s | 129 /s | OK |
| FortiGate-FortiCloud (faz_cloud) log rate | 0 | 0 | 0 | OK |

## Memory detail

| Field | Bytes | Size |
| --- | --- | --- |
| Total | 3,897,774,080 | 3.63 GiB |
| Used | 1,510,289,408 | 1.41 GiB (38.7%) |
| Free | 2,000,379,904 | 1.86 GiB |
| Freeable (cache/buffers) | 387,104,768 | 369 MiB |

## Logging / disk detail

- `log_disk_status: not_available` — no SSD/disk log device on this unit (`disk` log device `is_available: false`, `disabled_by_default: true`), so disk utilization is reported as 0% and is effectively not applicable.
- Log destination is memory + FortiCloud (`log_device_default: forticloud`, FortiCloud enabled, FortiAnalyzer disabled).
- FortiCloud log quota: 500,000,000 bytes; used reported as 0.

## Flags above 80%

- None. Highest observed values at sample time: memory 38%, CPU 0% (24-hr CPU peak 20%), disk 0%/N-A.
- Note (informational, not a threshold breach): IPv4 session count peaked at 1,899 in the last 24h vs 597 now, and the FortiCloud log rate briefly peaked at 129/s. No session-capacity figure is returned by the tool, so no percentage-of-maximum is asserted.

---
Generated read-only from the FortiGate monitor API; no configuration was changed.
