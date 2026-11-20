# kagent-updates - 2026-11-20

**No releases and zero merged PRs or commits in `kagent` itself (main and both release branches idle since 11-19) — the window was docs and charts: substrate merged the OpenShift SCC fix ([#180](https://github.com/kagent-dev/substrate/pull/180)), tools merged the PDB rework ([#106](https://github.com/kagent-dev/tools/pull/106)), website took 10 PRs documenting breaking kagent changes. 12 PRs merged across 3 repos.**

Window: 2026-11-19 → 11-20 (since `latest.md` @ 11-19). Today: 2026-11-20 (America/Toronto).

## Releases

- **None published in window.** [merged:>=2026-11-19 search](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3E%3D2026-11-19&type=pullrequests) → 12 PRs, no tag cut, no new RC.
- Commits in window: **substrate 4** (merge [`fd66419`](https://github.com/kagent-dev/substrate/commit/fd66419e10fddc6209ee2c4c4135fafbb17fe845) of #180 + [`56f8c7a`](https://github.com/kagent-dev/substrate/commit/56f8c7a91c26cf40854ff64871f3650f7ce7d504) etc.), **tools 1** ([`b00ae36`](https://github.com/kagent-dev/tools/commit/b00ae36fd5aa76f2f53f78232928e291a292e865)), **website ~20**, **kagent 0**.
- Tops unchanged: [kagent v1.0.1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.1) (11-12; latest RC [v1.0.0-rc4](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc4)), [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) (11-08), [tools v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1) (11-04), [doc2vec v2.17.5](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.5) (11-12), kmcp v0.4.0 (09-17).

## Highlights

- **OpenShift friction fixed in substrate `main`:** [#180](https://github.com/kagent-dev/substrate/pull/180) (adalundhe, 11-19) stops pinning pod UIDs so `restricted-v2` assigns them (RustFS drops `runAsUser`/`fsGroup` on OpenShift; credential-provider sets only `runAsNonRoot: true`). Proven on CRC 4.21.14 / 5.0.0. Caveats: **not in v1.0.1**, the `manifests/` kustomize still pins IDs, and OpenShift renders need `helm template --api-versions security.openshift.io/v1`.
- **Breaking, telemetry:** kagent [#3552](https://github.com/kagent-dev/kagent/pull/3552) — gateway `x-user-id` is now span attr **`user.id`, not `enduser.id`** (website [#727](https://github.com/kagent-dev/website/pull/727)). Fix per-user cost/audit panels before a multi-tenant demo.
- **Breaking, Azure OpenAI:** kagent [#3651](https://github.com/kagent-dev/kagent/pull/3651) — **`azureAdToken` removed from `ModelConfig`** (it bypassed gateway credential injection); website [#731](https://github.com/kagent-dev/website/pull/731). Use an API-key Secret, token passthrough, or `actorJwt`.
- **Session lifecycle, two operator knobs documented:** opt-in `config.sessions.automaticRecovery` (A2A send drives crash recovery; in-progress → `KAGENT_SEND_NOT_ACCEPTED`, `state: RECOVERING`) — website [#730](https://github.com/kagent-dev/website/pull/730); and `kagent agent session delete --force` (kills a mid-turn session, no Actor suspend) — website [#729](https://github.com/kagent-dev/website/pull/729).
- **MCP startup latency:** kagent [#3560](https://github.com/kagent-dev/kagent/pull/3560) defers MCP discovery to invocation — no more blocking startup/readiness/Session-resume on a slow server; its tools are omitted for that execution and retried next run. Website [#732](https://github.com/kagent-dev/website/pull/732).
- **Chart hardening, tools `main`:** [#106](https://github.com/kagent-dev/tools/pull/106) (dimetron, 11-20) — PDB `0` budgets now render, both-set/neither-set now fails with an actionable message, PDB only renders at `replicaCount > 1` with a `NOTES.txt` warning when skipped. Not in v0.3.1.
- **Also newly documented:** PostgreSQL login retries via `config.database.pool.connectRetryTimeout` (default `90s`, `0s` = one try) — website [#734](https://github.com/kagent-dev/website/pull/734); UI EgressPolicy CRUD — [#726](https://github.com/kagent-dev/website/pull/726); Harness `compactionInterval` without `overlapSize` → overlap `0`, plus recovery counters by `kagent_worker_trigger` — [#735](https://github.com/kagent-dev/website/pull/735).
- **IPv6 still unmerged:** substrate [#185](https://github.com/kagent-dev/substrate/pull/185) (+904/−331, 27 files) remains `blocked`; companions [#183](https://github.com/kagent-dev/substrate/pull/183)–[#187](https://github.com/kagent-dev/substrate/pull/187) all open, no IPv6 commits landed.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| substrate | [#180](https://github.com/kagent-dev/substrate/pull/180) | charts: let OpenShift's SCC assign pod IDs | adalundhe | 11-19 |
| tools | [#106](https://github.com/kagent-dev/tools/pull/106) | fix(helm): preserve zero PDB budgets, validate min/max | dimetron | 11-20 |
| website | [#730](https://github.com/kagent-dev/website/pull/730) | Draft docs: opt-in crash recovery on send | github-actions[bot] | 11-19 |
| website | [#727](https://github.com/kagent-dev/website/pull/727) | Draft docs: record trusted user as `user.id` | github-actions[bot] | 11-19 |
| website | [#726](https://github.com/kagent-dev/website/pull/726) | Draft docs: manage EgressPolicies from the UI | github-actions[bot] | 11-19 |
| website | [#731](https://github.com/kagent-dev/website/pull/731) | Draft docs: remove Azure OpenAI inline token | github-actions[bot] | 11-19 |
| website | [#729](https://github.com/kagent-dev/website/pull/729) | Draft docs: session force deletion | github-actions[bot] | 11-19 |
| website | [#732](https://github.com/kagent-dev/website/pull/732) | Draft docs: defer MCP tool discovery | github-actions[bot] | 11-19 |
| website | [#734](https://github.com/kagent-dev/website/pull/734) | Draft docs: retry failed PostgreSQL logins | github-actions[bot] | 11-19 |
| website | [#735](https://github.com/kagent-dev/website/pull/735) | Draft docs: compaction overlap + recovery telemetry | github-actions[bot] | 11-19 |

\+2 more (website dep bumps [#741](https://github.com/kagent-dev/website/pull/741), [#740](https://github.com/kagent-dev/website/pull/740)) — [full list](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3E%3D2026-11-19&type=pullrequests). No merged PRs or commits at all in kagent, kmcp, doc2vec, khook, byo-ext or community.

## Watch list (kagent Enterprise alpha12 / Agent Substrate 0.6 on omni-viper)

- **nginx CVE still unpatched on your branch:** backport [#3732](https://github.com/kagent-dev/kagent/pull/3732) → `release/v1.0.x` still open, `mergeable_state: blocked`, and that branch had no commits in the window — alpha12 ships nginx 1.31.8. Keep HTTP/2 off or rebuild the UI image. Related: [#3738](https://github.com/kagent-dev/kagent/pull/3738) fixes v0.10.6 image CVEs.
- **Docs are ahead of alpha12:** `user.id` rename, `azureAdToken` removal, `automaticRecovery`, `--force` delete, deferred MCP discovery, UI EgressPolicies are all `main`-only — verify each against your build before quoting the site.
- **Substrate pairing:** kagent 1.0.x ↔ substrate **v1.0.1** (not "0.6"). SCC fix is post-tag, so OpenShift still needs an `anyuid`/`nonroot-v2` grant until the next release.
- **Open, worth re-testing on merge:** substrate [#48](https://github.com/kagent-dev/substrate/pull/48) `podLabels`, [#131](https://github.com/kagent-dev/substrate/pull/131) namespace-bound Secret reader; kagent [#3435](https://github.com/kagent-dev/kagent/pull/3435) git/S3 skill sources, [#3341](https://github.com/kagent-dev/kagent/pull/3341) snapshot upload on suspend, [#3413](https://github.com/kagent-dev/kagent/pull/3413) policy for verified actors, [#3726](https://github.com/kagent-dev/kagent/pull/3726) signing-key hot reload, [#3729](https://github.com/kagent-dev/kagent/pull/3729) Python BYO gRPC entry point; tools [#118](https://github.com/kagent-dev/tools/pull/118) (helm 4.3.2, cilium-cli 0.20.3).
- **Docs pipeline noise:** 10 of 12 merged PRs were bot-drafted from a solo-io draft repo and landed despite a `markdown-leaks` scan failure — spot-check any page you cite in a deck.
