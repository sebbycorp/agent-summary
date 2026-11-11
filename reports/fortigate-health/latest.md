# fortigate-health - 2026-11-11

**Verdict: HEALTHY** — all monitored resources are well below the 80% alerting threshold; nothing to flag.

- Device: fw-maniak-hq · FortiGate 80F (FGT80FTK22061709) · FortiOS v7.4.11 build 2878 · VDOM root
- Sample time: 2026-11-11 09:00:05 EST (14:00:05 UTC), taken from the latest `fg_resource_usage` datapoint
- Source: read-only calls to `fg_system_status` and `fg_resource_usage` (no changes made)

## Resource usage

| Metric | Current | 1-min avg | 24-h max / avg | >80%? |
|---|---|---|---|---|
| CPU (all cores) | 0 % | 0 % (max 0) | 22 % / 0 % | No |
| Memory (FortiOS %used) | 38 % | 38 % | 39 % / 38 % | No |
| Memory (absolute) | 1.39 GiB used / 3.63 GiB total | — | — | No |
| Memory free / freeable | 1.89 GiB free, 360 MiB freeable | — | — | No |
| Disk (log disk usage) | 0 % reported (24-h max 0) | 0 % | 0 % / 0 % | No — see note |
| Sessions (IPv4) | 464 | 469 | 1358 / 690 | No |
| Sessions (IPv6) | 10 | 16 | 20 / 10 | No |
| Session setup rate (IPv4) | 1 /s | 3 /s (peak 8) | 60 /s peak, 2 /s avg | No |
| Session setup rate (IPv6) | 0 /s | 0 /s | 8 /s peak, 0 /s avg | No |
| NPU-offloaded sessions | 134 (v6: 0) | 131 | 805 peak / 209 avg | No |
| NTurbo sessions | 26 (v6: 0) | 26 | 84 peak / 27 avg | No |
| Disk log rate | 0 msg/s | 0 | 0 / 0 | No |
| FortiAnalyzer log rate | 0 msg/s | 0 | 0 / 0 | No |
| FortiCloud log rate | 16 msg/s | 9 (peak 16) | 49 peak / 10 avg | No |
| FortiAnalyzer→Cloud log rate | 0 msg/s | 0 | 0 / 0 | No |

## CPU core detail

All 8 cores report 100 % idle (user 0 / system 0 / nice 0 / iowait 0), consistent with the 0 % aggregate CPU.

## Flags and notes

- **Nothing above 80%.** Peak values in the last 24 h are CPU 22 %, memory 39 %, sessions 1358, setup rate 60 /s — comfortable headroom for an 80F.
- **Disk utilization is not assessable.** `fg_system_status` reports `log_disk_status: not_available` and the disk counter series is flat 0, which means disk usage is not being reported (no local log disk), not a genuine 0 % measurement. FortiCloud logging is active (`forticloud_lograte` ~9-16 msg/s, 24-h avg 10 msg/s), so logs are going off-box. Worth confirming this is the intended log destination.
- Session mix is largely hardware-offloaded (134 of 464 in NPU, 26 in NTurbo), which explains the near-zero CPU.
- Session count is trending down slightly versus the 24-h average (464 now vs 690 avg) — normal idle-period behaviour, no action needed.

## Actions taken

None. This check was read-only; no configuration was created, modified, or disabled on the firewall.
