# fortigate-health - 2026-11-09

**Verdict: HEALTHY** — no resource counter at or above 80%; CPU idle, memory 38%, sessions well within limits.

## Device context

| Field | Value |
| --- | --- |
| Hostname | fw-maniak-hq |
| Model | FortiGate 80F (FGT80F) |
| Serial | FGT80FTK22061709 |
| FortiOS | v7.4.11 build 2878 |
| VDOM | root |
| Sample time | 2026-11-09 09:00 EST (America/Toronto) |
| log_disk_status | not_available (no local disk log destination) |

## Resource usage (fg_resource_usage)

| Counter | Current | 1-min avg / max | 1-hour avg / max | 24-hour avg / max | >80%? |
| --- | --- | --- | --- | --- | --- |
| CPU % (all 8 cores) | 0% (100% idle) | 0 / 0 | 0 / 10 | 0 / 19 | No |
| Memory % | 38% | 38 / 38 | 38 / 38 | 38 / 39 | No |
| Memory used / total | 1.38 GiB / 3.63 GiB (1,486,979,072 / 3,897,774,080 bytes) | free 1.89 GiB, freeable 0.35 GiB | — | — | No |
| Disk % | 0% | 0 / 0 | 0 / 0 | 0 / 0 | No |
| Sessions (IPv4) | 740 | 716 / 754 | 779 / 946 | 840 / 1799 | No |
| Sessions (IPv6) | 17 | 12 / 17 | 11 / 20 | 10 / 21 | No |
| Session setup rate (IPv4) | 10 /sec | 4 / 10 | 3 / 50 | 3 / 99 | No |
| Session setup rate (IPv6) | 0 /sec | 0 / 0 | 0 / 6 | 0 / 8 | No |
| NPU offloaded sessions | 211 | 208 / 223 | 204 / 264 | 251 / 1001 | No |
| NPU offloaded sessions v6 | 0 | 0 / 0 | 0 / 0 | 0 / 0 | No |
| NTurbo sessions | 24 | 22 / 24 | 25 / 50 | 27 / 97 | No |
| NTurbo sessions v6 | 0 | 0 / 0 | 0 / 0 | 0 / 0 | No |
| Disk log rate | 0 /sec | 0 / 0 | 0 / 0 | 0 / 0 | No |
| FortiAnalyzer log rate | 0 /sec | 0 / 0 | 0 / 0 | 0 / 0 | No |
| FortiCloud log rate | 20 /sec | 13 / 20 | 12 / 36 | 12 / 61 | No |
| FAZ-to-cloud log rate | 0 /sec | 0 / 0 | 0 / 0 | 0 / 0 | No |

## Flags (>80%)

- None. No CPU, memory, disk, session, or log-rate counter is at or above the 80% threshold.

## Notes (informational, not alerts)

- CPU is flat-lined at 0% across the full 24-hour window; the worst 24-hour spike was 19%, so the box is effectively idle.
- Memory is pinned at 38% with only a 1-point swing over 24 hours (38–39%) — no leak trend.
- Disk usage reads 0% and `log_disk_status = not_available`; the disk/FortiAnalyzer log rates are also 0, while FortiCloud logging is actively flowing (~12 msg/s average, 61 msg/s peak). Logging is FortiCloud-only — expected in this lab, worth confirming that is intentional.
- Sessions sit near 740 vs. a 24-hour peak of 1799 — normal traffic, no session-table pressure, no flood condition (setup rate peaked at 99/s in 24 h, currently 10/s).
- ~28% of sessions are NPU-offloaded (211 of 740), so forwarding is mostly in hardware.
- Read-only check: no configuration on the firewall was changed.
