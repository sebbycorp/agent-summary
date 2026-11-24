# agentgateway-updates - 2026-11-24

**The code repo finally moved: 13 PRs merged into `agentgateway` in ~12 hours (`main` now [`a30bf3e`](https://github.com/agentgateway/agentgateway/commit/a30bf3e771d2310fde8038caaa9382444110ed77), 11-24T03:49Z) — multi-endpoint standalone MCP ([#4473](https://github.com/agentgateway/agentgateway/pull/4473)), an in-tree SSE decoder replacing abandoned `tokio_sse_codec` ([#4502](https://github.com/agentgateway/agentgateway/pull/4502)), Responses→Chat-Completions image/file translation ([#4529](https://github.com/agentgateway/agentgateway/pull/4529)), the Claude-Code-on-Bedrock beta-header fix ([#4420](https://github.com/agentgateway/agentgateway/pull/4420)), plus strict `backendAuth` parsing (soft breaking). Still no v1.8 tag; the release-note roll-up is up to ~28 entries.**

_Window: since the 2026-11-23 digest (closed 11-22T13:00Z). 15 merged PRs: 13 in `agentgateway`, 2 in `website`. `benchmarks` and `community` idle._

## Releases

**None published** in window. Cut stays Oct 27; everything since is nightly / `:latest-dev`.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | - |

No `v1.8.0-alpha.1` yet. The rolling release-note PR [website#1586](https://github.com/agentgateway/website/pull/1586) grew overnight (still open, `behind`) to ~28 entries: file-backed backend mTLS + CEL actor-chain recursion (#3872/#4340/#4365/#4431/#4461), MCP Skills (#4214), Gemini Decisions provider (#4402), `basicAuth` bcrypt cost cap (#4405), oauth token-exchange cache inputs (#4468), `gen_ai.response.id` (#4483), model discovery in logs (#4500), MCP session resume on GET (#4437), lb active-endpoint preference (#4497).

## Highlights

- **One gateway, many MCP endpoints — standalone** ([#4473](https://github.com/agentgateway/agentgateway/pull/4473), howardjohn, 11-23T22:22Z). New `mcp.endpoints` serves several server sets under their own paths (`/mcp`, `/sales/mcp`), each with own `targets`, `statefulMode` override and policies; an endpoint policy replaces the shared one of the same type, except `authorization`/`mcpAuthorization` where both apply. Paths can't overlap each other, `/.well-known`, or UI/LLM paths. Config rows gain a `<path>:<name>` id; UI groups servers by endpoint and the playground picks an endpoint. **This is the department/tenant-isolation MCP story that was previously only credible in Kubernetes mode.**
- **Streaming reliability: SSE decoder now in-tree** ([#4502](https://github.com/agentgateway/agentgateway/pull/4502), markuskobler). `tokio_sse_codec` hadn't shipped since `0.0.2` (Aug 2023): it merged events when a CRLF straddled reads, rejected bare-CR endings, killed the stream after one oversized event, and logged unknown field values. The replacement takes LF/CR/CRLF however reads split, *skips* an oversized event instead of closing, keeps passthrough forwarding, and **stops resetting streaming guardrails mid-event** — re-test guardrails on long streamed answers.
- **Multimodal no longer silently dropped** ([#4529](https://github.com/agentgateway/agentgateway/pull/4529), markuskobler, builds on #4239/#4504). On Responses→Chat-Completions backends (Gemini, Vertex, completions-only), `input_image`→`image_url` (keeps `detail` + cache breakpoint), `input_file`→`file` from `file_data`/`file_id`/data URL; Gemini/Vertex get files as `image_url` data URLs (Vertex forced to `high`). Images/files in system/developer/assistant roles now **error instead of vanishing**. Limit: OpenAI Chat Completions rejects a file plus a cache-breakpoint image in one message.
- **Claude Code on Bedrock stops 400-ing** ([#4420](https://github.com/agentgateway/agentgateway/pull/4420), Quentin-M). `advanced-tool-use-2025-11-20` (unknown to Bedrock) is rewritten to `tool-search-tool-2025-10-19` + `tool-examples-2025-10-29`, deduped; 8 betas added to the allowlist (`compact-2026-09-04`, `computer-use-2024-10-22`, `fallback-credit-2026-07-01`, `inline-tools-2026-09-15`, `mid-conversation-system-clear-at-2026-08-21`, `model-context-window-exceeded-2025-08-26`, two thinking ones). Probed live on Runtime + Mantle across five Claudes. Native requests still skip the allowlist unless `AGENTGATEWAY_BEDROCK_ANTHROPIC_BETA_HEADERS` is set.
- **Soft breaking: `backendAuth` is now strict** ([#4514](https://github.com/agentgateway/agentgateway/pull/4514), ymesika). Short-form `key:` + sibling `credentials:` silently dropped the extra credentials (config validated, second header never sent) — fixed. `BackendAuthCompat` also rejects unknown fields, so a typo like `credentails` that used to load **with no key at all** now fails at startup. **Audit demo manifests before the next deploy.**
- **HBONE CONNECT resolves the Service host** ([#4516](https://github.com/agentgateway/agentgateway/pull/4516), rajatvig, fixes #4515). `HboneAddress::socket_addr` → `gateway_socket_addr`, so the CONNECT listener binds the Service host not the pod IP; verified in `kind` + Istio. Re-test ambient-style routing on omni-viper.
- **InferencePool/Service routing keeps provider health** ([#4469](https://github.com/agentgateway/agentgateway/pull/4469), danehans, fixes #4465). Provider and endpoint outcomes recorded independently (incl. fail-open success) instead of lost when endpoint selection fails — matters for Gateway API Inference Extension demos.
- **Guardrails / config-load hardening**: request-phase coverage for async `tasks/*` ([#4493](https://github.com/agentgateway/agentgateway/pull/4493), stevenctl — guardrail impls must map tool ID → task ID themselves); unusable OpenAPI MCP ops (no `operationId`, cookie params) now fail at config load with the target name instead of at first `initialize` ([#4511](https://github.com/agentgateway/agentgateway/pull/4511), jamesilse). Also: stale budget-window writes ignored ([#4503](https://github.com/agentgateway/agentgateway/pull/4503)), file-backed `mcp.settings` endpoint rows appear in `/api/config/resources` ([#4527](https://github.com/agentgateway/agentgateway/pull/4527)), standalone schema kinds documented ([#4167](https://github.com/agentgateway/agentgateway/pull/4167)).
- **Model catalog refreshed** ([#4509](https://github.com/agentgateway/agentgateway/pull/4509), bot): `tencent/Hy4` (deepinfra, huggingface; 1M ctx), `genie-4` on Gemini (video/audio out), `writer.palmyra-vision-7b` on Bedrock, `nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B` on Nebius. Daily follow-up [#4542](https://github.com/agentgateway/agentgateway/pull/4542) open.
- **Docs site: agent-readable + lighter** ([website#1604](https://github.com/agentgateway/website/pull/1604) pins `docs-theme-extras` v0.6.0 so `<article>` precedes the sidebar — agents no longer chew 20-80K of nav before content — and [website#1587](https://github.com/agentgateway/website/pull/1587) moves the chatbot @-mention index into one fingerprinted file: `/docs/kubernetes/latest/` 628 KB → 289 KB).
- **Also open/next**: a retry policy for callouts ([#4544](https://github.com/agentgateway/agentgateway/pull/4544), ymesika) is a new ext-callout resilience knob; streaming token reassembly for data tokenization ([#4349](https://github.com/agentgateway/agentgateway/pull/4349)) is still unreleased.
- **No new CVEs against org-owned code in window.** Nearest security-relevant merges: strict `backendAuth` (#4514), OpenAPI load validation (#4511), request-phase MCP guardrails (#4493).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [#4473](https://github.com/agentgateway/agentgateway/pull/4473) | mcp: serve multiple MCP endpoints in standalone mode | [howardjohn](https://github.com/howardjohn) | 11-23 22:22Z |
| agentgateway | [#4502](https://github.com/agentgateway/agentgateway/pull/4502) | llm: replace tokio_sse_codec with an in-tree SSE decoder | [markuskobler](https://github.com/markuskobler) | 11-23 22:37Z |
| agentgateway | [#4529](https://github.com/agentgateway/agentgateway/pull/4529) | llm: translate Responses images and files to Chat Completions | [markuskobler](https://github.com/markuskobler) | 11-24 00:50Z |
| agentgateway | [#4420](https://github.com/agentgateway/agentgateway/pull/4420) | bedrock: map advanced-tool-use, refresh Anthropic beta allowlist | [Quentin-M](https://github.com/Quentin-M) | 11-23 21:15Z |
| agentgateway | [#4514](https://github.com/agentgateway/agentgateway/pull/4514) | backendAuth: keep credentials with a short-form key | [ymesika](https://github.com/ymesika) | 11-23 16:56Z |
| agentgateway | [#4516](https://github.com/agentgateway/agentgateway/pull/4516) | hbone: resolve the service host target on CONNECT | [rajatvig](https://github.com/rajatvig) | 11-23 21:56Z |
| agentgateway | [#4469](https://github.com/agentgateway/agentgateway/pull/4469) | fix: track delegated provider health separately | [danehans](https://github.com/danehans) | 11-23 21:33Z |
| agentgateway | [#4493](https://github.com/agentgateway/agentgateway/pull/4493) | mcpGuardrails: cover request phase tasks/* | [stevenctl](https://github.com/stevenctl) | 11-24 03:49Z |
| agentgateway | [#4511](https://github.com/agentgateway/agentgateway/pull/4511) | fix(mcp): reject unusable OpenAPI operations at config load | [jamesilse](https://github.com/jamesilse) | 11-23 17:49Z |
| agentgateway | [#4503](https://github.com/agentgateway/agentgateway/pull/4503) | budgets: ignore stale writes from an earlier window | [conradhanson](https://github.com/conradhanson) | 11-23 16:25Z |
| agentgateway | [#4527](https://github.com/agentgateway/agentgateway/pull/4527) | mcp: list file-backed endpoint settings | [howardjohn](https://github.com/howardjohn) | 11-24 00:15Z |
| agentgateway | [#4167](https://github.com/agentgateway/agentgateway/pull/4167) | standalone: document schema kinds of each item | [howardjohn](https://github.com/howardjohn) | 11-23 22:09Z |
| agentgateway | [#4509](https://github.com/agentgateway/agentgateway/pull/4509) | Update model catalog | github-actions[bot] | 11-23 22:03Z |
| website | [#1604](https://github.com/agentgateway/website/pull/1604) | Agent readiness: docs-theme-extras v0.6.0 + layout overrides | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 11-23 17:23Z |
| website | [#1587](https://github.com/agentgateway/website/pull/1587) | Chatbot: load @-mention page index from one file | [kristin-kronstain-brown](https://github.com/kristin-kronstain-brown) | 11-23 14:59Z |

`benchmarks`, `community`: **none in window**.

## Watch list

| Item | Why |
| --- | --- |
| [#4530](https://github.com/agentgateway/agentgateway/pull/4530) · [#4535](https://github.com/agentgateway/agentgateway/pull/4535) · [#4534](https://github.com/agentgateway/agentgateway/pull/4534) | howardjohn's fresh guardrail/telemetry batch: bound client-supplied metric labels, realtime guardrail buffer limits, CEL MCP parsing on `/sse` in request phase. Test dashboards + `/sse` guardrail demos. |
| [#4532](https://github.com/agentgateway/agentgateway/pull/4532) · [#4490](https://github.com/agentgateway/agentgateway/pull/4490) · [#4533](https://github.com/agentgateway/agentgateway/pull/4533) | Follow-ons to #4473: MCP session upstreams rebuilt per request, Skills on multiplex targets, strip h2c Upgrade. Expect stateful-MCP behaviour churn. |
| [#4545](https://github.com/agentgateway/agentgateway/pull/4545) · [#4394](https://github.com/agentgateway/agentgateway/pull/4394) | Expire cached remote signing keys / isolate remote JWKS fetch failures — together they change IdP-outage auth behaviour. Re-run the JWT demo when either lands. |
| [#4544](https://github.com/agentgateway/agentgateway/pull/4544) | New retry policy for callouts (ymesika) — new API surface for ext-callout resilience. |
| [#4349](https://github.com/agentgateway/agentgateway/pull/4349) | Streaming token reassembly (data tokenization) still open (11-24T10:17Z) — the DLP/tokenization demo stays unreleased. |
| [#4512](https://github.com/agentgateway/agentgateway/pull/4512) | `databaseRetention` for request logs still open (11-24T09:53Z). Keep using `2160h`, not `90d`, when it merges. |
| [#4531](https://github.com/agentgateway/agentgateway/pull/4531) · [#4538](https://github.com/agentgateway/agentgateway/pull/4538) · [#4539](https://github.com/agentgateway/agentgateway/pull/4539) (draft) · [#4540](https://github.com/agentgateway/agentgateway/pull/4540) | Gemini Decisions/SystemOne + Messages↔Responses translation workstream — more multimodal/streaming churn mid-week. |
| [website#1586](https://github.com/agentgateway/website/pull/1586) | The 1.8 release-note roll-up (open, `behind`). Watch for the `v1.8.0-alpha.1` tag; verify `AGENTGATEWAY_BEDROCK_RUNTIME_CONVERSE` / `AGENTGATEWAY_VERTEX_ANTHROPIC_BETA_HEADERS` are settable in the mode you demo. |
| [website#1609](https://github.com/agentgateway/website/pull/1609) | New GreptimeDB LLM-observability integration guides — possible third-party observability demo path. |
