# kagent-updates - 2026-11-27

**Website-only day: 2 merges in `website` (the CLI-install post now leads with Agent Substrate and carries a video walkthrough), zero releases, zero merged code PRs in kagent or substrate — and substrate [#192](https://github.com/kagent-dev/substrate/pull/192) is still `blocked`, so the non-`ate-system` installer bug remains live on omni-viper.**

Window: since 2026-11-26 (`latest.md`). Today: 2026-11-27 (America/Toronto).

## Releases

**None in window** — no release object published in any org repo since 11-23. Current latest per main repo (all unchanged):

- [kagent v1.0.1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.1) (11-12) — latest stable; preceded by [v1.0.0](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0) and [v1.0.0-rc4](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc4) (both 11-09).
- [kagent v0.10.7](https://github.com/kagent-dev/kagent/releases/tag/v0.10.7) (11-23) — newest overall tag, but 0.10.x is `workflow_dispatch`-only and never flagged "latest".
- [substrate v1.1.0-alpha1](https://github.com/kagent-dev/substrate/releases/tag/v1.1.0-alpha1) (11-23) — pre-release for 1.1 work; [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) (11-08) is still the stable pairing for kagent 1.0.x.
- [kmcp v0.4.0](https://github.com/kagent-dev/kmcp/releases/tag/v0.4.0) (09-17) — unchanged.
- tools / doc2vec / khook / byo-ext / mockllm / community: no releases, no commits in window.

## Highlights

- **The install story now leads with Agent Substrate, not kagent** ([#765](https://github.com/kagent-dev/website/pull/765), merged 11-26). `/blog/install-kagent-with-the-cli` is retitled "Install Agent Substrate in One Command with the kagent CLI" (metadata, H1, blog index card, and the link from the MCP-tasks post). Slug unchanged, so existing links and slides still work — only the title you quote changes. Caveat from the PR: the description and opening paragraph still describe installing kagent.
- **Video walkthrough is live in that post** ([#766](https://github.com/kagent-dev/website/pull/766), merged 11-26, `<YouTube id="jP-n7htbaJU" />` via the existing MDX component). Worth watching once before a demo; it covers the one-liner install end to end. Neither PR ran a build/lint/browser preview.
- **Nothing new to pull for alpha12.** `kagent` `main` is still at [`68b2d19`](https://github.com/kagent-dev/kagent/commit/68b2d19730327661b186576ecd59144a214a1de7) (push signing-key handoff #3782, 11-24) and `substrate` `main` at [`2d0e0bc`](https://github.com/kagent-dev/substrate/commit/2d0e0bc783771390e230b15b89c547ed52841ae4) (#193, 11-23) — third and fourth consecutive idle days. Latest UI state on omni-viper remains the 11-24 form/picker audit.
- **Blocker unchanged.** #192 re-checked today: `open`, `mergeable_state: blocked`, 1 commit, +20/-3 across 3 files, head `d7177b9`, base `fd66419`. Until it merges, installing substrate under any release/namespace other than `ate-system` leaves atelet falling back to `api.ate-system.svc` and **workers never register** — keep the release in `ate-system`, or set `--ateapi-address`/`--ateapi-server-name`.
- **Cold-start install fix, gated.** [#3805](https://github.com/kagent-dev/kagent/pull/3805) (wait for PostgreSQL before migrations, `bug`, +177/-51 over 8 files) retries transient connection failures for 2 min and raises the Helm startup probe to ~10 min readiness — verified against a 60s DB outage with zero restarts. Exactly the fresh-cluster flake we hit in demos; `mergeable_state: blocked` today.
- **Big feature in review:** [#3803](https://github.com/kagent-dev/kagent/pull/3803) *Anthropic Self Hosted Sandboxes* (4 commits, +2899/−3678, 50 files) consumes bash commands outside the sandbox — no more sandbox-lifetime games with Anthropic. Biggest open feature on the board.
- **Windows install gap closing.** [#3806](https://github.com/kagent-dev/kagent/pull/3806) adds `scripts/get-kagent.ps1` (`irm … | iex`, checksum-verified, user-`PATH`, no admin) for the already-shipped `kagent-windows-amd64.exe`. Still open; also flags that the bash script re-downloads a matching release (`1.0.1` vs tag `v1.0.1`).
- **Stale sweep is swallowing community fixes again.** Touched by the bot today at 10:05Z with `stale`: [#3656](https://github.com/kagent-dev/kagent/pull/3656) (validate ask-user answers identically in the Claude and Codex harnesses — a malformed HITL answer no longer kills the session), [#3069](https://github.com/kagent-dev/kagent/pull/3069) (fail the turn on truncated tool-call args instead of running the tool with none — silent wrong behaviour on Azure OpenAI/Mistral/Foundry), [#3068](https://github.com/kagent-dev/kagent/pull/3068) (Postgres test-container leaks). Demo-relevant fixes we may need to carry locally.

## Merged PRs by repo

| repo | PR | title | author | merged |
| --- | --- | --- | --- | --- |
| website | [#766](https://github.com/kagent-dev/website/pull/766) | Blog: embed the video walkthrough in the CLI install post | [sebbycorp](https://github.com/sebbycorp) | 11-26 |
| website | [#765](https://github.com/kagent-dev/website/pull/765) | Blog: retitle the CLI install post to lead with Agent Substrate | [sebbycorp](https://github.com/sebbycorp) | 11-26 |

[All 2 merged in window](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3E%3D2026-11-26&type=pullrequests) — none in kagent, substrate, kmcp, tools, doc2vec, community, khook, byo-ext or mockllm. Supporting commits: [`f58ee9d`](https://github.com/kagent-dev/website/commit/f58ee9d318fd0f9a6403e6ae95a570f1fabcf6d1), [`245d674`](https://github.com/kagent-dev/website/commit/245d67441d52189d430ed04d001c9d42082743fd).

## Watch list (kagent Enterprise alpha12 / Agent Substrate on omni-viper)

- **Pairing unchanged:** alpha12 (kagent 1.0.x) with **[substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1)** — not [v1.1.0-alpha1](https://github.com/kagent-dev/substrate/releases/tag/v1.1.0-alpha1), which only `main` pins.
- **Blocker:** substrate [#192](https://github.com/kagent-dev/substrate/pull/192) — do not rename the substrate release/namespace until it merges.
- **CRD/API changes to test when they land:** substrate [#130](https://github.com/kagent-dev/substrate/pull/130) adds `spec.maxActorsPerWorker` to WorkerPool (forwards `--max-actors`; changing it drains and rolls workers — re-apply the CRD), kagent [#3762](https://github.com/kagent-dev/kagent/pull/3762) EgressPolicy `authentications` and [#3763](https://github.com/kagent-dev/kagent/pull/3763) mixed-case-scheme CEL — all unmerged, so no chart CRD churn today.
- **Security/hardening candidates for omni-viper:** substrate [#131](https://github.com/kagent-dev/substrate/pull/131) namespace-scoped Secret reader (`credentialProvider.rbac.namespaces`), [#169](https://github.com/kagent-dev/substrate/pull/169) RustFS creds from `existingSecret`, [#101](https://github.com/kagent-dev/substrate/pull/101) extra CAs for egress upstream TLS, kagent [#3801](https://github.com/kagent-dev/kagent/pull/3801) reject push auth with no signing key. #131/#169/#101 were all refreshed 11-26 by [QuentinBisson](https://github.com/QuentinBisson) — actively shepherded.
- **Ops/QoL if merged:** substrate [#46](https://github.com/kagent-dev/substrate/pull/46) PodMonitors, [#48](https://github.com/kagent-dev/substrate/pull/48) `podLabels`, [#176](https://github.com/kagent-dev/substrate/pull/176) `image.digests`, [#175](https://github.com/kagent-dev/substrate/pull/175) `gvisorReleaseLocation` (air-gapped), [#174](https://github.com/kagent-dev/substrate/pull/174) `networkPolicies.enabled` — all open.
- **Runtime/API-impacting, open:** kagent [#3791](https://github.com/kagent-dev/kagent/pull/3791) request-scoped MCP tool-call metadata (updated 11-26), [#3786](https://github.com/kagent-dev/kagent/pull/3786) resume actors suspended by reachability checks, [#3764](https://github.com/kagent-dev/kagent/pull/3764) MCP tool-name collisions, [#3795](https://github.com/kagent-dev/kagent/pull/3795) Python BYO over gRPC (0.10.x backport).
- **Carry-or-cherry-pick:** the `stale` HITL/tool-args fixes above plus the runtime cert-trust pair #3321/#3320.
- **Demo asset:** pair the video from [#766](https://github.com/kagent-dev/website/pull/766) with the MCP-tasks post — one "install → long-running task → survives restart" pitch, now titled around Agent Substrate.
