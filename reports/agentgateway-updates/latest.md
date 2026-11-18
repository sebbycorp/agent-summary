# agentgateway-updates - 2026-11-18

**Quiet day after the storm: 2 merges into `agentgateway` main (Postgres concurrent-startup fix, macOS/Rust 1.100 build fix) and 6 into `website` (the headline being installable OSS "assistant skills"). Still no 1.8 tag — newest artifact remains the Oct 27 v1.7.0 cut.**

_Window: merged/pushed after the last report's overnight batch (effectively `>=2026-11-17T02:00Z`; a raw `merged:>=2026-11-17` search returns 38, but ~30 of those are the `[Automated] Draft docs` / reference-docs batch already covered on 11-17). `agentgateway` main has no commits on 11-18. `benchmarks` and `community` idle._

## Releases

**None published in window.** Everything below the Oct 27 cut is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [v1.7.0 "Docs for v1.7.0 release"](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | – |
| benchmarks · community | None | – |

## Highlights

- **Multiple proxies can boot against one Postgres at once** — [agw#4368](https://github.com/agentgateway/agentgateway/pull/4368) (conradhanson, merged by stevenctl 11-17, +433/−9, 6 files). Table creation now runs in a **single transaction guarded by an advisory lock**; the cleanup `DELETE` is deliberately last and outside it (idempotent, safe under concurrency). Fixes the startup race/failure you hit when rolling N replicas against a shared DB for budgets/request-log storage. New `postgres_tests.rs` covers concurrent startup, that lock timeouts don't leak into the pool, restart-against-current-schema not blocking on other proxies' open transactions, and expired-row deletes. Demo-relevant: this was the "second replica CrashLoops on the demo Postgres" failure mode.
- **macOS builds again on Rust 1.100; benches can opt out of pprof** — [agw#4450](https://github.com/agentgateway/agentgateway/pull/4450) (markuskobler, 11-17, +10/−2). `pprof` is now a **Linux-only** library dependency (it backs the Linux-only admin endpoint) plus a dev-dependency so the bench hook still profiles on macOS. Also: benches sample at 1000 Hz *inside* the measured code and skew timings — set `AGENTGATEWAY_BENCH_NO_PPROF=1` for clean numbers before quoting any local benchmark figure.
- **OSS "AI assistant skills" shipped on the docs site** — website [#1523](https://github.com/agentgateway/website/pull/1523) (jamesilse-solo, merged by artberger 11-17, 57 files, +2632). Two installable skill packages (standalone + Kubernetes controller) distributed via `npx skills` and stable `current` ZIP URLs, with "Set up with an AI assistant" pages linked from quickstart and installation, OpenAI/Vertex/Bedrock examples, deterministic packaging and integrity checks. Marked *preview*; validation is fixture-based (no live provider auth, cluster deploy, licensing or budget enforcement). Good scripted-demo material — and worth rehearsing, since it drives agent-facing config generation.
- **Docs CI was red on `main` and is fixed; hidden 1.6.x content restored** — website [#1555](https://github.com/agentgateway/website/pull/1555) (kristin-kronstain-brown, 11-17). If you saw stale/missing versioned pages last week, that's why.
- **Azure federation docs landed** — website [#1556](https://github.com/agentgateway/website/pull/1556) (artberger, 11-17): Azure role + GKE-to-Azure-identity wiring for LLM backends.
- **Datadog guides reworked** — website [#1481](https://github.com/agentgateway/website/pull/1481) (kristin-kronstain-brown, 11-17), aligned to the reworked upstream observability example — the copy to hand customers in the observability slide.
- **Reference docs refreshed from both `main` and `v1.7.0`** — [#1553](https://github.com/agentgateway/website/pull/1553), [#1554](https://github.com/agentgateway/website/pull/1554) (bot, 11-17); the schema/CLI reference the site publishes is now current with the 11-16 feature batch.
- **No CVEs, no breaking merges** in window. The security-adjacent work (config file deny/allowlist, JWKS isolation, top-level guardrail policies) is all still **open**.

## Merged PRs by repo

Window total: **8 merges** (2 `agentgateway`, 6 `website`) beyond what the 11-17 report covered.

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4368](https://github.com/agentgateway/agentgateway/pull/4368) | postgres table-related sql queries use single xact with advisory lock | conradhanson | 11-17 |
| agentgateway | [4450](https://github.com/agentgateway/agentgateway/pull/4450) | fix: build on macOS with Rust 1.100 and allow opting out of bench pprof | markuskobler | 11-17 |
| website | [1523](https://github.com/agentgateway/website/pull/1523) | docs: add OSS agentgateway assistant skills and setup guides | jamesilse-solo | 11-17 |
| website | [1555](https://github.com/agentgateway/website/pull/1555) | Fix failing CI checks on main and restore hidden 1.6.x content | kristin-kronstain-brown | 11-17 |
| website | [1556](https://github.com/agentgateway/website/pull/1556) | azure role and gke to azure identity | artberger | 11-17 |
| website | [1481](https://github.com/agentgateway/website/pull/1481) | Update Datadog guides for the reworked upstream example | kristin-kronstain-brown | 11-17 |
| website | [1553](https://github.com/agentgateway/website/pull/1553) | [Automated] Update reference docs (main) from agentgateway main | github-actions[bot] | 11-17 |
| website | [1554](https://github.com/agentgateway/website/pull/1554) | [Automated] Refresh product-UI screenshots | github-actions[bot] | 11-17 |

No new merges in `benchmarks` or `community`.

## Watch list

| Repo | PR | Why |
| --- | --- | --- |
| agentgateway | [4430](https://github.com/agentgateway/agentgateway/pull/4430) | **(breaking, still blocked)** default `agentgateway.user` becomes `jwt.email` → `jwt.sub` → API-key metadata, dropping `apiKey.name`. Splits request-log history per person across the upgrade, changes UI API-key ownership, and attributes JWT+key requests to the JWT. Pin `standardAttributes.user` for old behaviour. |
| agentgateway | [4431](https://github.com/agentgateway/agentgateway/pull/4431) | Security hardening: deny/allowlist files referenced from config (blocks `/dev/…`-style references). Blocked, needs review. |
| agentgateway | [4387](https://github.com/agentgateway/agentgateway/pull/4387) | **CRD shape change**: prompt-guard policies move to top level with per-type CEL rules (`openAIModeration` key/secretRef, `bedrockGuardrails` +aws, `googleModelArmor` gcp only); backend CRD 1.23 MB → 1.08 MB, old nested fields kept with a deprecation warning. Blocked; prep for #4205. |
| agentgateway | [4394](https://github.com/agentgateway/agentgateway/pull/4394) · [4402](https://github.com/agentgateway/agentgateway/pull/4402) · [4447](https://github.com/agentgateway/agentgateway/pull/4447) | Auth isolation for remote JWKS failures; Gemini Decisions/SystemOne routing; drop catalog tiers with an unrecognized `serviceTier`. |
| agentgateway | [4453](https://github.com/agentgateway/agentgateway/pull/4453) · [4454](https://github.com/agentgateway/agentgateway/pull/4454) | New MCP work (11-17): reject `server/discover` for stdio and SSE targets; expose per-target instructions as `instructions://`. Test MCP discovery in multi-target demos. |
| agentgateway | [4349](https://github.com/agentgateway/agentgateway/pull/4349) · [4409](https://github.com/agentgateway/agentgateway/pull/4409) | Streaming token reassembly (data tokenization) and UI migration to antd components — UI screenshots in decks may drift. |
| agentgateway | [4330](https://github.com/agentgateway/agentgateway/pull/4330) · [4309](https://github.com/agentgateway/agentgateway/pull/4309) · [2784](https://github.com/agentgateway/agentgateway/pull/2784) | Bedrock Claude thinking translation, Bedrock/Mantle `/openai` route prefix, native Gemini-on-Vertex for Anthropic Messages. All touched 11-17; all Bedrock/Vertex demo paths. |
| agentgateway | [4449](https://github.com/agentgateway/agentgateway/pull/4449) · [4451](https://github.com/agentgateway/agentgateway/pull/4451) · [4448](https://github.com/agentgateway/agentgateway/pull/4448) | AWS SDK `BehaviorVersion` bump, cargo-update cooldown policy, and no session timeout during DB migration. |

Re-test on omni-viper with `:latest-dev`: 3 replicas + shared Postgres cold-start (4368, incl. lock-timeout/restart cases), macOS bench run with `AGENTGATEWAY_BENCH_NO_PPROF=1` (4450), plus last cycle's MCP GET-across-replicas, malformed-Host 400, Vertex beta-header filter and standalone `llm.policies.retry`. Try the new assistant-skill install path before quoting it on stage.
