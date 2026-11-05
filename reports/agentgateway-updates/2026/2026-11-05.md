# agentgateway-updates - 2026-11-05

Busy 24h: custom model-source plugin points merged, a new compression policy and "UI under any path prefix" landed, Bedrock moved to native Runtime + Converse fallback — **15 PRs merged across 2 repos**, still no release since v1.7.0.

_Window: merged PRs `merged:>=2026-11-04T12:10Z` (since the 2026-11-04 report). Pushed: [agentgateway](https://github.com/agentgateway/agentgateway), [website](https://github.com/agentgateway/website); `benchmarks`/`community` idle._

## Releases

**None published in the window:**

| Repo | Latest stable | Latest pre-release | Notes |
| --- | --- | --- | --- |
| [agentgateway](https://github.com/agentgateway/agentgateway/releases) | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (2026-10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (2026-10-26) | No 1.8 tag — below is nightly/`latest` only |
| [website](https://github.com/agentgateway/website/releases) | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (2026-10-27) | – | No new docs cut |
| benchmarks · community | None | – | Idle |

## Highlights

- **Pluggable model sources are in** — [#4292](https://github.com/agentgateway/agentgateway/pull/4292) adds controller plugin points for a custom model source, mirroring the custom-policy/custom-backend plugin APIs (13 files, +283/−103). Follow-up [#4301](https://github.com/agentgateway/agentgateway/pull/4301), merged hours later, **flipped** those semantics: selectors (Gateway `allowedListeners`, HTTPRoute backendRef) allow *all* models if any is enabled, but a **direct reference from a virtual model now needs an explicit `kind`**. Demo impact: an in-house model catalog can be a plugin, not a fork — saved YAML may need the new `kind`.
- **New `compression` policy** — [#4178](https://github.com/agentgateway/agentgateway/pull/4178), 23 files (+3019/−965): first-class response-compression config, rescued from stalled #2781. New user-facing config surface — check the YAML shape before demoing.
- **UI behind a path prefix** — [#4266](https://github.com/agentgateway/agentgateway/pull/4266) adds `config.uiBasePath` / `UI_BASE_PATH` (Grafana `serve_from_sub_path` style): `<base href>`, relative Vite assets, prefixed redirects, OIDC at `{uiBasePath}/ui/login`. Ends the per-prefix build workaround; unblocks the shared `/team/agentgateway` proxy topology. OIDC untested against a real IdP.
- **Bedrock now native-API-first** — [#4298](https://github.com/agentgateway/agentgateway/pull/4298) keeps Converse only for translations native APIs lack (Responses→Claude) and drops `web_search` / `oauth-*` betas on Messages, which native Runtime rejects. Plus [#4299](https://github.com/agentgateway/agentgateway/pull/4299) (missing AWS field) and [#4297](https://github.com/agentgateway/agentgateway/pull/4297) (Bedrock **invoke metrics**).
- **Breaking change queued** — open [#4300](https://github.com/agentgateway/agentgateway/pull/4300) `(breaking) llm: default to native endpoints on AWS`: flips off `converse` to `/chat/completions`, `/messages`, `/responses`; flag stays, **different IAM role**.
- **DNS** — [#4302](https://github.com/agentgateway/agentgateway/pull/4302) disables the Hickory DNS cache (pre-req for #2810); `maxTTL` [#4304](https://github.com/agentgateway/agentgateway/pull/4304) still open, so expect higher DNS QPS meanwhile.
- **Windows standalone works now** — [#4264](https://github.com/agentgateway/agentgateway/pull/4264) resolves home via `USERPROFILE`/`HOMEDRIVE`+`HOMEPATH`; [#4276](https://github.com/agentgateway/agentgateway/pull/4276) fixes `--version` printing `True`.
- **Catalog churn + metrics docs** — [#4293](https://github.com/agentgateway/agentgateway/pull/4293) merged (adds endpoints, e.g. `/embeddings` on Nebius `Qwen/Qwen3-Embedding-8B`); bump [#4306](https://github.com/agentgateway/agentgateway/pull/4306) open — don't hardcode model IDs. New per-`backend` gauges `agentgateway_requests_active` / `agentgateway_upstream_connections_active` documented in [#1450](https://github.com/agentgateway/website/pull/1450) — ready-made capacity slide.

## Merged PRs by repo

### agentgateway (12)

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [#4178](https://github.com/agentgateway/agentgateway/pull/4178) | feat: add compression policy | makavity | 2026-11-04 |
| agentgateway | [#4266](https://github.com/agentgateway/agentgateway/pull/4266) | feat(ui): serve the UI under any path prefix | liorbenita2 | 2026-11-04 |
| agentgateway | [#4292](https://github.com/agentgateway/agentgateway/pull/4292) | controller: plugin points for custom model source | howardjohn | 2026-11-04 |
| agentgateway | [#4301](https://github.com/agentgateway/agentgateway/pull/4301) | controller: flip semantics on custom models | howardjohn | 2026-11-04 |
| agentgateway | [#4298](https://github.com/agentgateway/agentgateway/pull/4298) | bedrock: fall back to Converse on native Runtime | howardjohn | 2026-11-04 |
| agentgateway | [#4297](https://github.com/agentgateway/agentgateway/pull/4297) | llm: support invoke metrics on bedrock | howardjohn | 2026-11-04 |
| agentgateway | [#4302](https://github.com/agentgateway/agentgateway/pull/4302) | Disable Hickory DNS cache | howardjohn | 2026-11-04 |

+5 more: [#4264](https://github.com/agentgateway/agentgateway/pull/4264) Windows home dir, [#4299](https://github.com/agentgateway/agentgateway/pull/4299) bedrock field, [#4276](https://github.com/agentgateway/agentgateway/pull/4276) Win `--version`, [#4296](https://github.com/agentgateway/agentgateway/pull/4296) config-API docs, [#4293](https://github.com/agentgateway/agentgateway/pull/4293) catalog — [all merged](https://github.com/agentgateway/agentgateway/pulls?q=is%3Apr+is%3Amerged)

### website (3)

| Repo | PR # | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| website | [#1450](https://github.com/agentgateway/website/pull/1450) | [Automated] Draft docs: upstream req/conns metrics | github-actions[bot] | 2026-11-04 |
| website | [#1468](https://github.com/agentgateway/website/pull/1468) | Replace Hextra favicons/manifest with branding | kristin-kronstain-brown | 2026-11-04 |
| website | [#1469](https://github.com/agentgateway/website/pull/1469) | Link fixes | kristin-kronstain-brown | 2026-11-05 |

### benchmarks · community

None in window.

## Watch list

- **Pre-test the AWS native-endpoint flip** [#4300](https://github.com/agentgateway/agentgateway/pull/4300) (open, blocked): update the demo's Bedrock IAM policy, re-run streaming + tool-calling; `web_search`/`oauth-*` betas gone on Messages.
- **Open today**: `mcp: respect timeout on stdio requests` [#4305](https://github.com/agentgateway/agentgateway/pull/4305) (hanging stdio servers finally time out).
- **Unreleased, needs nightly/`latest`** (no 1.8 RC): CEL `upstreamCalls` [#4044](https://github.com/agentgateway/agentgateway/pull/4044); direct-response split [#4250](https://github.com/agentgateway/agentgateway/pull/4250) — breaks filters keyed on `reason=DirectResponse`, MCP ext_authz deny 500→403; `ultrafast` tier [#4259](https://github.com/agentgateway/agentgateway/pull/4259); nested virtual models [#4216](https://github.com/agentgateway/agentgateway/pull/4216); backend TLS fail-closed [#4198](https://github.com/agentgateway/agentgateway/pull/4198); `AgentgatewayModel` flag dropped [#4280](https://github.com/agentgateway/agentgateway/pull/4280).
- **Still open**: budget by `agentgateway.dev/id` [#4284](https://github.com/agentgateway/agentgateway/pull/4284), GCP tokens in `backendAuth` [#4279](https://github.com/agentgateway/agentgateway/pull/4279), gRPC local errors as `Unavailable` [#4226](https://github.com/agentgateway/agentgateway/pull/4226), empty Chat Completions should fail [#4128](https://github.com/agentgateway/agentgateway/pull/4128) — +more: [open PRs](https://github.com/pulls?q=is%3Apr+is%3Aopen+org%3Aagentgateway).
- **Stale demo script**: retries follow `spec.frontend.http.maxBufferSize` (2 MiB default, 32 MiB LLM) per [website#1463](https://github.com/agentgateway/website/pull/1463) — re-shoot the "large requests aren't retried" segment.
