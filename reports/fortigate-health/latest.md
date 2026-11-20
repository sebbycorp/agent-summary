# fortigate-health - 2026-11-20

**Verdict: HEALTHY** — every resource counter is far below the 80% flag line; CPU idle, memory at ~38%, sessions and setup rate well within 80F capacity.

- Device: `fw-maniak-hq` — FortiGate 80F (`FGT80FTK22061709`), FortiOS **v7.4.11 build 2878**, VDOM `root`
- Sample time: **2026-11-20 14:00:05 UTC** (09:00 America/Toronto)
- Source (read-only): `fg_resource_usage`, `fg_system_status`, plus `fg_log_state` for disk/logging context

## Resource usage — right now

| Metric | Current | Peak (24 h) | Flag > 80% | Status |
|---|---|---|---|---|
| CPU (all 8 cores) | **0 %** (user 0 / system 0 / idle 100) | 19 % | 80 % | OK |
| Memory | **38 %** — 1.42 GiB used / 3.63 GiB total | 40 % | 80 % | OK |
| Disk | **0 %** — no local log disk (`log_disk_status: not_available`) | 0 % | 80 % | OK (see notes) |
| Sessions (IPv4) | **1,034** | 3,451 | n/a | OK |
| Sessions (IPv6) | **20** | 21 | n/a | OK |
| Session setup rate (IPv4) | **4 /s** | 147 /s | n/a | OK |
| Session setup rate (IPv6) | **0 /s** | 7 /s | n/a | OK |
| NPU-offloaded sessions | **433** (IPv6: 0) | 2,652 | n/a | OK |
| nturbo sessions | **36** (IPv6: 0) | 84 | n/a | OK |
| Log rate — FortiCloud | **13 /s** | 98 /s | n/a | OK |
| Log rate — local disk / FortiAnalyzer | **0 /s** | 0 /s | n/a | OK |

Memory detail (from `fg_system_status`): total 3,897,774,080 B (3.63 GiB), used 1,519,771,648 B (1.42 GiB), free 1,993,633,792 B (1.86 GiB), freeable 384,368,640 B (367 MiB).

## Counters returned by the tool

`cpu`, `mem`, `disk`, `session`, `session6`, `setuprate`, `setuprate6`, `npu_session`, `npu_session6`, `nturbo_session`, `nturbo_session6`, `disk_lograte`, `faz_lograte`, `forticloud_lograte`, `faz_cloud_lograte` — current values shown above; 1-min / 10-min / 30-min / 1-hour / 12-hour / 24-hour history also returned.

Short-term trend (1-min window): CPU max 0 %, memory steady at 38 %, sessions 872–1,034 (avg 949), setup rate 0–23 /s (avg 8), FortiCloud log rate 8–25 /s (avg 15). No spikes in the trailing hour (CPU peak 11 %, session peak 1,915, setup-rate peak 44 /s).

## Flags (anything above 80%)

- **None.** No counter — current or 24-hour peak — approached the 80% threshold.

## Notes

- **Disk 0 % is expected, not a clean bill of health on storage:** the 80F has no local log disk enabled (`is_available: false`, `disabled_by_default: true`) and FortiAnalyzer is disabled. Logging is going to **FortiCloud** (enabled; reported quota 500,000,000 B, used 0 at sample time). Watch FortiCloud quota/retention rather than disk %.
- All sessions except ~433 NPU-offloaded and ~36 nturbo-accelerated flows are CPU-path; at 1,034 sessions and 4 setups/s there is no offload or control-plane pressure.
- IPv6 is a very small share of the table (20 of 1,054 sessions) and is fully idle on the NPU/nturbo paths.
- Read-only check: no configuration was changed on the firewall.
