# kagent-updates - 2026-11-25

**Three releases in one day (kagent v0.10.7, tools v0.3.2, substrate v1.1.0-alpha1 published as a release object); 19 PRs merged org-wide, headlined by a kagent UI wave (forms overhaul, `/mcp` rename, breaking chat→conversation rename) and doc2vec's Zendesk OAuth merge — but substrate #192 is still blocked, so the non-`ate-system` installer bug is live on omni-viper.**

Window: since 2026-11-23 (`latest.md`). Today: 2026-11-25 (America/Toronto). Bare PR numbers link via the table.

## Releases

- **[kagent v0.10.7](https://github.com/kagent-dev/kagent/releases/tag/v0.10.7)** (11-23) — 0.10.x CVE release: mcp 1.30.0 + kagent-tools 0.3.2 ([#3779](https://github.com/kagent-dev/kagent/pull/3779)), image CVE fixes ([#3738](https://github.com/kagent-dev/kagent/pull/3738)), ModelConfig retry ([#3546](https://github.com/kagent-dev/kagent/pull/3546)), prompts namespace ([#3407](https://github.com/kagent-dev/kagent/pull/3407)). **Correction to yesterday:** v0.10.7 now exists; 0.10.x is `workflow_dispatch`-only, never marked "latest" ([#3567](https://github.com/kagent-dev/kagent/pull/3567)).
- **[tools v0.3.2](https://github.com/kagent-dev/tools/releases/tag/v0.3.2)** (11-23) — helm 4.3.2 + cilium-cli 0.20.3 via [tools#118](https://github.com/kagent-dev/tools/pull/118): fixes CVE-2026-111943/-116587/-116591 in the `tools` image.
- **[substrate v1.1.0-alpha1](https://github.com/kagent-dev/substrate/releases/tag/v1.1.0-alpha1)** — published 11-23 with `prerelease: false`, so it reads as stable; its notes omitted the `substrate-atelet` chart (#193).
- **Unchanged:** [kagent v1.0.1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.1), [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1), [kmcp v0.4.0](https://github.com/kagent-dev/kmcp/releases/tag/v0.4.0), [doc2vec v2.17.5](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.5) — no kagent 1.0.x release in window.

## Highlights

- **mcp 1.30 changes behaviour, not just CVEs** — per the v0.10.7 notes, a `RemoteMCPServer` whose URL **redirects to another host/port now fails to connect**; point the URL at the target. (CVE-2026-115563: OAuth credentials could reach a server-chosen authorization server.)
- **Two breaking UI renames.** `/mcp` → `/mcp-servers` (#3778): `/mcp` was both the page and the controller's MCP endpoint, nginx sent it to the controller, so reload gave `405` (MCP clients still use `/mcp`). And chat/session → **"conversation"** (#3776): `/agents/:id/chat` **no longer resolves**; rename `chatPartRenderers`, `agentLinks.chat`, `newChat`, `agentChat`/`chatRow`, `sessionId`→`conversationId`.
- **One shared UI form system (most demo-visible, #3705)** — all eight create/edit forms share validation, hints, toasts and the unsaved-changes prompt; free-text refs became searchable pickers. This is what the Enterprise alpha UI will look like.
- **MCP tool-result images render inline** (#3246) — Grafana `get_panel_image` shows the PNG, not `[MCP image: mimeType=…]`; PR ships a Prometheus+Grafana manifest. New demo beat.
- **Push signing-key rotation survives restarts** (#3782) — optional previous key via Helm Secret or CSI file keeps replicas signing through the activation delay.
- **doc2vec #149 merged** (draft yesterday): Zendesk OAuth `client_credentials`, tickets via incremental cursor export. `email`+`api_token` still work, but no new tokens since 2026-10-27 and all dead **2027-04-30** — move Zendesk RAG demos to OAuth now.
- **kagent.dev docs fixed, now agent-readable** — unstyled pages on repeat visits came from the worker reading ASSETS `304` as a miss (#754); #752 adds content-before-nav, `.md` twins, per-section `llms.txt`, `Accept: text/markdown`.
- **Substrate idle again:** `main` got only #193 ([`2d0e0bc`](https://github.com/kagent-dev/substrate/commit/2d0e0bc783771390e230b15b89c547ed52841ae4)), and [#192](https://github.com/kagent-dev/substrate/pull/192) (atelet hard-codes `api.ate-system.svc` → **workers never register under any other release name/namespace**) is still `blocked`. Yesterday's blocked queue unmoved: #3762 EgressPolicy `authentications` (no UI), #3763 mixed-case-scheme CEL, #3764 MCP tool collisions, #3765 telemetry flush.

## Merged PRs by repo

| repo | PR | title | author | merged |
| --- | --- | --- | --- | --- |
| kagent | [#3705](https://github.com/kagent-dev/kagent/pull/3705) | shared UI forms | alan-bentley-pgh | 11-24 |
| kagent | [#3776](https://github.com/kagent-dev/kagent/pull/3776) | chats→conversations | Charlesthebird | 11-24 |
| kagent | [#3246](https://github.com/kagent-dev/kagent/pull/3246) | MCP images in chat | toreysoloio | 11-23 |
| kagent | [#3782](https://github.com/kagent-dev/kagent/pull/3782) | push key handoff | shashankram | 11-24 |
| kagent | [#3778](https://github.com/kagent-dev/kagent/pull/3778) | /mcp-servers route | cjlg-soloio | 11-23 |
| kagent | [#3781](https://github.com/kagent-dev/kagent/pull/3781) | tools 0.3.2 main | danielle-mustillo | 11-23 |
| kagent | [#3779](https://github.com/kagent-dev/kagent/pull/3779) | mcp 1.30, v0.10.7 | danielle-mustillo | 11-23 |
| kagent | [#3780](https://github.com/kagent-dev/kagent/pull/3780) | modal delete confirm | Charlesthebird | 11-24 |
| kagent | [#3777](https://github.com/kagent-dev/kagent/pull/3777) | sidebar highlight | Charlesthebird | 11-24 |
| doc2vec | [#149](https://github.com/kagent-dev/doc2vec/pull/149) | zendesk OAuth | jmunozro | 11-23 |
| substrate | [#193](https://github.com/kagent-dev/substrate/pull/193) | atelet in notes | iplay88keys | 11-23 |
| website | [#752](https://github.com/kagent-dev/website/pull/752) | agent readiness | kristin-kronstain-brown | 11-23 |
| website | [#754](https://github.com/kagent-dev/website/pull/754) | docs 304 fix | kristin-kronstain-brown | 11-23 |
| website | [#758](https://github.com/kagent-dev/website/pull/758) | assistant+CVE blogs | sebbycorp | 11-24 |
| website | [#759](https://github.com/kagent-dev/website/pull/759) | CLI install post | sebbycorp | 11-24 |
| website | [#760](https://github.com/kagent-dev/website/pull/760) | blog card grid | sebbycorp | 11-24 |
| website | [#761](https://github.com/kagent-dev/website/pull/761) | egress UI post | sebbycorp | 11-24 |
| website | [#762](https://github.com/kagent-dev/website/pull/762) | blog category fix | artberger | 11-24 |
| website | [#757](https://github.com/kagent-dev/website/pull/757) | Policies in demo | sebbycorp | 11-23 |

[All 19](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3E%3D2026-11-23&type=pullrequests); none in kmcp, tools, community, khook, byo-ext, mockllm.

## Watch list (kagent Enterprise alpha12 / Agent Substrate on omni-viper)

- **Pairing unchanged:** alpha12 (kagent 1.0.x) pairs with **substrate v1.0.1**, not [v1.1.0-alpha1](https://github.com/kagent-dev/substrate/releases/tag/v1.1.0-alpha1) (`main` pins it; tip `2d0e0bc`).
- **Substrate #192** still `blocked` — keep the release in `ate-system`, or patch `--ateapi-address`/`--ateapi-server-name` on both atelet templates.
- **Pin kagent-tools 0.3.2** on `main`/0.10.x builds; audit `RemoteMCPServer` URLs for cross-host redirects given mcp 1.30.0.
- **UI re-audit before demoing a `main` build:** `/mcp-servers`, conversation URLs + extension renames, new form/picker flow, modal deletes. Alpha12's shipped UI predates all of it.
- **Open, worth testing:** [#3786](https://github.com/kagent-dev/kagent/pull/3786) resume actors suspended by reachability checks, [#3801](https://github.com/kagent-dev/kagent/pull/3801) reject push auth with no signing key, [#3798](https://github.com/kagent-dev/kagent/pull/3798) checkpoint purge on idle expiry, [#3795](https://github.com/kagent-dev/kagent/pull/3795) Python BYO over gRPC backport, [#3712](https://github.com/kagent-dev/kagent/pull/3712) harness + MCP details pages.
- **A2A hardening, API-impacting:** [#3797](https://github.com/kagent-dev/kagent/pull/3797) `A2A-Version`, [#3794](https://github.com/kagent-dev/kagent/pull/3794) conceal inaccessible tasks, [#3793](https://github.com/kagent-dev/kagent/pull/3793) bearer auth on trusted-proxy cards, [#3791](https://github.com/kagent-dev/kagent/pull/3791) A2A metadata → MCP `_meta`.
- **Re-apply CRDs when #3762/#3763 merge.** Stale sweep 11-25 hit the pood1e HITL/agentgateway stack (#3419, #3416, #3404, #3403) plus #3296, #3572.
- **New demo assets:** [CVE agent in gVisor, default-deny egress](https://kagent.dev/blog/kagent-cve-agent-substrate-secure-sandbox), [personal assistant on Agent Substrate](https://kagent.dev/blog/personal-ai-assistant-kagent-agent-substrate), [one-command CLI install](https://kagent.dev/blog/install-kagent-with-the-cli).
