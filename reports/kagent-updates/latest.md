# kagent-updates - 2026-11-10

**kagent v1.0.0 GA shipped — first stable release, with the new `curl | bash` → `kagent install` onboarding and default Harnesses; substrate landed declarative install/upgrade values (`#165`, backported in `#167`); doc2vec v2.17.4 published. ~31 PRs merged across 4 repos.**

Window: 2026-11-09 → 2026-11-10. Not repeated from the 2026-11-09 digest: substrate `#153`/`#154`/`#157`/`#159`/`#162`/`#163`, tools `#98`, website `#691`/`#707`, kagent `#3340`.

## Releases

| Repo | Tag | Date | Highlights |
| --- | --- | --- | --- |
| kagent | [v1.0.0](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0) | 2026-11-09 | **First GA.** v1 API (Harness / AgentTemplate / Agent / Session / SandboxTemplate), Substrate-backed runtimes, Claude/Codex/BYO harnesses, checkpoints, egress policies, auth ([compare v0.9.10...v1.0.0](https://github.com/kagent-dev/kagent/compare/v0.9.10...v1.0.0)) |
| kagent | [v1.0.0-rc4](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc4) | 2026-11-09 | last RC before GA, hours earlier |
| doc2vec | [v2.17.4](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.4) | 2026-11-10 | survives browserless restarts, skips oversized pages pre-render |
| substrate | [v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) (unchanged) | 2026-11-08 | no new tag; `release-1.0` is now ahead of it via `#167` — a `v1.0.2` is queued |
| tools | [v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1) (unchanged) | 2026-11-04 | — |
| kmcp | [v0.4.0](https://github.com/kagent-dev/kmcp/releases/tag/v0.4.0) (unchanged) | 2026-09-17 | no activity in window |

## Highlights

- **kagent 1.0 is the demo headline.** GA pins Substrate **`v1.0.0`** — `go/go.mod` on `main` still reads `replace go.ate.dev/substrate => github.com/kagent-dev/substrate v1.0.0`. For omni-viper: the "Agent Substrate 0.6" line is *not* what kagent 1.0 tracks; 1.0 pairs with substrate 1.0.x. Expect an Enterprise alpha on `v1.0.0` + substrate `v1.0.1`/`v1.0.2`.
- **Install is now two lines** — `export OPENAI_API_KEY=...` then `curl -sL https://kagent.dev/install | bash && kagent install` ([#3566](https://github.com/kagent-dev/kagent/pull/3566)). Ships default Harnesses `kagent-default` / `codex-default` / `claude-default`, `--provider` (no CLI config file), enables gRPC reflection, prints next steps. Biggest pre-sales change in this window.
- **Breaking for existing Harnesses:** `workload.command`/`args` are now **rejected** on Claude and Codex harnesses (previously silently dropped, so agents reported Ready while running the image entrypoint); empty lists rejected everywhere. Existing objects fail validation on their *next* update ([#3616](https://github.com/kagent-dev/kagent/pull/3616)).
- **Delegated identity on MCP calls fixed:** MCP/HTTP context wrapping hid the native ADK session ID, so STS cache lookups missed and outbound requests lost delegated identity ([#3605](https://github.com/kagent-dev/kagent/pull/3605), backport [#3612](https://github.com/kagent-dev/kagent/pull/3612)). Go ADK now honours `runtimeDefaults.telemetry.captureContent` — model spans finally carry prompts/replies ([#3592](https://github.com/kagent-dev/kagent/pull/3592), backport [#3611](https://github.com/kagent-dev/kagent/pull/3611)). Both tagged `cherrpick-1.0`.
- **Substrate install is declarative now:** `ResetThenReuseValues` dropped, so values come from chart + config only — stale values get removed and switching `postgres`/`objectStore` mode on upgrade works. But `install` refuses once the `substrate` release exists, and **`upgrade` requires the same `-c` config used at install** ([#165](https://github.com/kagent-dev/substrate/pull/165)); all three installer PRs reached `release-1.0` via [#167](https://github.com/kagent-dev/substrate/pull/167).
- **v1.0.x got 7 cherry-picks** ([#3586](https://github.com/kagent-dev/kagent/pull/3586)) including `RevertSession` for crashed runtimes (#3395, customer-driven) and deferring MCP tool discovery so slow discovery no longer fails agent resumes (#3560).
- **Docs flipped to 1.0:** `latest` now points at 1.0.x ([#703](https://github.com/kagent-dev/website/pull/703)), reference docs regenerated at `v1.0.0` with new claude/codex/golang-adk image digests ([#715](https://github.com/kagent-dev/website/pull/715)), install docs simplified ([#705](https://github.com/kagent-dev/website/pull/705)), Anthropic self-hosted sandboxes published ([#706](https://github.com/kagent-dev/website/pull/706)), plus the kagent 1.0 launch post ([#714](https://github.com/kagent-dev/website/pull/714)).
- **Python dep drift to watch:** google-adk allowed up to 2.13.0, which **defaults telemetry to OTel GenAI v2 and removes the `call_llm` span** (opt out via `ADK_TELEMETRY_SCHEMA_VERSION_OPT_IN=otel_semconv_1_36`) ([#3581](https://github.com/kagent-dev/kagent/pull/3581)). Dashboards on the old schema will go quiet.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3566](https://github.com/kagent-dev/kagent/pull/3566) | feat: improve install and first-agent onboarding | EItanya | 2026-11-09 |
| kagent | [#3616](https://github.com/kagent-dev/kagent/pull/3616) | fix(api): reject unsupported Harness workload overrides | timflannagan | 2026-11-09 |
| kagent | [#3605](https://github.com/kagent-dev/kagent/pull/3605) | adk/sts: preserve session identity on outbound MCP requests | shashankram | 2026-11-09 |
| kagent | [#3592](https://github.com/kagent-dev/kagent/pull/3592) | adk/telemetry: honor runtime content capture settings | shashankram | 2026-11-09 |
| kagent | [#3586](https://github.com/kagent-dev/kagent/pull/3586) | fix: backport pre-release fixes to v1.0.x (7 PRs) | EItanya | 2026-11-09 |
| kagent | [#3611](https://github.com/kagent-dev/kagent/pull/3611) / [#3612](https://github.com/kagent-dev/kagent/pull/3612) | [backport] telemetry capture + STS session identity | shashankram | 2026-11-09 |
| kagent | [#3574](https://github.com/kagent-dev/kagent/pull/3574) | fix(adk): default overlapSize to 0 when compactionInterval set | subhaballe | 2026-11-09 |
| kagent | [#3581](https://github.com/kagent-dev/kagent/pull/3581) | deps: google-adk >=2.13.0 (breaking OTel semconv default) | dependabot | 2026-11-09 |
| kagent | [#3606](https://github.com/kagent-dev/kagent/pull/3606) | docs: document release branch backport policy | timflannagan | 2026-11-09 |
| kagent | [#3590](https://github.com/kagent-dev/kagent/pull/3590) / [#3591](https://github.com/kagent-dev/kagent/pull/3591) | cleanup: drop unused ConfigHashAnnotation, KubeClientWrapper | timflannagan | 2026-11-09 |
| kagent | 9 dependabot PRs | [#3576](https://github.com/kagent-dev/kagent/pull/3576) [#3577](https://github.com/kagent-dev/kagent/pull/3577) [#3578](https://github.com/kagent-dev/kagent/pull/3578) [#3580](https://github.com/kagent-dev/kagent/pull/3580) [#3582](https://github.com/kagent-dev/kagent/pull/3582) [#3583](https://github.com/kagent-dev/kagent/pull/3583) [#3584](https://github.com/kagent-dev/kagent/pull/3584) | dependabot | 2026-11-09 |
| substrate | [#165](https://github.com/kagent-dev/substrate/pull/165) | install: make install and upgrade values declarative | EItanya | 2026-11-09 |
| substrate | [#167](https://github.com/kagent-dev/substrate/pull/167) | [release-1.0] backport step lists, Options.Version, declarative values | EItanya | 2026-11-10 |
| website | [#703](https://github.com/kagent-dev/website/pull/703) | 1.0.0 release switchover (`latest` → 1.0.x) | Rachael-Graham | 2026-11-09 |
| website | [#715](https://github.com/kagent-dev/website/pull/715) | reference docs at v1.0.0 / kmcp v0.4.0 (conrefs changed) | github-actions | 2026-11-09 |
| website | [#714](https://github.com/kagent-dev/website/pull/714) | Blog: kagent 1.0 Is Here, agents on Agent Substrate | sebbycorp | 2026-11-09 |
| website | [#705](https://github.com/kagent-dev/website/pull/705) | simplify the kagent install docs for kagent#3566 | EItanya | 2026-11-10 |
| website | [#706](https://github.com/kagent-dev/website/pull/706) | Anthropic self-hosted sandboxes | Stevenjin8 | 2026-11-09 |
| website | [#710](https://github.com/kagent-dev/website/pull/710) [#711](https://github.com/kagent-dev/website/pull/711) [#712](https://github.com/kagent-dev/website/pull/712) [#713](https://github.com/kagent-dev/website/pull/713) | build fixes, link fixes, stop tracking Python bytecode | Nadine2016, kristin-kronstain-brown | 2026-11-09 |
| doc2vec | [#142](https://github.com/kagent-dev/doc2vec/pull/142) | survive browserless restarts, skip oversized pages (v2.17.4) | djannot | 2026-11-10 |

All merged PRs in window shown. None in khook, mockllm, community, byo-ext.

## Watch list (kagent Enterprise alpha12 / Substrate on omni-viper)

- **Pin vs. reality:** kagent 1.0.0 pins substrate `v1.0.0`, but substrate `release-1.0` now carries the installer library, `Options.Version` and declarative values via `#159`/`#167` with **no tag yet**. Test installer-driven installs off a `v1.0.2`/`main` chart, not the GA tag.
- **`#165` behaviour change:** keep the install config with the cluster — `upgrade` without `-c` silently applies bundled defaults onto an external DB. `install` refuses on a deployed release; use `upgrade`, and `--dry-run` first.
- **`#3616` CEL rules are a silent breaking change** for any saved Claude/Codex Harness with `workload.command`/`args`; audit that YAML before the alpha upgrade.
- **Runtime image digests moved** with the v1.0.0 docs regeneration (`claude-harness`, `codex-harness`, `golang-adk`) — re-pin if demo YAML pins by digest.
- **Security-adjacent, still open:** runtime certificate trust for v0.10.x ([#3320](https://github.com/kagent-dev/kagent/pull/3320)); extra egress CAs for the gateway upstream TLS ([substrate#101](https://github.com/kagent-dev/substrate/pull/101)).
- **Open PRs worth exercising on omni-viper:** per-Harness CPU/memory limits [#3625](https://github.com/kagent-dev/kagent/pull/3625), session ID to MCP servers in a configurable header [#3628](https://github.com/kagent-dev/kagent/pull/3628), forward caller credential on MCP/model calls [#3018](https://github.com/kagent-dev/kagent/pull/3018), declarative graph workflows for ADK Go 2.x [#3344](https://github.com/kagent-dev/kagent/pull/3344), interrupted-checkpoint recovery [#3519](https://github.com/kagent-dev/kagent/pull/3519), opt-in crash recovery on send [#3399](https://github.com/kagent-dev/kagent/pull/3399), AKS installer profile [substrate#161](https://github.com/kagent-dev/substrate/pull/161).
- **Follow-ups from `#3566` not yet done:** a `kagent.dev/install` redirect + favicon, and removing the now-obsolete `grpcReflection` step for plain Helm installs — don't quote the quickstart verbatim mid-flux.
- Carried over: substrate `upgrade` still doesn't roll WorkerPool images; `uninstall` keeps CRDs/CA keys/PVC without `--include-crds`/`--purge`; doc2vec can't yet pre-skip compressed/chunked responses, and the browserless 4Gi limit (incl. its memory-backed `/dev/shm`) still counts against one budget.
