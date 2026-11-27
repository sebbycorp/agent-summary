# fortigate-health - 2026-11-27

**Verdict: HEALTHY** — every resource counter is well below the 80% threshold; nothing to flag.

- **Device:** fw-maniak-hq — FortiGate 80F, serial FGT80FTK22061709, FortiOS v7.4.11 build 2878, VDOM root
- **Check time:** 2026-11-27 09:00:05 America/Toronto (14:00:05 UTC)
- **Sources:** read-only `fg_resource_usage` + `fg_system_status` (plus `fg_log_state` for the disk/logging context). No configuration was read back with write tools and nothing on the firewall was changed.

## Resource usage

| Metric | Current | 24 h max | 24 h avg | Status |
|---|---|---|---|---|
| CPU (all 8 cores) | 0 % | 19 % | 0 % | OK |
| Memory | 38 % — 1.41 GiB / 3.63 GiB (1,516,670,976 / 3,897,774,080 bytes) | 39 % | 38 % | OK |
| Disk | 0 % (see note) | 0 % | 0 % | OK / n-a |
| Sessions (IPv4) | 1,078 | 2,016 | 912 | OK |
| Sessions (IPv6) | 15 | 21 | 10 | OK |
| Session setup rate | 12 /s | 172 /s | 3-4 /s | OK |
| Session setup rate (IPv6) | 0 /s | 7 /s | 0 /s | OK |
| NPU-offloaded sessions | 487 | 1,044 | 397 | OK |
| NPU-offloaded sessions (IPv6) | 0 | 0 | 0 | OK |
| NTurbo sessions | 26 | 92 | 20 | OK |
| NTurbo sessions (IPv6) | 0 | 0 | 0 | OK |
| FortiCloud log rate | 11 /s | 107 /s | 11 /s | OK |
| Local-disk log rate | 0 /s | 0 /s | 0 /s | n/a |
| FortiAnalyzer log rate | 0 /s | 0 /s | 0 /s | n/a |
| FortiAnalyzer-cloud log rate | 0 /s | 0 /s | 0 /s | n/a |

## Threshold review (>80%)

No metric exceeded 80%. The highest sustained values were memory at 38-39%, CPU peaking at 19% over the last hour/24h window with all 8 cores currently reporting 100% idle, and session count peaking at 2,016 in 24 hours against a 1,078 instantaneous value. Setup-rate spikes (172/s peak in 24 h) are short bursts with no CPU or memory consequence.

## Notes

- **Memory detail:** free 1.86 GiB (1,992,949,760 bytes) plus 0.36 GiB (388,153,344 bytes) reclaimable/cache; no memory pressure and no sign of leak-driven growth (24 h min/max 38%/39%).
- **Disk note:** the reported 0% disk is *not* an independent healthy-disk reading — disk logging is unavailable on this unit (`log_disk_status: not_available`, local disk `is_available: false`, 0 SSDs available, FortiAnalyzer disabled). Logs are forwarded to FortiCloud (enabled, 500 MB quota, 0 bytes currently used). There is therefore no local log-partition fill risk to monitor.
- **Session mix:** roughly 45% of IPv4 sessions are NPU-offloaded (487 of 1,078) and 26 are NTurbo-accelerated, which is consistent with the near-zero CPU usage.
- **Recommendation:** none required. Continue daily collection; only memory trend, CPU spikes sustained above 80%, or a session count approaching the 80F platform limit would warrant action.
