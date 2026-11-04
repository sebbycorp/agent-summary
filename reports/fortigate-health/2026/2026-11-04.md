# fortigate-health - 2026-11-04

**Verdict: HEALTHY** — all resource counters well under the 80% threshold; nothing to flag.

## Device context

| Field | Value |
| --- | --- |
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | v7.4.11 build 2878 |
| VDOM | root |
| Log disk status | not_available |
| Sample time | 2026-11-04 08:33:26 EST (13:33:26 UTC) |

## Resource usage (live)

| Counter | Current | 1-min avg | 1-hour avg / max | 24-hour avg / max | Status |
| --- | --- | --- | --- | --- | --- |
| CPU (%) | 0 | 0 (max 3) | 0 / 12 | 0 / 23 | OK |
| Memory (%) | 37 | 37 | 37 / 38 | 37 / 39 | OK |
| Memory (bytes) | 1.38 GiB used / 3.63 GiB total | — | — | — | 37.95% used |
| Disk (%) | 0 | 0 | 0 / 0 | 0 / 0 | OK (log disk not_available) |
| Sessions (IPv4) | 1,014 | 1,047 (max 1,073) | 1,151 / 1,778 | 1,040 / 7,602 | OK |
| Sessions (IPv6) | 9 | 9 (max 10) | 11 / 21 | 10 / 21 | OK |
| Session setup rate (/s) | 3 | 5 (max 19) | 5 / 121 | 4 / 192 | OK |
| Setup rate IPv6 (/s) | 0 | 0 | 0 / 7 | 0 / 9 | OK |
| NPU-offloaded sessions | 388 | 410 (max 440) | 441 / 590 | 388 / 1,379 | OK |
| NPU IPv6 sessions | 0 | 0 | 0 / 0 | 0 / 0 | OK |
| NTurbo sessions | 15 | 16 (max 18) | 13 / 41 | 14 / 55 | OK |
| NTurbo IPv6 sessions | 0 | 0 | 0 / 0 | 0 / 0 | OK |
| FortiCloud log rate (/s) | 13 | 12 (max 17) | 15 / 82 | 13 / 170 | OK |
| Local disk log rate (/s) | 0 | 0 | 0 / 0 | 0 / 0 | OK |
| FortiAnalyzer log rate (/s) | 0 | 0 | 0 / 0 | 0 / 0 | OK |
| FortiAnalyzer-cloud log rate (/s) | 0 | 0 | 0 / 0 | 0 / 0 | OK |

CPU core breakdown (fg_system_status): all 8 cores 100% idle — user 0, system 0, nice 0, iowait 0.

Memory detail (fg_system_status): total 3,897,774,080 B (3.63 GiB), used 1,479,372,800 B (1.38 GiB), free 2,046,779,392 B (1.91 GiB), freeable 371,621,888 B (0.35 GiB).

## Flags (>80%)

None. Highest observed values in the 24-hour window: CPU 23%, memory 39%, disk 0%, sessions 7,602, setup rate peak 192/s. No counter is anywhere near the 80% alert line.

## Notes

- Data is read-only, from `fg_resource_usage` and `fg_system_status` only; nothing was modified on the firewall.
- Disk utilization reads 0 and the device reports `log_disk_status: not_available`, so disk is not a meaningful health signal on this unit — logging is offloaded to FortiCloud, which is the active log channel at ~13 events/s.
- Session and setup-rate 24-hour peaks (7,602 sessions / 192 new sessions per second) are transient bursts; the platform average is ~1,040 sessions at ~4 new/s and CPU stayed flat at 0–1% through them.
- Memory is the only steadily-loaded metric (37% now, 39% 24-hour max) and it is flat, with no drift or upward trend.

## History

- Rolling copy: `reports/fortigate-health/latest.md`
- Snapshot of record: `reports/fortigate-health/2026/2026-11-04.md`
