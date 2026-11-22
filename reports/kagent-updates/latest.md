# kagent-updates - 2026-11-22

**Quiet day: zero releases and one merged PR org-wide (kagent [#3407](https://github.com/kagent-dev/kagent/pull/3407), prompts-page namespace fix on `release/v0.10.x`); substrate `main` still idle at `fd66419` — but three new kagent PRs opened today all touch CRD/API surface (EgressPolicy `authentications`, RemoteMCPServer TLS CEL, MCP tool disambiguation).**

Window: since 2026-11-21 (`latest.md`). Today: 2026-11-22 (America/Toronto).

## Releases

- **None published in window**, in any repo. Latest stable unchanged: [kagent v1.0.1](https://github.com/kagent-dev/kagent/releases/tag/v1.0.1), [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1), [tools v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1), [kmcp v0.4.0](https://github.com/kagent-dev/kmcp/releases/tag/v0.4.0), doc2vec v2.17.5.
- **Latest pre-release:** substrate **`v1.1.0-alpha1`** ([tags](https://github.com/kagent-dev/substrate/tags)), still at [`fd66419`](https://github.com/kagent-dev/substrate/commit/fd66419e10fddc6209ee2c4c4135fafbb17fe845), the 11-19 OpenShift SCC merge. No new substrate tag since.
- **Branch tips:** kagent `main` = [`dedcef2`](https://github.com/kagent-dev/kagent/commit/dedcef228d064eaf595c7a02e72c0610b39167f5) (Substrate alpha1 pin), `release/v1.0.x` = [`c86529b`](https://github.com/kagent-dev/kagent/commit/c86529b07a780ac765e432509c0ca9a74cfddd3c) (nginx 1.31.9), `release/v0.10.x` advanced to [`ce8de68`](https://github.com/kagent-dev/kagent/commit/ce8de683ebe261247f0a6bbeb817ab55f5d44d96). substrate, tools, website, kmcp, doc2vec: no commits in window.

## Highlights

- **The only merge, and it is a demo-quality UI fix:** [#3407](https://github.com/kagent-dev/kagent/pull/3407) ([`ce8de68`](https://github.com/kagent-dev/kagent/commit/ce8de683ebe261247f0a6bbeb817ab55f5d44d96), merged 11-21, label `bug`) — `/prompts` no longer hard-codes namespace `kagent`. The UI Deployment injects `KAGENT_NAMESPACE` from `metadata.namespace` (downward API), so `namespaceOverride` and a non-`kagent` `--namespace` both work; `getUiRuntimeConfig()` gained `defaultNamespace`. A failed listing now renders an inline `Alert` instead of a full-page `ErrorState`, so the namespace selector stays usable. Fixes #3025; UI-only plus one env var.
- **CRD change in flight — EgressPolicy:** [#3762](https://github.com/kagent-dev/kagent/pull/3762) (`enhancement`, opened 11-22, +260/−40 across 8 files) adds an `authentications` **array** per destination, mutually exclusive with `authentication` (enforced by CEL), with `authentication` documented as deprecated. Implements #3714. No UI support yet. If it merges, your EgressPolicy YAML needs migrating.
- **CRD validation hole — RemoteMCPServer TLS:** [#3763](https://github.com/kagent-dev/kagent/pull/3763) (`bug`, opened 11-22) — the CEL rule rejected `spec.tls` only for a lowercase `http://` prefix, so `HTTP://` and `Http://` were admitted with TLS configured while traffic still went over plaintext. Fix lowercases the scheme check and regenerates the shipped CRD. Fixes #3757. Check any customer manifest using a mixed-case scheme.
- **Multi-MCP tool collisions:** [#3764](https://github.com/kagent-dev/kagent/pull/3764) (`bug`, opened 11-22) — disambiguates tools from multiple MCP servers; matters for any demo with several `RemoteMCPServer`s behind one agent.
- **Claude Code runtime stall:** [#3760](https://github.com/kagent-dev/kagent/pull/3760) (`bug`, opened 11-21) — drain stdout while event delivery stalls, directly in the Claude-harness demo path.
- **Docs queueing behind features:** four automated draft-docs PRs opened 11-22 on website — [#750](https://github.com/kagent-dev/website/pull/750) (push signing-key hot reload), [#749](https://github.com/kagent-dev/website/pull/749) ("ship an example Agent and skills editor"), [#748](https://github.com/kagent-dev/website/pull/748) (Python BYO over A2A gRPC), [#747](https://github.com/kagent-dev/website/pull/747) (helm metrics reader RBAC). [#749](https://github.com/kagent-dev/website/pull/749) hints an example-Agent/skills-editor feature is landing in `main`.
- **Still branch-only (no tag):** yesterday's nginx 1.31.9 backport (`c86529b`) and the 0.10.x image CVE fixes (`f82ac99`) have no release object. Note 0.10.x releases are now started by `workflow_dispatch` and never marked GitHub "latest" ([`9f0c4f8`](https://github.com/kagent-dev/kagent/commit/9f0c4f875ad4f583a1ff749d89a4689261ea7f5c)), so a v0.10.7 may lag.
- **Housekeeping:** kagent [#3540](https://github.com/kagent-dev/kagent/pull/3540) and [#3528](https://github.com/kagent-dev/kagent/pull/3528) were labelled `stale` on 11-21.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3407](https://github.com/kagent-dev/kagent/pull/3407) | fix(ui): use the install namespace on the prompts page (backport to `release/v0.10.x`) | rogan297 | 11-21 |

One merged PR org-wide in the window ([search](https://github.com/search?q=org%3Akagent-dev+is%3Apr+is%3Amerged+merged%3E%3D2026-11-21&type=pullrequests)). Nothing merged in substrate, tools, website, kmcp or doc2vec.

## Watch list (kagent Enterprise alpha12 / Agent Substrate on omni-viper)

- **Pairing unchanged:** alpha12 (kagent 1.0.x) still pairs with substrate **v1.0.1** — not `v1.1.0-alpha1`, which `main` now pins. Substrate `main` has had no commits since 11-19, so the OpenShift SCC fix ([#180](https://github.com/kagent-dev/substrate/pull/180), at [`fd66419`](https://github.com/kagent-dev/substrate/commit/fd66419e10fddc6209ee2c4c4135fafbb17fe845)) is unreleased and the manual SCC grant is still needed on omni-viper.
- **Test on merge, kagent open:** [#3762](https://github.com/kagent-dev/kagent/pull/3762) EgressPolicy `authentications` (deprecates `authentication`; no UI), [#3763](https://github.com/kagent-dev/kagent/pull/3763) regenerated `RemoteMCPServer` CEL — re-apply existing manifests, [#3764](https://github.com/kagent-dev/kagent/pull/3764) MCP tool disambiguation, [#3760](https://github.com/kagent-dev/kagent/pull/3760) Claude stdout drain, [#3192](https://github.com/kagent-dev/kagent/pull/3192) read-only toolchain mounts (updated 11-22), [#3510](https://github.com/kagent-dev/kagent/pull/3510) Helm 4.2 OCI output parsing, [#3750](https://github.com/kagent-dev/kagent/pull/3750) read-only `GITHUB_TOKEN`.
- **Docs ahead of your build:** sandboxes docs and the Claude-managed-agents page remain `main`-only; the four draft-docs PRs above are not merged, so verify against alpha12 before quoting in a deck.
- **doc2vec:** draft [#149](https://github.com/kagent-dev/doc2vec/pull/149) (Zendesk OAuth client credentials) updated 11-22 — still draft, no new release.
