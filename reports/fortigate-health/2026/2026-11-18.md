# fortigate-health - 2026-11-18

**Verdict: HEALTHY — no counter at or above 80%.**

- Host: fw-maniak-hq (FortiGate 80F, serial FGT80FTK22061709)
- FortiOS: v7.4.11 build 2878, VDOM root
- Sampled: 2026-11-18 09:00:06 EST (America/Toronto) — last datapoint epoch-ms 1795010406000
- Source tools: `fg_resource_usage`, `fg_system_status` (read-only; no configuration changes made)

## Resource counters

| Metric | Current | 1-min avg | 24-h max | Notes |
|---|---|---|---|---|
| CPU (all cores) | 0 % | 0 % | 18 % | 8 cores: 7 at 100 % idle, 1 at 99 % idle (1 % user) |
| Memory | 38 % (1.41 GiB used / 3.63 GiB total) | 38 % | 39 % | free 1.86 GiB, freeable 0.36 GiB (total 3,897,774,080 B; used 1,515,929,600 B) |
| Disk usage | 0 % | 0 % | 0 % | `log_disk_status: not_available` — no local log disk |
| Sessions (IPv4) | 744 | 780 | 1,743 | 10-min range 639–858 |
| Sessions (IPv6) | 11 | 10 | 20 | |
| Session setup rate (IPv4) | 1 /s | 5 /s | 128 /s | 10-min max 31 /s |
| Session setup rate (IPv6) | 0 /s | 0 /s | 7 /s | |
| NPU-offloaded sessions | 359 | 387 | 921 | ~48 % of IPv4 sessions hardware-offloaded |
| NPU-offloaded IPv6 sessions | 0 | 0 | 0 | |
| NTurbo sessions | 25 | 25 | 69 | |
| NTurbo IPv6 sessions | 0 | 0 | 0 | |
| FortiCloud log rate | 19 ev/s | 12 ev/s | 220 ev/s | 24-h avg 10 ev/s — log shipping active |
| Local disk log rate | 0 ev/s | 0 | 0 | consistent with no disk |
| FortiAnalyzer log rate | 0 ev/s | 0 | 0 | no FortiAnalyzer configured |
| FortiAnalyzer-over-cloud log rate | 0 ev/s | 0 | 0 | |

## Flags (threshold > 80 %)

None. No metric reached or exceeded 80 %.

## Notes / observations

- CPU is effectively idle (0 % aggregate); 24-h peak was 18 % and 12-h peak 18 % — well inside headroom for the 80F.
- Memory is flat at 38 % across all windows (24-h max 39 %) with 1.86 GiB free plus 0.36 GiB freeable — stable, no leak signature.
- Session load is light (744 current vs 1,743 24-h peak) and setup rate is low (avg 2–5 /s), so no session-table or new-session pressure.
- About half the IPv4 sessions are offloaded to the NP (`npu_session` 359) plus 25 NTurbo sessions; the CPU is not carrying the forwarding load.
- Disk counters read 0 and `log_disk_status` is `not_available`, so disk percentage is not a meaningful health signal on this unit; logging is going to FortiCloud (`forticloud_lograte` ~10–19 ev/s), with no local-disk or FortiAnalyzer log rate.
