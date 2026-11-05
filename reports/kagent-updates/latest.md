# kagent-updates - 2026-11-05

kagent **v1.0.0-rc2** and **v0.10.5** shipped, Agent Substrate **v1.0.0-rc1** is the new pin (mTLS actor ingress), tools **0.3.1** cleared 148 CVE findings; ~76 merged PRs across kagent, substrate, tools, doc2vec and website in the last 7 days.

Window: 2026-10-29 → 2026-11-05 (no prior `latest.md`, so the default 7-day window was used).

## Releases

| Repo | Tag | Date | Highlights |
| --- | --- | --- | --- |
| kagent | [v1.0.0-rc2](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc2) | 2026-11-04 | Second RC; pins Agent Substrate v1.0.0-rc2, CVE-fixed runtime images, narrowed controller RBAC |
| kagent | [v0.10.5](https://github.com/kagent-dev/kagent/releases/tag/v0.10.5) | 2026-11-04 | 0.x maintenance line: HIGH/CRITICAL CVE fixes in the six runtime images, kagent-tools 0.3.1 |
| kagent | [v1.0.0-rc1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc1) | 2026-10-30 | First RC: pre-RC API breaks, Substrate rc1 mTLS router, `kagent install --dry-run` Helm export |
| substrate | [v1.0.0-rc1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.0-rc1) | 2026-10-30 | 1.x RC line; requires mTLS for actor ingress (A2A + sandbox guest calls) |
| substrate | [v0.7.0-alpha3](https://github.com/kagent-dev/substrate/releases/tag/v0.7.0-alpha3) | 2026-10-30 | 0.7 alpha for the 0.x/Enterprise line (image-cache v3, AgentGateway v1.7.0) |
| tools | [v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1) | 2026-11-04 | Bundled CLI + Go dep bumps; tool-server 148 → 38 scan findings |
| doc2vec | [v2.17.1](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.1) / [2.17.2](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.2) / [2.17.3](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.3) | 2026-11-02 | MCP image CVE sweep, runs as non-root UID 14000, Docker build unbroken |

Current latest stable per main repo: kagent stable line `v0.10.5`, 1.x pre-GA at `v1.0.0-rc2`; substrate `v0.6.0` stable with `v1.0.0-rc1` as the newest tag; kmcp still `v0.4.0` (nothing new in window).

## Highlights

- **Substrate 1.x RC changes the network path — test on omni-viper.** Actor ingress now requires mTLS: `config.substrate.routerUrl` moves to `https://atenet-router.ate-system.svc:443` and direct Helm installs must set `atenetRouter.ingressAllowedSPIFFEIDs`, or A2A and sandbox guest calls are denied ([kagent#3457](https://github.com/kagent-dev/kagent/pull/3457), [substrate#104](https://github.com/kagent-dev/substrate/pull/104) — plaintext router listeners removed, empty allowlist denies ingress).
- **Pre-RC API breaks you cannot get later**: `Session.agent` becomes `AgentProvenance{ref, uid}`, obsolete proto reservations removed, `scheduled-run/` message-ID prefix reserved ([kagent#3453](https://github.com/kagent-dev/kagent/pull/3453)); `Harness.status` plus ADK `network`/`share_tools` deleted ([kagent#3454](https://github.com/kagent-dev/kagent/pull/3454)).
- **Breaking CRD change to ModelProviderConfig**: separate `discoveryAuth.secretRef` and `modelConfigTemplate.auth`; model inventory/timestamps/secret hashes dropped from status, cached model reads now return NotFound ([kagent#3494](https://github.com/kagent-dev/kagent/pull/3494)).
- **Security posture is the story of the week**: v0.10.5 runtime images cut `app` 69→19 and `golang-adk` 6→0 findings ([kagent#3522](https://github.com/kagent-dev/kagent/pull/3522)); tools 0.3.1 drops 148→38 ([tools#99](https://github.com/kagent-dev/tools/pull/99)); UI nginx base patched for critical brotli CVE-2026-111497 ([kagent#3489](https://github.com/kagent-dev/kagent/pull/3489)); controller RBAC wildcard replaced with scoped Roles ([kagent#3378](https://github.com/kagent-dev/kagent/pull/3378)).
- **Demo-able reliability fixes**: failed golden builds for Agents/SandboxTemplates now auto-retry instead of needing a reapply ([kagent#3335](https://github.com/kagent-dev/kagent/pull/3335)); Go runtime TaskStore saves survive a controller restart (37 retried attempts vs. the old 4-in-713 ms) ([kagent#3391](https://github.com/kagent-dev/kagent/pull/3391)).
- **MCP surface grew**: `list_agents` + `invoke_agent` let an MCP client discover and start an Agent without pre-creating a Session ([kagent#3486](https://github.com/kagent-dev/kagent/pull/3486)); embedders can extend the core `/mcp` server via `app.Options.MCPExtensions` ([kagent#3495](https://github.com/kagent-dev/kagent/pull/3495)).
- **Installer/GitOps friendliness**: `kagent install --dry-run` exports a reviewable Helm recipe (`install.sh` + values), on top of a new reusable install library ([kagent#3432](https://github.com/kagent-dev/kagent/pull/3432), [#3427](https://github.com/kagent-dev/kagent/pull/3427)); model API keys no longer appear in process args ([kagent#3446](https://github.com/kagent-dev/kagent/pull/3446)).
- **New Helm knobs on the Substrate side**: atelet `nodeSelector` for dedicated worker nodes ([substrate#102](https://github.com/kagent-dev/substrate/pull/102)), router request-parking budget ([substrate#116](https://github.com/kagent-dev/substrate/pull/116)), egress proxy log level ([substrate#119](https://github.com/kagent-dev/substrate/pull/119)).
- **Model provider coverage**: Gemini custom `baseUrl` + Actor JWT auth ([kagent#3458](https://github.com/kagent-dev/kagent/pull/3458)), Bedrock runtime origin allowed in Harness egress ([kagent#3438](https://github.com/kagent-dev/kagent/pull/3438)), current Foundry Grok IDs incl. `grok-4.7` ([kagent#3513](https://github.com/kagent-dev/kagent/pull/3513)).
- **Caveat for upgrade demos**: Substrate 0.7.x squashes its PostgreSQL migrations and moves the node-local image cache from v2 to v3 — databases must be recreated and actors drained per the Helm upgrade notes ([kagent#3444](https://github.com/kagent-dev/kagent/pull/3444), [substrate#108](https://github.com/kagent-dev/substrate/pull/108)).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3457](https://github.com/kagent-dev/kagent/pull/3457) | chore: upgrade Substrate to v1.0.0-rc1 (mTLS actor ingress) | EItanya | 2026-10-30 |
| kagent | [#3453](https://github.com/kagent-dev/kagent/pull/3453) | refactor(api)!: pre-RC API breaks for machine principals | EItanya | 2026-10-30 |
| kagent | [#3494](https://github.com/kagent-dev/kagent/pull/3494) | api/models: ModelProviderConfig discovery contract (breaking) | shashankram | 2026-11-02 |
| kagent | [#3378](https://github.com/kagent-dev/kagent/pull/3378) | fix(helm): narrow and separate controller RBAC permissions | EItanya | 2026-11-02 |
| kagent | [#3522](https://github.com/kagent-dev/kagent/pull/3522) | fix(deps): resolve CVEs in v0.10.x runtime images | iplay88keys | 2026-11-04 |
| kagent | [#3335](https://github.com/kagent-dev/kagent/pull/3335) | feat(controller): retry failed golden builds | danielle-mustillo | 2026-10-29 |
| kagent | [#3391](https://github.com/kagent-dev/kagent/pull/3391) | fix(adk): retry TaskStore calls through a controller restart | adalundhe | 2026-11-02 |
| kagent | [#3486](https://github.com/kagent-dev/kagent/pull/3486) | feat(mcp): discover and invoke agents | EItanya | 2026-11-02 |
| kagent | [#3432](https://github.com/kagent-dev/kagent/pull/3432) | feat(cli): export Helm installation with dry-run | iplay88keys | 2026-10-29 |
| kagent | [#3458](https://github.com/kagent-dev/kagent/pull/3458) | adk/models: Gemini endpoint overrides + Actor JWT auth | shashankram | 2026-10-30 |
| kagent | [#3446](https://github.com/kagent-dev/kagent/pull/3446) | fix(install): keep model credentials out of process args | iplay88keys | 2026-10-30 |
| kagent | [#3520](https://github.com/kagent-dev/kagent/pull/3520) | chore(deps): upgrade Substrate to v1.0.0-rc2 | EItanya | 2026-11-04 |
| kagent | [#2947](https://github.com/kagent-dev/kagent/pull/2947) | fix(adk): tell the model why a person rejected a tool call | fiunchinho | 2026-11-03 |
| substrate | [#104](https://github.com/kagent-dev/substrate/pull/104) | Sync upstream runtime, AgentGateway, docs (breaking: no plaintext router) | EItanya | 2026-10-29 |
| substrate | [#106](https://github.com/kagent-dev/substrate/pull/106) | Maintain fork release branches and unify publishing | EItanya | 2026-10-30 |
| substrate | [#102](https://github.com/kagent-dev/substrate/pull/102) | feat(helm): nodeSelector support for atelet daemonset | alexnovak | 2026-11-04 |
| substrate | [#116](https://github.com/kagent-dev/substrate/pull/116) | feat(helm): configure the router request parking budget | renato0307 | 2026-11-03 |
| substrate | [#119](https://github.com/kagent-dev/substrate/pull/119) | feat(helm): configure the egress proxy log level | TarasLykhenko | 2026-11-04 |
| tools | [#99](https://github.com/kagent-dev/tools/pull/99) | fix(deps): bump bundled CLIs and Go deps to resolve CVEs | iplay88keys | 2026-11-04 |
| tools | [#91](https://github.com/kagent-dev/tools/pull/91) | feat: ship kagent 1.x skills bundle in the tools OCI image | dimetron | 2026-10-29 |
| doc2vec | [#129](https://github.com/kagent-dev/doc2vec/pull/129) | fix(mcp): clear the MCP image's dependency CVEs (8 crit → 0) | Charlesthebird | 2026-11-02 |
| doc2vec | [#94](https://github.com/kagent-dev/doc2vec/pull/94) | mcp: run image as numeric non-root user (14000) | SarthakB11 | 2026-11-02 |
| doc2vec | [#136](https://github.com/kagent-dev/doc2vec/pull/136) | fix(deps): pin @chonkiejs/core to unbreak the Docker build | djannot | 2026-11-02 |
| website | [#652](https://github.com/kagent-dev/website/pull/652) | docs: update kagent 1.x docs for v1.0.0-rc1 | Rachael-Graham | 2026-11-02 |
| website | [#682](https://github.com/kagent-dev/website/pull/682) | Helm install and upgrade guides | Rachael-Graham | 2026-11-04 |

+51 more (mostly dependabot and automated reference-doc runs): [merged PR search](https://github.com/pulls?q=is%3Apr+org%3Akagent-dev+is%3Amerged+merged%3A%3E%3D2026-10-29)

## Watch list

- **kagent v1.0.0-rc2 / substrate v1.0.0-rc1** are the RC pair to validate on omni-viper before an Enterprise alpha12 cut; kagent now pins Substrate rc2 ([#3520](https://github.com/kagent-dev/kagent/pull/3520)). Watch for a GA `v1.0.0`.
- **`atenetRouter.ingressAllowedSPIFFEIDs`** — any direct Helm install (not `kagent install`) that skips this loses A2A and sandbox guest traffic ([#3457](https://github.com/kagent-dev/kagent/pull/3457), [website#652](https://github.com/kagent-dev/website/pull/652)).
- **ModelProviderConfig discovery contract** ([#3494](https://github.com/kagent-dev/kagent/pull/3494)) — CRD/validation change plus status pruning; re-check any demo that reads discovered models from status.
- **Protobuf compatibility gate is now strict** ([#3455](https://github.com/kagent-dev/kagent/pull/3455)) and pre-RC breaks were taken with Buf exceptions — expect further API churn until `v1.0.0` freezes.
- **Substrate DB reseed + image-cache v3** required on 0.6 → 0.7/1.x upgrades ([#3444](https://github.com/kagent-dev/kagent/pull/3444), [substrate#108](https://github.com/kagent-dev/substrate/pull/108)); plan downtime for demos.
- **gVisor arm64 blocker**: Debian 13 base cannot be used because its arm64 glibc pointer-auth breaks gVisor checkpoint/restore (google/gvisor#13542), so `app` keeps unfixed Debian 12 packages ([kagent#3522](https://github.com/kagent-dev/kagent/pull/3522)).
- **Image scans moved from per-push to daily** ([#3496](https://github.com/kagent-dev/kagent/pull/3496)) and `release/**` loses per-push scanning — new CVEs surface more slowly now.
- **Deprecation to track**: environment variables are no longer a supported interface — `docs/env.md`, `kagent env` and the env-docs generator were removed ([#3434](https://github.com/kagent-dev/kagent/pull/3434), [website#674](https://github.com/kagent-dev/website/pull/674)); configure via Helm/CLI/CRDs only.
- **kagent-tools 0.3.1** is now the chart default on the 0.x line; the 1.x tools skills bundle image still exceeds the 100 MiB per-package limit, so only the skills-only root loads today ([tools#91](https://github.com/kagent-dev/tools/pull/91)).
