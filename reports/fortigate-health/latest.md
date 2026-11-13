# fortigate-health - 2026-11-13

**Verdict: HEALTHY** — all sampled counters are well under the 80% threshold; nothing to flag.

- **Device:** fw-maniak-hq — FortiGate 80F (FGT80F), serial FGT80FTK22061709
- **FortiOS:** v7.4.11 build 2878, VDOM `root`
- **Sample time:** 2026-11-13 09:00:06 EST (epoch 1794578406, from the monitor API sample window)
- **Source:** read-only `fg_system_status` + `fg_resource_usage` (no changes made to the firewall)

## Resource usage

| Metric | Now | 1-min avg / max | 1-hour max | 24-hour avg / max | Threshold 80% | Verdict |
|---|---|---|---|---|---|---|
| CPU usage (aggregate) | 0% (user 0 / system 0 / idle 100) | 0 / 0% | 6% | 0 / 17% | OK | Pass |
| CPU – core 0 (busiest) | 1% user, 99% idle | — | — | — | OK | Pass |
| CPU – cores 1-7 | 0% (100% idle) | — | — | — | OK | Pass |
| Memory usage (%) | 38% | 38 / 38% | 39% | 38 / 39% | OK | Pass |
| Memory used / total | 1,510,789,120 B / 3,897,774,080 B (1.41 GiB / 3.63 GiB) | — | — | — | — | Pass |
| Memory free / freeable | 2,007,597,056 B / 379,387,904 B (1.87 GiB / 0.35 GiB) | — | — | — | — | Pass |
| Disk usage (%) | 0% | 0 / 0% | 0% | 0 / 0% | OK | Pass |
| Sessions (IPv4) | 733 | 725 / 743 | 2,537 | 566 / 2,537 | OK | Pass |
| Sessions (IPv6) | 11 | 13 / 19 | 20 | 9 / 20 | OK | Pass |
| Session setup rate (IPv4) | 0 /sec | 4 / 12 | 115 | 1 / 138 | OK | Pass |
| Session setup rate (IPv6) | 0 /sec | 0 / 0 | 0 | 0 / 6 | OK | Pass |
| NPU-offloaded sessions | 349 (v4) / 0 (v6) | 354 / 361 | 1,078 | 201 / 1,078 | OK | Pass |
| nt:turbo offloaded sessions | 28 (v4) / 0 (v6) | 26 / 29 | 59 | 25 / 67 | OK | Pass |
| Log rate – local disk | 0 /sec | 0 | 0 | 0 | OK | Pass |
| Log rate – local FAZ | 0 /sec | 0 | 0 | 0 | OK | Pass |
| Log rate – FortiCloud | 9 /sec | 12 / 17 | 50 | 8 / 91 | OK | Pass |
| Log rate – FAZ-to-cloud | 0 /sec | 0 | 0 | 0 | OK | Pass |

## Above-threshold flags (>80%)

**None.** No counter returned by `fg_resource_usage` reached 80% at sample time or anywhere in the 24-hour history:

- CPU: 0% now, 17% peak over 24 h
- Memory: 38% now, 39% peak over 24 h
- Disk: 0% throughout
- Sessions / setup rate: no capacity limit is returned by the resource-usage API, so these are reported as raw counts only — the 24 h session peak (2,537) and setup-rate peak (138 /sec) are low for an 80F.

## Notes

- Memory is reported two ways: the monitor API's own `mem.current` is **38%**, while a `used + freeable` / total calculation gives **48.5%** (0.35 GiB is reclaimable page cache). Either figure is comfortably healthy.
- `log_disk_status` is **not_available** and both local disk and FAZ log rates are 0 — logging is going to **FortiCloud** (avg ~8-13 events/sec, 24 h peak 91/sec), so the 0% disk counter reflects no local log disk rather than an idle disk.
- Traffic profile: IPv4-heavy with offload working well (~48% of IPv4 sessions in the NPU, 349 of 733), plus a small steady IPv6 presence (~11 sessions).
- This check was read-only. No configuration, policy, address, or interface changes were made.
