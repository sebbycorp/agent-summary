# kagent-updates - 2026-11-19

**No releases anywhere (7th day) and only 5 merged PRs org-wide — your blog post finally landed ([website #722](https://github.com/kagent-dev/website/pull/722)) and an nginx HTTP/2-RCE fix merged on `main` ([#3730](https://github.com/kagent-dev/kagent/pull/3730), backport to `release/v1.0.x` still open, so alpha12 lacks it). Quiet on merges, busy upstream: ~18 PRs opened since yesterday, dominated by a 6-PR IPv6/dual-stack actor-networking series in substrate and a Python-BYO-on-Substrate rework that changes the entry point.**

Window: 2026-11-18 → 11-19 (since `latest.md` @ 11-18). Today: 2026-11-19 (America/Toronto).

## Releases

- **None published in window.** [merged:>=2026-11-18 search](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3E%3D2026-11-18&type=pullrequests) → 5 PRs, no tag cut, no new RC.
- Commits since 11-18: **kagent 3** ([`d5c04c1`](https://github.com/kagent-dev/kagent/commit/d5c04c178186a2803bbf86aac7fec8a18fd8a770) nginx, [`97fe83d`](https://github.com/kagent-dev/kagent/commit/97fe83d7af8bf98ac0682dd854984b0b45a23389) google-auth, [`92ea3e6`](https://github.com/kagent-dev/kagent/commit/92ea3e684c29129d670a7ea8cddccf2ae06c569d) docs), **website 2** ([`0bd9bcc`](https://github.com/kagent-dev/website/commit/0bd9bcc4bddd026e9bd14a5b0e958af53e7edede), [`13e6507`](https://github.com/kagent-dev/website/commit/13e65071037654c96284b25bf45ebdf79f4d9418)), **substrate / tools / kmcp / doc2vec zero**.
- Tops unchanged: [kagent v1.0.1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.1) (11-12; latest RC [v1.0.0-rc4](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc4)), [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) (11-08), [tools v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1) (11-04), [doc2vec v2.17.5](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.5) (11-12).

## Highlights

- **Security: nginx HTTP/2 RCE fixed on `main`, not in 1.0.x.** [#3730](https://github.com/kagent-dev/kagent/pull/3730) (Charlesthebird, merged 11-18) bumps the UI base 1.31.8→1.31.9 for CVE-2026-123633 / CVE-2026-123007 ([Wiz](https://www.wiz.io/blog/nginx-http2-rce-vulnerabilities)). Stock installs are safe (default `nginx.conf` keeps HTTP/2 off); custom configs that enable it aren't. **Backport [#3732](https://github.com/kagent-dev/kagent/pull/3732) → `release/v1.0.x` is still blocked** — Enterprise alpha12 (from v1.0.1) lacks it.
- **Your blog post shipped:** [website #722](https://github.com/kagent-dev/website/pull/722) "Install Agent Substrate on Any Kubernetes Cluster" (Michael Levan, universal `kubectl-ate` installer: Kind/Minikube/AKS/EKS/GKE, no placeholder `PROJECT_ID`) merged 11-18 after sitting open since 11-11 — good omni-viper leave-behind.
- **Substrate: IPv6 is now a workstream (6 PRs, steigr, all 11-19).** Core [#185](https://github.com/kagent-dev/substrate/pull/185): the actor sandbox follows the worker pod's IP families (IPv4 `/30`, IPv6 `fd00:a7e:17::/126`, or both) with per-family nftables, `/etc/hosts`, wakeup probe and a new `actornet.Families` API for gVisor and microVM (~1,240 lines, 60% tests). Companions: [#183](https://github.com/kagent-dev/substrate/pull/183) listeners, [#184](https://github.com/kagent-dev/substrate/pull/184) kind DNS, [#186](https://github.com/kagent-dev/substrate/pull/186) Jupyter dual-stack, [#187](https://github.com/kagent-dev/substrate/pull/187) bracketed IPv6 egress authority. Gaps: microVM snapshots don't record families; IPv6-only can't pull from IPv4-only registries (quay.io hosts the Jupyter image). All unmerged.
- **Breaking-ish for Python BYO on Substrate:** [#3729](https://github.com/kagent-dev/kagent/pull/3729) (supreme-gg-gg, +652/−1480, 63 files) — the gateway sent gRPC to an HTTP JSON-RPC server, so Python BYO requests never reached the model. A2A gRPC now on port 80, `/readyz` on 8081, Uvicorn/FastAPI/`build_local()` removed; **entry point moves `uvicorn.run(app.build(), …)` → `app.build().run()`** for OpenAI `KAgentApp`, CrewAI, LangGraph, ADK.
- **Signing-key lifecycle:** [#3726](https://github.com/kagent-dev/kagent/pull/3726) (shashankram, +754/−114) — push/callback keys hot-reload with staged activation: both public keys advertised, new-key signing delayed (Helm-tunable 5m default / 24h max, +30s), old keys kept through JWT expiry+skew, callback backoff capped 60s/20 attempts. Trade-offs: restarts lose key history; replicas reload independently.
- **A2A tenant propagation:** [#3735](https://github.com/kagent-dev/kagent/pull/3735) (Xio-Shark, fixes #3210) — remote A2A tool lacked `DisableTenantPropagation`, so a BYO agent calling another Agent carried `kagent/<caller>` and the gateway rejected it ("tenant does not match the Agent URL"). **Resubmission of #3219, which stalebot auto-closed 11-18 with the bug still open.**
- **Scheduled dispatches:** [#3728](https://github.com/kagent-dev/kagent/pull/3728) (fixes #3727) — gateway-rejected dispatches stayed RUNNING until deadline; now FAILED with the original reason. Same family as yesterday's actor-leak [#3708](https://github.com/kagent-dev/kagent/pull/3708).
- **Substrate memory fix closed unmerged:** [#181](https://github.com/kagent-dev/substrate/pull/181) scoped the atecontroller cache to Deployments labelled `ate.dev/worker-pool` — 199→10.2 MiB at 10k unrelated Deployments, 516→10.2 MiB at 30k (carries upstream #3923). Useful number for dense clusters; fix not present.
- **ADK polish, all open:** cached+reasoning tokens for OpenAI usage [#3734](https://github.com/kagent-dev/kagent/pull/3734); Ollama tool-name [#3724](https://github.com/kagent-dev/kagent/pull/3724)/stop-on-cancel [#3723](https://github.com/kagent-dev/kagent/pull/3723); LangGraph tool-call publishing [#3721](https://github.com/kagent-dev/kagent/pull/3721); Bedrock blank tool-result [#3718](https://github.com/kagent-dev/kagent/pull/3718); OpenAI SDK tool-result↔call link [#3716](https://github.com/kagent-dev/kagent/pull/3716); `kagent dashboard` on Linux [#3733](https://github.com/kagent-dev/kagent/pull/3733); live UI specs on the built image [#3722](https://github.com/kagent-dev/kagent/pull/3722).
- **Closed unmerged:** `deny` auth mode [#3719](https://github.com/kagent-dev/kagent/pull/3719) (multi-tenant gap still open on v1.0.1; live work [#3018](https://github.com/kagent-dev/kagent/pull/3018)), Azure Workload Identity chart tests [#3731](https://github.com/kagent-dev/kagent/pull/3731).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3730](https://github.com/kagent-dev/kagent/pull/3730) | fix(ui): bump nginx to 1.31.9 for HTTP/2 CVEs | Charlesthebird | 11-18 |
| kagent | [#3703](https://github.com/kagent-dev/kagent/pull/3703) | chore(deps): bump google-auth 2.62.1 → 2.62.2 (/python) | dependabot[bot] | 11-18 |
| kagent | [#3713](https://github.com/kagent-dev/kagent/pull/3713) | docs: fix dead OpenAI Agents SDK / contributing links | farisbasha | 11-18 |
| website | [#722](https://github.com/kagent-dev/website/pull/722) | Blog: Install Agent Substrate on Any Kubernetes Cluster | sebbycorp | 11-18 |
| website | [#739](https://github.com/kagent-dev/website/pull/739) | chore(deps): bump @playwright/test 1.65.0 → 1.65.1 | dependabot[bot] | 11-18 |

That is the entire window — no merged PRs in substrate, tools, kmcp, doc2vec, khook, byo-ext or community.

## Watch list (kagent Enterprise alpha12 / Agent Substrate 0.6 on omni-viper)

- **Chase nginx backport [#3732](https://github.com/kagent-dev/kagent/pull/3732)** — alpha12 ships nginx 1.31.8; rebuild the UI image or keep HTTP/2 off.
- **Re-test Python BYO when [#3729](https://github.com/kagent-dev/kagent/pull/3729) lands** (new entry point, gRPC on 80, `/readyz` 8081).
- **Nothing to pull for substrate** (last commit 11-13, tag v1.0.1). Pairing unchanged: kagent 1.0.x ↔ substrate v1.0.1 (not "0.6"); `upgrade` still doesn't roll WorkerPool images.
- **IPv6 [#183](https://github.com/kagent-dev/substrate/pull/183)–[#187](https://github.com/kagent-dev/substrate/pull/187)** — ask for a preview image once #185 merges; pre-mirror the Jupyter image for IPv6-only.
- **Untouched chart/security asks:** substrate [#176](https://github.com/kagent-dev/substrate/pull/176), [#174](https://github.com/kagent-dev/substrate/pull/174), [#175](https://github.com/kagent-dev/substrate/pull/175), [#169](https://github.com/kagent-dev/substrate/pull/169), [#180](https://github.com/kagent-dev/substrate/pull/180) (OpenShift SCC); tools [#114](https://github.com/kagent-dev/tools/pull/114) (`TOKEN_PASSTHROUGH` only reaches kubectl), [#116](https://github.com/kagent-dev/tools/pull/116).
- **Multi-tenancy Q&A:** `insecure` still trusts `X-User-Id`, `trusted-proxy` doesn't verify JWTs — cite [#3018](https://github.com/kagent-dev/kagent/pull/3018) + [#3735](https://github.com/kagent-dev/kagent/pull/3735), don't promise a `deny` mode.
- **Stalebot risk:** re-check `stale`-labelled community PRs before demoing on them.
