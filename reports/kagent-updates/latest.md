# kagent-updates - 2026-11-06

Agent Substrate raced to **v1.0.0-rc4** with a brand-new `substrate-atelet` chart, kagent main now pins rc4 and installs atelet as its own Helm release, and **kagent v0.10.6** landed — which hard-disables Substrate on the 0.10.x charts; 20 PRs merged across kagent, substrate and website since the last digest.

Window: 2026-11-05 → 2026-11-06 (since `reports/kagent-updates/latest.md` of 2026-11-05; late-11-05 merges that missed that digest are included).

## Releases

| Repo | Tag | Date | Highlights |
| --- | --- | --- | --- |
| substrate | [v1.0.0-rc4](https://github.com/kagent-dev/substrate/releases/tag/v1.0.0-rc4) | 2026-11-05 | New `substrate-atelet` chart (atelet upgraded separately, per version-skew policy); documented in-place migration with `--take-ownership`; router timeouts exposed |
| substrate | [v1.0.0-rc3](https://github.com/kagent-dev/substrate/releases/tag/v1.0.0-rc3) | 2026-11-05 | release-1.0 upstream syncs + router timeout/parking-budget backports (no atelet chart yet) |
| substrate | [v1.0.0-rc2](https://github.com/kagent-dev/substrate/releases/tag/v1.0.0-rc2) | 2026-11-05 | Tag cut for the pin kagent v1.0.0-rc2 used; three RCs published the same day |
| kagent | [v0.10.6](https://github.com/kagent-dev/kagent/releases/tag/v0.10.6) | 2026-11-05 | Python ADK images move to Wolfi (app 18→1, app-full 156→9 HIGH/CRIT); **Substrate install blocked on 0.10.x** |

Current latest per main repo: kagent stable `v0.10.6` (0.x) / `v1.0.0-rc2` (1.x pre-GA), substrate newest `v1.0.0-rc4` with `v0.6.0` the last 0.x stable, tools `v0.3.1` (nothing new), doc2vec `v2.17.3`, kmcp `v0.4.0` (no activity; last push 2026-10-29).

## Highlights

- **omni-viper blocker for the Enterprise alpha12 / Substrate 0.6 line**: Substrate is now *unsupported* on kagent 0.10.x — the `kagent` and `kagent-crds` charts fail to install if `substrate.enabled`, `controller.substrate.enabled`, `substrateWorkerPool.create` or `SUBSTRATE_ATE_API_ENDPOINT` is set ([kagent#3539](https://github.com/kagent-dev/kagent/pull/3539), shipped in [v0.10.6](https://github.com/kagent-dev/kagent/releases/tag/v0.10.6)). Substrate demos must run the 1.x line.
- **atelet is its own Helm release now — the install/upgrade runbook changed.** New `charts/substrate-atelet` renders the same atelet, reads the same value paths, and takes the object store under `objectStore` with a credentials Secret; the built-in atelet in the `substrate` chart is deprecated ([substrate#126](https://github.com/kagent-dev/substrate/pull/126), backported in [#135](https://github.com/kagent-dev/substrate/pull/135)). Install order: `substrate-crds` → `substrate-podcert` → `substrate-atelet --wait` → `substrate` with `atelet.enabled=false`.
- **kagent installer follows suit and pins rc4**: installs `substrate-atelet` first, waits for the full DaemonSet rollout (Helm `--wait` tolerates `maxUnavailable` pods — zero ready atelets on a one-node cluster), adds `install.atelet` overrides, and migrates existing installs in place on Helm 3.17+ with `helm.sh/resource-policy=keep` + `--take-ownership` ([kagent#3548](https://github.com/kagent-dev/kagent/pull/3548)).
- **Core MCP server became a full session API**: `create_session`, `get_session`, `list_tasks`, `get_task`, `delete_session`, `rename_session`, `get_session_checkpoint`, `delete_session_checkpoint`, plus `agent`/`all_states` filters on `list_sessions` — an MCP client can now start a Session, poll turns and read back tool calls ([kagent#3339](https://github.com/kagent-dev/kagent/pull/3339)). Note a response shape change: `agent` is now `namespace/name` in `list_sessions`/`fork_session` output.
- **MCP interop fix worth demoing**: `invoke_agent`'s top-level `oneOf` was dropped so Gemini accepts the tool schema ([kagent#3534](https://github.com/kagent-dev/kagent/pull/3534)) — previously the tool failed schema validation on that provider.
- **Session lifecycle hardening**: normal deletion errors while turns are active, with an explicit force option in API/CLI/UI, actors deleted directly so an interrupted `RESUMING` resume can't block deletion ([kagent#3530](https://github.com/kagent-dev/kagent/pull/3530)); cancelled tasks are now finalized instead of left dangling ([kagent#3541](https://github.com/kagent-dev/kagent/pull/3541)).
- **EgressPolicy gets a UI** — new Policies page (search, namespace filter, "Used by" table), create/edit form validated against CRD rules, agent form picks namespace policies, `EgressPolicies=false` no longer renders the agent as failed, management role gains `egresspolicies` write ([kagent#3542](https://github.com/kagent-dev/kagent/pull/3542)). Good story for network-policy demos.
- **Security**: v0.10.6 moves Python ADK runtime images to `chainguard/wolfi-base`, clearing every unfixed Debian 12 finding (`libssl3`, `libc6`, curl, krb5, perl, zlib): `app` 18→1, `app-full` 156→9, remainder unpatched upstream ([kagent#3539](https://github.com/kagent-dev/kagent/pull/3539)).
- **Router tuning knobs** on both branches for timeouts (60s defaults) so long model turns stop hitting the old implicit limits ([substrate#132](https://github.com/kagent-dev/substrate/pull/132), backport [#134](https://github.com/kagent-dev/substrate/pull/134)).
- **Model catalogue**: Bedrock Grok inference profiles are now suggested ([kagent#3523](https://github.com/kagent-dev/kagent/pull/3523)); `kagent uninstall` asks before deleting self-managed WorkerPools ([kagent#3511](https://github.com/kagent-dev/kagent/pull/3511)).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3548](https://github.com/kagent-dev/kagent/pull/3548) | chore(deps): upgrade Substrate to rc4 and install Atelet separately | EItanya | 2026-11-06 |
| substrate | [#126](https://github.com/kagent-dev/substrate/pull/126) | Add a substrate-atelet chart for upgrading atelet separately | iplay88keys | 2026-11-05 |
| substrate | [#135](https://github.com/kagent-dev/substrate/pull/135) | [backport release-1.0] substrate-atelet chart | iplay88keys | 2026-11-05 |
| kagent | [#3339](https://github.com/kagent-dev/kagent/pull/3339) | feat(mcp): session lifecycle and read-back tools in the core MCP server | QuentinBisson | 2026-11-05 |
| kagent | [#3539](https://github.com/kagent-dev/kagent/pull/3539) | fix: move Python ADK images to Wolfi, disable Substrate on release/v0.10.x | iplay88keys | 2026-11-05 |
| kagent | [#3530](https://github.com/kagent-dev/kagent/pull/3530) | session/lifecycle: support force deletion without suspension | shashankram | 2026-11-05 |
| kagent | [#3542](https://github.com/kagent-dev/kagent/pull/3542) | feat(ui): manage EgressPolicies from the UI | alan-bentley-pgh | 2026-11-05 |
| kagent | [#3534](https://github.com/kagent-dev/kagent/pull/3534) | fix(mcp): drop invoke_agent's top-level oneOf so Gemini accepts it | jotacamou | 2026-11-05 |
| kagent | [#3541](https://github.com/kagent-dev/kagent/pull/3541) | fix(taskstore): finalize failed tasks after execution cancellation | EItanya | 2026-11-05 |
| kagent | [#3523](https://github.com/kagent-dev/kagent/pull/3523) | feat(models): suggest Bedrock Grok inference profiles | jayunit100 | 2026-11-05 |
| kagent | [#3511](https://github.com/kagent-dev/kagent/pull/3511) | fix(cli): confirm before deleting self-managed WorkerPools on uninstall | iplay88keys | 2026-11-05 |
| substrate | [#132](https://github.com/kagent-dev/substrate/pull/132) | charts/substrate: expose router timeouts with 60s defaults | shashankram | 2026-11-05 |
| substrate | [#134](https://github.com/kagent-dev/substrate/pull/134) | [backport] charts/substrate: expose router timeouts | shashankram | 2026-11-05 |
| substrate | [#128](https://github.com/kagent-dev/substrate/pull/128) | Sync upstream main through 17fa8586e | EItanya | 2026-11-05 |
| substrate | [#133](https://github.com/kagent-dev/substrate/pull/133) | Sync release-1.0 with upstream | EItanya | 2026-11-05 |
| substrate | [#136](https://github.com/kagent-dev/substrate/pull/136) | Sync release-1.0 copyright and documentation updates | EItanya | 2026-11-06 |
| website | [#693](https://github.com/kagent-dev/website/pull/693) | Docs for rc2 | Rachael-Graham | 2026-11-05 |
| website | [#694](https://github.com/kagent-dev/website/pull/694) | Update kagent 0.x reference docs at v0.10.5 | github-actions[bot] | 2026-11-05 |
| website | [#695](https://github.com/kagent-dev/website/pull/695) | [Automated] Refresh kagent UI screenshots (mock-backed) | github-actions[bot] | 2026-11-05 |
| website | [#696](https://github.com/kagent-dev/website/pull/696) | Shorten generated CLI page intro | Rachael-Graham | 2026-11-05 |

No merged PRs in window for tools, doc2vec, kmcp, khook, community, mockllm, byo-ext.

## Watch list

- **Pin alignment for the alpha12 cut**: kagent main now requires Substrate `v1.0.0-rc4` + the `substrate-atelet` chart ([kagent#3548](https://github.com/kagent-dev/kagent/pull/3548), [substrate#126](https://github.com/kagent-dev/substrate/pull/126)) while the released kagent 1.x is still `v1.0.0-rc2` — validate rc4 + split-atelet on omni-viper *before* cutting alpha12, and expect a `v1.0.0-rc3`/GA kagent tag shortly.
- **Migration hazard**: moving atelet out of the `substrate` release *requires* annotating `helm.sh/resource-policy=keep` first, otherwise the next `helm upgrade substrate` deletes the atelet DaemonSet; needs Helm 3.17+ for `--take-ownership` ([substrate#135](https://github.com/kagent-dev/substrate/pull/135) "Existing installs", [substrate rc4 notes](https://github.com/kagent-dev/substrate/releases/tag/v1.0.0-rc4)). Rehearse this on omni-viper.
- **Deprecated**: atelet bundled in the `substrate` chart is deprecated and will be removed — plan demos/scripts around `substrate-atelet` + `install.atelet` overrides ([substrate#126](https://github.com/kagent-dev/substrate/pull/126), [kagent#3548](https://github.com/kagent-dev/kagent/pull/3548)).
- **Version-skew order is now enforced by charting**: podcert → atelet (`--wait`) → WorkerPool `workerImage` → CRDs → substrate; roll back in reverse, and only roll back `substrate-atelet` after every worker is back on its old image ([substrate#135](https://github.com/kagent-dev/substrate/pull/135)).
- **Break to announce**: any 0.10.x deployment that sets a Substrate value now fails install — 0.x + Substrate is unsupported, upgrade to 1.x ([kagent#3539](https://github.com/kagent-dev/kagent/pull/3539)).
- **MCP output break**: `agent` in `list_sessions`/`fork_session` responses becomes `namespace/name`; MCP tool annotations (`readOnlyHint`/`destructiveHint`) are now set — check any client that parses these ([kagent#3339](https://github.com/kagent-dev/kagent/pull/3339)).
- **arm64 gVisor caveat persists**: Wolfi's arm64 glibc/libstdc++ use pointer auth, gVisor doesn't preserve PAC keys across checkpoint/restore (google/gvisor#13542), so restored actors SIGILL — relevant for arm64 Substrate demos ([kagent#3539](https://github.com/kagent-dev/kagent/pull/3539)).
