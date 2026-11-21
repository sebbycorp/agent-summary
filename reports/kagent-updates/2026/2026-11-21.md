# kagent-updates - 2026-11-21

**No published releases anywhere; the day was security — the nginx 1.31.9 HTTP/2 backport landed on `release/v1.0.x` ([#3732](https://github.com/kagent-dev/kagent/pull/3732)) and v0.10.6 image CVEs were fixed ([#3738](https://github.com/kagent-dev/kagent/pull/3738)) — while kagent `main` re-pinned Substrate to a new `v1.1.0-alpha1` tag ([#3754](https://github.com/kagent-dev/kagent/pull/3754)). 12 PRs merged across 3 repos (9 kagent, 2 tools, 1 website), zero in substrate.**

Window: since 2026-11-20 (`latest.md`). Today: 2026-11-21 (America/Toronto).

## Releases

- **None published in window.** Pushes only in kagent, tools, website; **substrate `main` had no commits**.
- **New tag, no release object:** substrate **`v1.1.0-alpha1`** ([tags](https://github.com/kagent-dev/substrate/tags), at [`fd66419`](https://github.com/kagent-dev/substrate/commit/fd66419e10fddc6209ee2c4c4135fafbb17fe845) = the 11-19 OpenShift SCC merge). Brings `kubectl-ate` installer packages, extra egress credential providers, atelet `nodeSelector`.
- Latest stable unchanged: [kagent v1.0.1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.1), [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1), [tools v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1), [doc2vec v2.17.5](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.5), [kmcp v0.4.0](https://github.com/kagent-dev/kmcp/releases/tag/v0.4.0).

## Highlights

- **CVE, your branch:** [#3732](https://github.com/kagent-dev/kagent/pull/3732) merged to `release/v1.0.x` ([`c86529b`](https://github.com/kagent-dev/kagent/commit/c86529b07a780ac765e432509c0ca9a74cfddd3c)) — UI nginx **1.31.8 → 1.31.9**, fixing HTTP/2 RCEs CVE-2026-123633 / CVE-2026-123007. Yesterday's blocked watch-list item is done; alpha12's nginx risk closes once you build v1.0.x off that commit. Stock `nginx.conf` doesn't enable HTTP/2.
- **CVE, 0.10.x:** [#3738](https://github.com/kagent-dev/kagent/pull/3738) ([`f82ac99`](https://github.com/kagent-dev/kagent/commit/f82ac9950e26d087efaab1fc3b030a6fa1034264)) — HIGH/CRITICAL in undici, glibc, nodejs, git, zlib fixed via `wolfi-base:latest` + npm 11.21.3; `skills-init` Alpine → Wolfi (64.7 → 128 MB). Also fixes the rolling-upgrade test flake.
- **CVE, tools image:** [#118](https://github.com/kagent-dev/tools/pull/118) ([`073c2b3`](https://github.com/kagent-dev/tools/commit/073c2b3a504842a33113cc8c713553ce9bb04836)) — helm 4.3.2, cilium-cli 0.20.3 (CVE-2026-111943, CVE-2026-78661). Still flagged: kubectl 1.37.2 CVE-2026-78663 (CRITICAL) and kubectl-argo-rollouts — no stable upstream fix.
- **Breaking, Python BYO:** [#3729](https://github.com/kagent-dev/kagent/pull/3729) — all four Python adapters share the ADK **A2A gRPC server on port 80**, `/readyz` on 8081; Uvicorn on 8080 removed. Entry points move `uvicorn.run(app.build(), ...)` → **`app.build().run()`**; without `--local`, `kagent-adk run` reads `config.json`/`agent-card.json` only from `/config`. Fixes Python/CrewAI/LangGraph readiness failures on Substrate — retest demo agents.
- **Pairing shifted:** [#3754](https://github.com/kagent-dev/kagent/pull/3754) pins `main` to substrate **v1.1.0-alpha1** (`ateapi.proto` identical, no wire change). Alpha12 (kagent 1.0.x) still pairs with substrate **v1.0.1** — not "0.6", not the alpha.
- **Security/auth:** [#3726](https://github.com/kagent-dev/kagent/pull/3726) — push **signing-key hot reload** from Secret/CSI with staged rotation (old+new JWKS keys advertised, Helm delay 5m default / 24h max), callback backoff capped at 60s / 20 durable attempts. No controller restart to rotate keys.
- **Model behaviour:** [#2997](https://github.com/kagent-dev/kagent/pull/2997) — Go ADK stops truncating earlier **Bedrock** tool results to 2000 bytes, so a loaded SKILL.md or fetched YAML stays visible across later tool calls; costs more input tokens unless `spec.kagent.compaction` is set.
- **Smaller:** [#3279](https://github.com/kagent-dev/kagent/pull/3279) Ollama string options (`num_ctx: "2048.0"`, `1/yes/on`) no longer crash the agent; [#3318](https://github.com/kagent-dev/kagent/pull/3318) CI/dev kind now **Kubernetes 1.37 with no beta certificate APIs**, matching the install guide (matters for EKS).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3732](https://github.com/kagent-dev/kagent/pull/3732) | [backport] bump nginx to 1.31.9 for HTTP/2 CVEs | Charlesthebird | 11-20 |
| kagent | [#3738](https://github.com/kagent-dev/kagent/pull/3738) | fix v0.10.6 image CVEs + upgrade-test flake | danielle-mustillo | 11-20 |
| kagent | [#3729](https://github.com/kagent-dev/kagent/pull/3729) | serve Python BYO agents over A2A gRPC on Substrate | supreme-gg-gg | 11-20 |
| kagent | [#3726](https://github.com/kagent-dev/kagent/pull/3726) | hot reload push signing keys, staged activation | shashankram | 11-20 |
| kagent | [#2997](https://github.com/kagent-dev/kagent/pull/2997) | stop truncating earlier Bedrock tool results | GuilleQP | 11-20 |
| kagent | [#3754](https://github.com/kagent-dev/kagent/pull/3754) | upgrade Substrate to v1.1.0-alpha1 | timflannagan | 11-20 |
| kagent | [#3318](https://github.com/kagent-dev/kagent/pull/3318) | test on Kubernetes 1.37 without beta cert APIs | ayushgharat234 | 11-20 |
| kagent | [#3279](https://github.com/kagent-dev/kagent/pull/3279) | handle Ollama options written as strings | piyush182004 | 11-20 |
| kagent | [#3739](https://github.com/kagent-dev/kagent/pull/3739) | steady modals, hover/contrast, chat markdown | Charlesthebird | 11-20 |
| tools | [#118](https://github.com/kagent-dev/tools/pull/118) | bump bundled helm 4.3.2, cilium-cli 0.20.3 | danielle-mustillo | 11-20 |
| website | [#744](https://github.com/kagent-dev/website/pull/744) | docs for the kagent sandboxes section | artberger | 11-20 |

\+1 in the 12, reported yesterday: tools [#106](https://github.com/kagent-dev/tools/pull/106). Website also took direct pushes [`e649721`](https://github.com/kagent-dev/website/commit/e6497212d51596f2db992995b4c28ddf4cef05f5) and [`ecd648f`](https://github.com/kagent-dev/website/commit/ecd648fe2c0349d1e2074b754f70ed5f90c0b8d1) — [full list](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3E%3D2026-11-20&type=pullrequests).

## Watch list (kagent Enterprise alpha12 / Agent Substrate on omni-viper)

- **Move alpha12 to a post-`c86529b` v1.0.x build** for nginx 1.31.9; the 0.10.x CVE fixes are branch-only — no v0.10.7 / v1.0.2 tag cut, so it's "build from branch", not a pull-able release.
- **Re-run the Substrate install path** after the alpha1 pin ([#3754](https://github.com/kagent-dev/kagent/pull/3754) pulls `kubectl-ate` installer packages + new egress credential providers). OpenShift still needs an SCC grant until a post-`fd66419` substrate release.
- **Docs ahead of your build:** sandboxes docs ([#744](https://github.com/kagent-dev/website/pull/744)) and the Claude-managed-agents page are `main`-only — check against alpha12 before quoting in a deck.
- **Test on merge, kagent open:** [#3752](https://github.com/kagent-dev/kagent/pull/3752) list models from `ModelProviderConfig`, [#3748](https://github.com/kagent-dev/kagent/pull/3748) `kagent status`, [#3749](https://github.com/kagent-dev/kagent/pull/3749) Go 1.27.4, [#3750](https://github.com/kagent-dev/kagent/pull/3750) read-only `GITHUB_TOKEN`, [#3708](https://github.com/kagent-dev/kagent/pull/3708) canceled dispatches, [#3435](https://github.com/kagent-dev/kagent/pull/3435) git/S3 skills, [#3341](https://github.com/kagent-dev/kagent/pull/3341) snapshot upload on suspend, [#3687](https://github.com/kagent-dev/kagent/pull/3687) Bearer case-insensitivity.
- **Substrate idle, 1.1 queue deep:** IPv6 [#183](https://github.com/kagent-dev/substrate/pull/183)–[#187](https://github.com/kagent-dev/substrate/pull/187) all open (untouched since 11-19); chart asks [#176](https://github.com/kagent-dev/substrate/pull/176), [#175](https://github.com/kagent-dev/substrate/pull/175), [#174](https://github.com/kagent-dev/substrate/pull/174), [#169](https://github.com/kagent-dev/substrate/pull/169), [#131](https://github.com/kagent-dev/substrate/pull/131), [#161](https://github.com/kagent-dev/substrate/pull/161).
- **doc2vec:** draft [#149](https://github.com/kagent-dev/doc2vec/pull/149) (Zendesk OAuth client credentials) opened 11-20.
