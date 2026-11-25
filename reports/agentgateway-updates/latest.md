# agentgateway-updates - 2026-11-25

**Quiet-code Monday, heavy hardening day: 45 PRs merged in ~33h (18 `agentgateway`, 27 `website`) — loopback rejection for dynamic backends, ReferenceGrant enforcement on `AgentgatewayBackend` refs (soft breaking), per-request MCP session rebuild, Gemini/Anthropic token-count and non-SSE compat. Still no v1.8 tag; `main`/`:latest-dev` carries the demo features.**

_Window: since the 2026-11-24 digest (closed 11-24T03:49Z). `benchmarks` and `community` idle._

## Releases

**None published** in window; the cut stays Oct 27, so everything below is nightly / `:latest-dev`. The rolling release-note PR [website#1586](https://github.com/agentgateway/website/pull/1586) (~30 entries) is open and `behind`, untouched since 11-23T15:11Z.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [v1.7.0 Docs](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | - |

## Highlights

- **SSRF hardening: dynamic backends can't dial loopback** ([#4528](https://github.com/agentgateway/agentgateway/pull/4528)) — in routed-connect mode `Backend::Dynamic` now hits `check_not_loopback` when `allow_dynamic_loopback` is false; only static paths were checked. Re-test the dynamic-MCP / agent-sandbox demo.
- **Soft breaking: ReferenceGrants enforced on `AgentgatewayBackend` refs** ([#4536](https://github.com/agentgateway/agentgateway/pull/4536)) — `PolicyCtx` flows into `BuildAgwBackendReferences` and the exported `CheckBackendRefGrant` gates every backend `backendRef` (e.g. `policies.tunnel.backendRef`); new testdata rejects a cross-namespace tunnel to a "victim" LLM backend as `PartiallyValid` / "missing a ReferenceGrant?". **Audit multi-namespace manifests before redeploying.**
- **MCP sessions follow config changes live** ([#4532](https://github.com/agentgateway/agentgateway/pull/4532)) — targets/clients/policies rebuild per request; only upstream state (stdio process, SSE stream, session id, pinned endpoint) persists, so policy edits apply without client reconnects.
- **Client compat + reliability** — `streamGenerateContent` without `?alt=sse` returns a JSON array, not 400, upstream still asked for SSE so usage/guardrails are unchanged (#4554); Vertex `count-tokens:rawPredict` routes as `AnthropicTokenCount` (#4556); Anthropic token counting to Bedrock (#4555); OpenAPI MCP parses at config time so a bad doc fails at load (#4551); disconnects no longer poison health (#4441); TLS CONNECT into an inner plain-HTTP bind no longer dials :443/503 (#4546).
- **Docs caught up to 1.8** — Gemini Decisions, the Vertex `anthropic-beta` allowlist + env var, catalog `endpoints`, virtual models, `responseJsonSchema` ([#1578](https://github.com/agentgateway/website/pull/1578)); Claude Desktop `mcp-remote` bridge fix, `GET /mcp` resume on any replica, `authorizationLocation` ([#1581](https://github.com/agentgateway/website/pull/1581)); CoreWeave with LoRA via `wandb-artifact:///` ([#1562](https://github.com/agentgateway/website/pull/1562)); AKS managed Gateway API needs Kubernetes 1.35–1.37 and 30 third-party pins were bumped, 9 majors (#1569, #1574) — re-test guides you demo.
- **No new CVEs** against org-owned code; nearest security-relevant merges: #4528, #4536, and guardrail coverage (realtime buffer limits #4535, CEL MCP parsing on `/sse` in the request phase #4534).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [#4536](https://github.com/agentgateway/agentgateway/pull/4536) | Expand route-and-policy ref grant checks | howardjohn | 11-24 17:34Z |
| agentgateway | [#4528](https://github.com/agentgateway/agentgateway/pull/4528) | Check loopback on routed connect mode | howardjohn | 11-24 17:14Z |
| agentgateway | [#4532](https://github.com/agentgateway/agentgateway/pull/4532) | mcp: rebuild session upstreams each request | howardjohn | 11-24 22:24Z |
| agentgateway | [#4551](https://github.com/agentgateway/agentgateway/pull/4551) | mcp: parse OpenAPI schema at config time | howardjohn | 11-24 19:05Z |
| agentgateway | [#4554](https://github.com/agentgateway/agentgateway/pull/4554) | gemini streamGenerateContent without alt=sse | howardjohn | 11-24 23:21Z |
| agentgateway | [#4556](https://github.com/agentgateway/agentgateway/pull/4556) · [#4555](https://github.com/agentgateway/agentgateway/pull/4555) | vertex anthropic token count; anthropic token counting to bedrock | howardjohn | 23:21Z |
| agentgateway | [#4534](https://github.com/agentgateway/agentgateway/pull/4534) · [#4535](https://github.com/agentgateway/agentgateway/pull/4535) | cel: request-phase MCP parse on /sse; guardrail buffer limits | howardjohn | 18:16Z |
| agentgateway | [#4441](https://github.com/agentgateway/agentgateway/pull/4441) · [#4546](https://github.com/agentgateway/agentgateway/pull/4546) · [#4549](https://github.com/agentgateway/agentgateway/pull/4549) | health reporting fixes; URI scheme from inner listener protocol; price dated + bare Vertex Claude names | howardjohn, hittyt, jamesilse | 18:35–22:36Z |
| agentgateway | [#4490](https://github.com/agentgateway/agentgateway/pull/4490) · [#4533](https://github.com/agentgateway/agentgateway/pull/4533) · [#4550](https://github.com/agentgateway/agentgateway/pull/4550) · [#4537](https://github.com/agentgateway/agentgateway/pull/4537) · [#4542](https://github.com/agentgateway/agentgateway/pull/4542) · [#4543](https://github.com/agentgateway/agentgateway/pull/4543) | Skills on multiplex targets; strip h2c upgrade; xds NACK guard; UI resource-list APIs; model catalog; gitignore | howardjohn +2 | 15:48–20:38Z |
| website | [#1578](https://github.com/agentgateway/website/pull/1578) · [#1581](https://github.com/agentgateway/website/pull/1581) · [#1580](https://github.com/agentgateway/website/pull/1580) | 1.8 drafts: LLM providers/APIs/catalog; MCP; observability+UI | artberger | 18:16–21:54Z |
| website | [#1571](https://github.com/agentgateway/website/pull/1571) · [#1572](https://github.com/agentgateway/website/pull/1572) · [#1573](https://github.com/agentgateway/website/pull/1573) | Azure v1.7 scopes/catalog/costs; Copilot embeddings; provider capability matrix | iandvt | 16:57–17:10Z |
| website | [#1562](https://github.com/agentgateway/website/pull/1562) · [#1574](https://github.com/agentgateway/website/pull/1574) · [#1569](https://github.com/agentgateway/website/pull/1569) | CoreWeave provider; 30 third-party pins (9 majors); AKS managed Gateway API | jmcshane, bot, iandvt | 15:22–17:51Z |

**+18 more, all in `website`** — automated 1.8 doc drafts and reference refreshes ([#1568](https://github.com/agentgateway/website/pull/1568), [#1606](https://github.com/agentgateway/website/pull/1606), [#1611](https://github.com/agentgateway/website/pull/1611)…) → [list](https://github.com/search?q=org%3Aagentgateway+is%3Apr+is%3Amerged+merged%3A%3E%3D2026-11-24T03%3A50%3A00Z&type=pullrequests). `benchmarks` / `community`: none.

## Watch list

| Item | Why |
| --- | --- |
| [#4548](https://github.com/agentgateway/agentgateway/pull/4548) · [#4530](https://github.com/agentgateway/agentgateway/pull/4530) · [#4488](https://github.com/agentgateway/agentgateway/pull/4488) | Disable legacy MCP protocols per client (draft — re-test `/sse`); bound client-supplied metric labels (cardinality/DoS); Weaver telemetry registry. |
| [#4545](https://github.com/agentgateway/agentgateway/pull/4545) · [#4394](https://github.com/agentgateway/agentgateway/pull/4394) | Expire cached signing keys / isolate remote JWKS failures — change IdP-outage auth; re-run the JWT demo on omni-viper. |
| [#4544](https://github.com/agentgateway/agentgateway/pull/4544) · [#4512](https://github.com/agentgateway/agentgateway/pull/4512) · [#4349](https://github.com/agentgateway/agentgateway/pull/4349) | Callout retry policy, `databaseRetention` for request logs, streaming token reassembly (DLP demo). |
| [#4557](https://github.com/agentgateway/agentgateway/pull/4557) · [#4558](https://github.com/agentgateway/agentgateway/pull/4558) · [#4561](https://github.com/agentgateway/agentgateway/pull/4561) · [#4562](https://github.com/agentgateway/agentgateway/pull/4562) | `llm.costStatus` CEL var, server-side key generation in the config API, BackendTLSPolicy status for standalone backends, Anthropic structured-output schemas. |
| [#4538](https://github.com/agentgateway/agentgateway/pull/4538) · [website#1586](https://github.com/agentgateway/website/pull/1586) · [#1577](https://github.com/agentgateway/website/pull/1577) · [#1576](https://github.com/agentgateway/website/pull/1576) | llm-ir Messages↔Responses workstream, plus the 1.8 release-note roll-up (open, `behind`), Gateway API v1.7 move and guardrails drafts — CRD/version guidance in flux; watch for `v1.8.0-alpha.1`. |
