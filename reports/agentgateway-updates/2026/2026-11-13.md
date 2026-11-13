# agentgateway-updates - 2026-11-13

**Still no 1.8 tag — core had exactly one merge (an oss-scanner CI fix), 5 website docs PRs landed, and the action moved to the open side: a prompt-guard CRD reshaping PR (1.23 MB → 1.08 MB) and a per-provider Bedrock Runtime API switch.**

_Window: merged/published `>=2026-11-12` (since the 2026-11-12 report). Active: [agentgateway](https://github.com/agentgateway/agentgateway) (docs/CI only), [website](https://github.com/agentgateway/website); `benchmarks` / `community` not pushed in window._

## Releases

**None published in the window.** Newest artifact remains the Oct 27 cut; everything after v1.7.0 (LLM-IR work, Bedrock fixes, catalog refreshes) is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) | No 1.8 tag; prior rc/alpha were [rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) and [alpha.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-alpha.1) (10-21) |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) "Docs for v1.7.0 release" (10-27) | – | Previous docs tags [v1.6.0](https://github.com/agentgateway/website/releases/tag/v1.6.0), [v1.5.0](https://github.com/agentgateway/website/releases/tag/v1.5.0); ref docs regenerate continuously via bot PRs ([1526](https://github.com/agentgateway/website/pull/1526), [1527](https://github.com/agentgateway/website/pull/1527)) |
| benchmarks · community | None | – | Idle |

## Highlights

- **Watch the prompt-guard CRD shape — it is about to change** — [4387](https://github.com/agentgateway/agentgateway/pull/4387) (open 11-12) moves guardrail `policies` out of the guard nesting to the **top level** of the Backend, and adds CEL validation rules: `policies` rejected on regex/webhook guards, `openAIModeration` allows only `key`/`secretRef`, `bedrockGuardrails` allows `key`/`secretRef`/`aws`, `googleModelArmor` only `gcp`. Stated motive is CRD size: backend CRD `1.23 MB → 1.08 MB` (-10,903 lines of schema), as prep for [4205](https://github.com/agentgateway/agentgateway/pull/4205). **Demo impact:** any saved prompt-guard manifest you paste on stage may need re-writing after this lands — test on `:latest-dev` before the 1.8 cut.
- **Bedrock Codex/tool-call breakage now has a per-provider fix in review** — [4392](https://github.com/agentgateway/agentgateway/pull/4392) (opened 11-13): Bedrock Runtime translates every chat request to `Converse`, which only carries *function* tools, so a Responses request from Codex loses `custom` tools and all tool calls. Today the only escape is global (`AGENTGATEWAY_BEDROCK_RUNTIME_CONVERSE=false` flips every provider at once). The PR adds `bedrockRuntimeApi: native | converse` per provider, defaulting to current behaviour; author's own table shows Codex `codex exec` going from `NONE` to executing the shell command. Caveat: no xDS `AIBackend.Bedrock` field yet, so xDS-configured providers still use the env switch.
- **`llms.txt` said there were two "latest" doc versions** — [1535](https://github.com/agentgateway/website/pull/1535) (merged 11-12): the Kubernetes `1.1.x` landing page had `title: Latest`, so `/llms.txt` and `/docs/llms.txt` listed both 1.1.x and 1.7.x as current — an agent reading it could pick the 4-versions-old docs. Now titled `Version 1.1.x`. If you point agents at agentgateway.dev, re-pull your index.
- **Docs theme v0.5.0: better agent-facing indexes and a Scalar option** — [1536](https://github.com/agentgateway/website/pull/1536) (merged 11-12) bumps `docs-theme-extras` to [v0.5.0](https://github.com/solo-io/docs-theme-extras/blob/v0.5.0/CHANGELOG.md): `/llms.txt` becomes a short index of per-version `llms.txt` links, stray "Note" lines and the "Generated on" timestamp are gone, the `openapi` shortcode can render with **Scalar** (`renderer="scalar"`, Swagger still default), and the `github`/`github-yaml` shortcodes retry dropped connections. Also fixes a Cloudflare redirect bug where the `llm*` rule swallowed each version's `llms.txt`.
- **Generated reference docs had stale links nobody's checker caught** — [1530](https://github.com/agentgateway/website/pull/1530) (merged 11-12): `normalize_doc_links.py` ignored *relative* `aliases:` (most of the site's aliases, because version trees are copied from `latest`), so the old→new URL map had 87 entries instead of 302. Stale URLs still returned 200 via alias stubs. One real fix shipped: `inferenceExtension` in the Helm reference now points at `documentation/llm/inference/` across six Helm pages.
- **Core is effectively idle on main** — `list_commits` since 11-12 returns exactly one commit, [e7cac5d](https://github.com/agentgateway/agentgateway/commit/e7cac5d74cc17e9bab6da94aafa8a0c40d74f8d7) = [4375](https://github.com/agentgateway/agentgateway/pull/4375) `Fix oss-scanner build` (jenshu): the `.oss-scanner` Docker build failed because the root `.dockerignore` excluded `examples/` (missing `examples/mcp-tls/certs/cert.pem`); Go/Rust versions now read from `go.mod` / `rust-toolchain.toml`, arm64 Go supported, Go tests precompiled for offline scanning. Supply-chain/CI hygiene, no runtime effect.
- **Yesterday's items are still the news** — the 68k-line **LLM-IR POC** [4383](https://github.com/agentgateway/agentgateway/pull/4383) (new `agent-llm-ir` crate converting Messages / Responses / Chat Completions / Gemini through one IR, off by default via `AGENTGATEWAY_LLM_IR_ENABLED=true`) and the Okta + Keycloak token-exchange docs [1532](https://github.com/agentgateway/website/pull/1532) / authn reorg [1533](https://github.com/agentgateway/website/pull/1533) (both merged 11-12, re-listed below) remain the two things to know before 1.8.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| website | [1536](https://github.com/agentgateway/website/pull/1536) | Bump docs-theme-extras to v0.5.0 (slim `llms.txt`, Scalar OpenAPI, `/llm` redirect fix) | kristin-kronstain-brown | 11-12 |
| website | [1530](https://github.com/agentgateway/website/pull/1530) | Resolve relative aliases so generated reference links get updated (87 → 302 rules) | kristin-kronstain-brown | 11-12 |
| agentgateway | [4375](https://github.com/agentgateway/agentgateway/pull/4375) | Fix oss-scanner build (dockerignore, toolchain versions, arm64) — only core merge in window | jenshu | 11-12 |
| website | [1535](https://github.com/agentgateway/website/pull/1535) | Fix 1.1.x version title that said `Latest` (duplicate "latest" in llms.txt) | kristin-kronstain-brown | 11-12 |
| website | [1533](https://github.com/agentgateway/website/pull/1533) | authn reorg — security/authentication topics restructured ("based on feedback") | artberger | 11-12 |
| website | [1532](https://github.com/agentgateway/website/pull/1532) | [docs] add more backend token exchange IdPs (Okta, Keycloak) | artberger | 11-12 |

All 6 in-window merges listed (`agentgateway` 1, `website` 5); 1532/1533 also appeared in yesterday's digest. Full list: [merged PRs `>=2026-11-12`](https://github.com/pulls?q=is%3Apr+is%3Amerged+org%3Aagentgateway+merged%3A%3E%3D2026-11-12). No releases published in window.

## Watch list

| Repo | PR | Why | Author |
| --- | --- | --- | --- |
| agentgateway | [4387](https://github.com/agentgateway/agentgateway/pull/4387) | **API/CRD change**: prompt-guard `policies` hoisted to top level + per-type CEL validation; CRD 1.23 → 1.08 MB; breaks saved guardrail manifests | jgreeer |
| agentgateway | [4392](https://github.com/agentgateway/agentgateway/pull/4392) | New `bedrockRuntimeApi: native | converse` param — fixes Codex/Responses custom-tool loss on Bedrock Runtime | Quentin-M |
| agentgateway | [4383](https://github.com/agentgateway/agentgateway/pull/4383) | LLM-IR POC (+67.8k lines, 383 files), env-gated `AGENTGATEWAY_LLM_IR_ENABLED=true` — future home of all format conversion | markuskobler |
| agentgateway | [4394](https://github.com/agentgateway/agentgateway/pull/4394) | New today: `fix(auth)` isolate remote JWKS fetch failures — IdP outage must not fail all authN | shubhamojha1 |
| agentgateway | [4391](https://github.com/agentgateway/agentgateway/pull/4391) | Draft: MCP **Skills extension** support — new MCP surface to demo (pairs with docs draft 1523) | bdubs00 |
| agentgateway | [4390](https://github.com/agentgateway/agentgateway/pull/4390) | feat(aws): CEL expressions for explicit credentials — per-request AWS creds | Sly1029 |
| agentgateway | [4386](https://github.com/agentgateway/agentgateway/pull/4386) | fix(vertex): order tools/system/messages last so prompt cache hits — cost/latency demo material | nklmilojevic |
| agentgateway | [4349](https://github.com/agentgateway/agentgateway/pull/4349) | Streaming token reassembly (data tokenization) — DLP-style token handling in streams | deem0n |
| agentgateway | [4365](https://github.com/agentgateway/agentgateway/pull/4365) · [4353](https://github.com/agentgateway/agentgateway/pull/4353) · [4354](https://github.com/agentgateway/agentgateway/pull/4354) | CEL `recurse()` for nested guardrails; real `ResolvedRefs` / `status.parents` on delegated child routes (Gateway API conformance beats) | ymesika, dongjiang1989 |
| agentgateway | [3637](https://github.com/agentgateway/agentgateway/pull/3637) | Keycloak RFC 9207 issuer mismatch fix, **open since 09-23** — still gates clean Keycloak token-exchange demos | Tazmainiandevil |
| agentgateway | [4377](https://github.com/agentgateway/agentgateway/pull/4377) · [4311](https://github.com/agentgateway/agentgateway/pull/4311) · [4309](https://github.com/agentgateway/agentgateway/pull/4309) | Bot model-catalog refresh; Bedrock local token counting; Mantle `/openai` prefix — check before a Bedrock demo | github-actions[bot], Quentin-M |
| website | [1523](https://github.com/agentgateway/website/pull/1523) · [1478](https://github.com/agentgateway/website/pull/1478) | Draft "OSS assistant skills" guides; Elastic standalone/K8s observability guides still open | jamesilse-solo, haowu1234 |

Nothing to re-test on omni-viper for a tag (no release in window); the prompt-guard CRD shape (4387) and `bedrockRuntimeApi` (4392) are the two `:latest-dev` items worth a spin.
