# fortigate-health - 2026-11-19

**Verdict: HEALTHY** — no resource counter above 80%; every reported metric sits below 50%.

## Device context (`fg_system_status`)

| Field | Value |
|---|---|
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | v7.4.11 build 2878 |
| VDOM | root |
| Log disk status | `not_available` |
| API status | HTTP 200 / `status: success` (both calls) |

Sample time: **2026-11-19 09:00:06 AM America/Toronto** (2026-11-19 14:00:06 UTC) — latest datapoint returned by `fg_resource_usage`.
Method: read-only `fg_system_status` + `fg_resource_usage` against the monitor API. No configuration changes were made.

## Resource usage right now (`fg_resource_usage`)

| Counter | Current | Supporting detail |
|---|---|---|
| CPU | **0 %** | 8/8 cores report 100 % idle (user 0 / system 0 / iowait 0); window maxima — 1-min 0 %, 10-min 1 %, 30-min 5 %, 1-hour 5 %, 12-hour 21 %, 24-hour 21 % |
| Memory | **38 %** | used 1,516,994,560 B (1.41 GiB) / total 3,897,774,080 B (3.63 GiB) = 38.9 %; free 1,997,230,080 B (1.86 GiB), freeable 383,549,440 B (0.36 GiB); flat 38 % across all windows, 24-h max 39 % |
| Disk | **0 % (not reported)** | disk counter is 0 in every window (1-min…24-h) and `log_disk_status: not_available` — no usable local disk-usage percentage is exposed |
| Sessions (IPv4) | **841** | 1-min avg 864 / max 925; 1-hour avg 901 / max 1342; 24-h avg 999 / max 3263 |
| Sessions (IPv6) | **10** | 1-min avg 10; 24-h avg 9 / max 21 |
| Session setup rate (IPv4) | **0 new/s** | 1-min avg 4 / max 14; 1-hour avg 4; 24-h avg 4 / peak 173 |
| Session setup rate (IPv6) | **0 new/s** | 24-h avg 0 / peak 5 |
| NPU-offloaded sessions | **427** | ≈51 % of IPv4 sessions hardware-offloaded; 24-h max 1310 |
| NPU-offloaded sessions (IPv6) | **0** | flat 0 across all windows |
| NTurbo sessions | **31** | 1-min avg 32; 24-h avg 19 / max 74 |
| NTurbo sessions (IPv6) | **0** | flat 0 across all windows |
| Disk log rate | **0 events/s** | flat 0 — consistent with disk logging being unavailable |
| FortiAnalyzer log rate | **0 events/s** | flat 0 |
| FortiCloud log rate | **14 events/s** | 1-min avg 13 / max 24; 24-h avg 12 / peak 124 — cloud logging is flowing |
| FortiAnalyzer→FortiCloud log rate | **0 events/s** | flat 0 |

## Flags above 80 %

**None.** No counter approached the 80 % threshold in the live sample or in any historical window:

- CPU peaked at **21 %** (24-h window), currently 0 %.
- Memory peaked at **39 %** (24-h window), currently 38 %.
- Sessions peaked at **3263** (24-h window) with a setup-rate spike of 173/s — no sign of session-table pressure.

## Watch items (informational, not alerts)

1. **Disk usage is unverifiable from these tools.** The `disk` counter returns 0 for every window and `fg_system_status` reports `log_disk_status: not_available`. That is a *missing measurement*, not a proven-healthy one — there is no local disk-usage figure to assess. Worth confirming out-of-band (e.g. `get system performance status` / `diagnose sys flash list`) before treating disk as checked.
2. **Log delivery is FortiCloud-only.** `forticloud_lograte` is alive at ~11–14 events/s while `disk_lograte` and `faz_lograte` are both pinned at 0. Consistent with the `not_available` disk state, but it means there is a single log destination and no local/FortiAnalyzer redundancy.
3. **Setup rate sampled at 0/s.** The instantaneous value is 0; the 1-minute average of 4/s and the 24-h peak of 173/s indicate normal bursty home-lab traffic, not a stall.

## Baseline for tomorrow's run

```
date=2026-11-19  cpu=0%  mem=38% (1.41/3.63 GiB)  disk=n/a  sess4=841  sess6=10
setup4=0/s  setup6=0/s  npu=427  npu6=0  nturbo=31  nturbo6=0
forticloud_lograte=14/s  disk_lograte=0/s  faz_lograte=0/s
verdict=healthy
```
