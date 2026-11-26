# kagent-updates - 2026-11-26

**Quiet day upstream: zero new releases and zero merged PRs in kagent or substrate — only two website merges ([#763](https://github.com/kagent-dev/website/pull/763) MCP-tasks blog post, [#764](https://github.com/kagent-dev/website/pull/764) homepage install bar); substrate [#192](https://github.com/kagent-dev/substrate/pull/192) is still `blocked`, so the non-`ate-system` installer bug remains live on omni-viper.**

Window: since 2026-11-25 (`latest.md`). Today: 2026-11-26 (America/Toronto). Bare PR numbers link via the table.

## Releases

**None in window** — no release object was published in any org repo since 11-23. Current latest per main repo (all unchanged from yesterday):

- [kagent v1.0.1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.1) (11-12) — latest stable 1.0.x; [v1.0.0](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0) (11-09), [v1.0.0-rc4](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc4) (11-09) precede it.
- [kagent v0.10.7](https://github.com/kagent-dev/kagent/releases/tag/v0.10.7) (11-23) — newest overall tag, but 0.10.x is `workflow_dispatch`-only and never marked "latest".
- [substrate v1.1.0-alpha1](https://github.com/kagent-dev/substrate/releases/tag/v1.1.0-alpha1) (11-23, `prerelease: false`) — pre-release for 1.1 work; [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) (11-08) remains the stable pairing for kagent 1.0.x.
- [kmcp v0.4.0](https://github.com/kagent-dev/kmcp/releases/tag/v0.4.0) (09-17) — unchanged.
- tools / doc2vec / khook / byo-ext / mockllm: no new releases and no commits on `main` in window (repos last touched 11-11 or earlier, except kmcp activity 11-24 with no new commits since 11-25).

## Highlights

- **New demo script published: kagent agents as MCP Tasks on Agent Substrate** ([#763](https://github.com/kagent-dev/website/pull/763), merged 11-25 by [sebbycorp](https://github.com/sebbycorp)). Walks `invoke_agent` returning a task handle in ~0.5s for a 2m46s job, `input_required` + `tasks/update` mid-task, `tasks/cancel`, and a task surviving a `kagent-controller` restart — plus why Substrate underneath (sandbox per task, default-deny egress, suspend-when-idle actors). Every command/output verified on **kagent 1.0.1** in kind, so it is directly runnable against the alpha12 build on omni-viper.
- **One-liner install is now the front door** ([#764](https://github.com/kagent-dev/website/pull/764), merged 11-25): hero install bar runs `curl -sL https://kagent.dev/install | bash && kagent install`, and the personal-assistant post dropped its `--version v1.0.0` pin so the CLI pulls latest. Use that command in demos instead of copying raw `get-kagent`.
- **Docs/post rendering fixed** (commits [`f50ee53`](https://github.com/kagent-dev/website/commit/f50ee53dd13bbb203cc173a7c5569fdca718ae4a), [`c589e7d`](https://github.com/kagent-dev/website/commit/c589e7d6612aae2305a6274300dbbee3c8c56641)): fenced code back to `text-sm` (was 18px via `prose-xl`) and the Mermaid sequence diagram replaced with a task hand-off timeline — better slides material for the MCP-tasks story.
- **kagent and substrate `main` have been idle since 11-24.** No commits to kagent `main` after [`68b2d19`](https://github.com/kagent-dev/kagent/commit/68b2d19730327661b186576ecd59144a214a1de7) (push signing-key handoff #3782) and none to substrate `main` after #193 ([`2d0e0bc`](https://github.com/kagent-dev/substrate/commit/2d0e0bc783771390e230b15b89c547ed52841ae4)) — so nothing new to pull for alpha12 today; yesterday's UI audit items (`/mcp-servers`, conversation renames, shared forms) are still the newest UI state.
- **Substrate installer bug unresolved.** #192 re-checked today: still `open` / `mergeable_state: blocked`, 1 commit, +20/-3 across both atelet templates, `base` now at `fd66419`. Until it merges, installing substrate under any release name/namespace other than `ate-system` leaves atelet falling back to `api.ate-system.svc` and **workers never register** — keep the release in `ate-system`, or patch `--ateapi-address`/`--ateapi-server-name`.
- **Reviewer queue is where the next value is, not `main`.** Newly opened since yesterday: [#3803](https://github.com/kagent-dev/kagent/pull/3803) Anthropic self-hosted sandboxes (11-25, by [Stevenjin8](https://github.com/Stevenjin8)), [#3805](https://github.com/kagent-dev/kagent/pull/3805) wait for PostgreSQL before running migrations (11-25, [supreme-gg-gg](https://github.com/supreme-gg-gg)) — a real cold-start install failure mode on fresh clusters, [#3806](https://github.com/kagent-dev/kagent/pull/3806) PowerShell install script for Windows (11-26).
- **Stale sweep is swallowing community fixes.** #3561 (xAI provider), #3537 (custom `baseUrl` host leak), #3321/#3320 (Python/native runtime cert trust), #3301/#3289 (Anthropic streaming/order), #3259 (Vertex AI creds at egress), #2575 and the whole pood1e HITL/agentgateway stack (#3424, #3419, #3416, #3414, #3404, #3403) + #3296 all carry `stale` and were touched 11-25/11-26. Two of them (cert trust) are security fixes we may need to carry locally.
- **Substrate chart PRs churned, none merged** — [#48](https://github.com/kagent-dev/substrate/pull/48) `podLabels`, [#46](https://github.com/kagent-dev/substrate/pull/46) PodMonitors, [#131](https://github.com/kagent-dev/substrate/pull/131) namespace-scoped Secret reader, [#96](https://github.com/kagent-dev/substrate/pull/96) pinned Helm in CI — all updated 11-26 by QuentinBisson, all still open. PodMonitors + scoped Secret reader are the two worth testing on omni-viper if they land.

## Merged PRs by repo

| repo | PR | title | author | merged |
| --- | --- | --- | --- | --- |
| website | [#763](https://github.com/kagent-dev/website/pull/763) | Blog: kagent agents run as MCP tasks on Agent Substrate | [sebbycorp](https://github.com/sebbycorp) | 11-25 |
| website | [#764](https://github.com/kagent-dev/website/pull/764) | Homepage: install command bar in the hero | [sebbycorp](https://github.com/sebbycorp) | 11-25 |

[All 2 merged in window](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3E%3D2026-11-25&type=pullrequests); none in kagent, substrate, kmcp, tools, doc2vec, community, khook, byo-ext, mockllm.

## Watch list (kagent Enterprise alpha12 / Agent Substrate on omni-viper)

- **Pairing unchanged:** alpha12 (kagent 1.0.x) pairs with **[substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1)**, not [v1.1.0-alpha1](https://github.com/kagent-dev/substrate/releases/tag/v1.1.0-alpha1) (only `main` pins the alpha, via kagent #3754 [`dedcef2`](https://github.com/kagent-dev/kagent/commit/dedcef228d064eaf595c7a02e72c0610b39167f5)).
- **Blocker:** substrate [#192](https://github.com/kagent-dev/substrate/pull/192) — do not rename the substrate release/namespace on omni-viper until it merges.
- **Test on omni-viper when merged:** [#3805](https://github.com/kagent-dev/kagent/pull/3805) Postgres-before-migrations (fresh installs), [#3786](https://github.com/kagent-dev/kagent/pull/3786) resume actors suspended by reachability checks (updated 11-26), [#3801](https://github.com/kagent-dev/kagent/pull/3801) reject push auth with no signing key, [#3764](https://github.com/kagent-dev/kagent/pull/3764) MCP tool-name collisions, [#3795](https://github.com/kagent-dev/kagent/pull/3795) Python BYO over gRPC backport to 0.10.x.
- **A2A/API-impacting, open:** [#3791](https://github.com/kagent-dev/kagent/pull/3791) request-scoped MCP tool-call metadata (updated 11-26), plus [#3500](https://github.com/kagent-dev/kagent/pull/3500)/[#3501](https://github.com/kagent-dev/kagent/pull/3501) MCP URL elicitations surfaced as `input_required` — good fit with the MCP Tasks post if they land.
- **Carry-or-cherry-pick candidates:** [#3321](https://github.com/kagent-dev/kagent/pull/3321)/[#3320](https://github.com/kagent-dev/kagent/pull/3320) runtime certificate trust refresh (labelled `bug`, now `stale`).
- **Re-apply CRDs** once #3762/#3763 (EgressPolicy `authentications`, mixed-case-scheme CEL) merge; still unmerged, so no chart CRD churn today.
- **New demo asset:** the MCP-tasks post from [#763](https://github.com/kagent-dev/website/pull/763) — pair it with the new hero install one-liner from [#764](https://github.com/kagent-dev/website/pull/764) for a two-minute "install → long-running task → survives restart" pitch.
