# agentgateway-updates - 2026-11-27

**Quiet on merges for a second day — zero merged PRs and zero commits to `main` since `134cbf02` (#4556, 11-24T23:22Z), so the demo build is still the 11-24 nightly. Incoming queue is the story: 7 new PRs in ~24h, four demo-relevant — MCP discovery timeouts (#4576), OIDC access-token forwarding (#4575), AgentCore MCP targets by runtime ARN (#4579, CRD change), plus a `probe` command for distroless readiness (#4581). No releases (v1.7.0 still stands, Oct 27); `benchmarks` / `community` idle.**

_Window: since the 2026-11-26 digest. `list_commits since=2026-11-26T00:00Z` → empty on all repos; `merged:>=2026-11-26` → 0 (the [48 hits](https://github.com/search?q=org%3Aagentgateway+is%3Apr+is%3Amerged+merged%3A%3E%3D2026-11-24&type=pullrequests) closed 11-24, already reported)._

## Releases

**None published** — cut still Oct 27; everything past the line is nightly / `:latest-dev`.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27T17:18Z) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [Docs for v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27T23:37Z) | – |

Still no `v1.8.0-alpha.1`. The rolling release-note PR [website#1586](https://github.com/agentgateway/website/pull/1586) is open + `behind`, untouched since 11-23T15:11Z, still flags that the reference URLs it adds were never seen in the docs, with unresolved notes for #4300 (Bedrock native endpoints, breaking), #3872 (file-backed mTLS), #4214 (MCP Skills).

## Highlights

- **One hung MCP server could stall the whole discovery fanout** ([#4576](https://github.com/agentgateway/agentgateway/pull/4576), xixi-box, 11-26T14:55Z, fixes #4572) — the top demo item this window. Optional `discoveryTimeout` on local MCP targets: one deadline from request to terminal response; `failOpen` logs and skips the target, `failClosed` returns 504; tool calls keep existing timeouts. Config-schema change. Makes a flaky third-party MCP server survivable in a tools demo.
- **OIDC login could still 400 vs. an OAuth-exchange backend** ([#4575](https://github.com/agentgateway/agentgateway/pull/4575), 11-26T14:04Z, fixes #4571): opt-in `forwardAccessToken` keeps the provider token in the encrypted browser session and supplies it as the Bearer / RFC 8693 subject token. Off by default; provider token lifetime can shorten the session.
- **Bedrock AgentCore MCP without hand-building the URL** ([#4579](https://github.com/agentgateway/agentgateway/pull/4579), 11-26T22:52Z, fixes #4574): K8s MCP targets gain `static.agentCore` (runtime ARN + qualifier); the controller emits the same AWS backend HTTPRoute uses, so the data plane derives the regional invocation URL with default TLS + SigV4 over Streamable HTTP. **CRD change** — regenerate CRDs.
- **Distroless had no readiness probe** ([#4581](https://github.com/agentgateway/agentgateway/pull/4581), 11-26T23:33Z, fixes #4580): new `agentgateway probe` hits `/healthz/ready` (1s timeout, default `127.0.0.1:15021`, `--address` override, exit 0/1) — fixes the loopback-bind vs. Pod-IP kubelet mismatch.
- **A broken authz rule looked like a non-match** ([#4584](https://github.com/agentgateway/agentgateway/pull/4584), 11-27T12:58Z, fixes #4583): CEL errors evaluate as false, so a failed Deny silently allows. New `agentgateway_authorization_cel_evaluation_errors_total` with bounded `mode`/`layer` labels.
- **Smaller fixes worth knowing**: Vertex cost accuracy ([#4582](https://github.com/agentgateway/agentgateway/pull/4582), matvas-txf, 11-27T09:05Z) — region-dependent pricing (`global` cheapest; `us`/`eu` ≈1.1x) plus Priority/Flex prices for newer Gemini models, so UI cost attribution shifts; and caller identity is now propagated to child spans ([#4578](https://github.com/agentgateway/agentgateway/pull/4578), 11-26T20:36Z, fixes #4573), which had been missing from outbound-latency metrics.
- **No new CVEs or advisories.** Auth hardening open/`blocked`: [#4545 `jwks.maxAge`](https://github.com/agentgateway/agentgateway/pull/4545) (expired keys → 503) and [#4394 JWKS failure isolation](https://github.com/agentgateway/agentgateway/pull/4394) — IdP-outage fail-open/closed behaviour changes; re-run the JWT demo when either lands.
- **Nothing to re-test**: HEAD still [`134cbf02`](https://github.com/agentgateway/agentgateway/commit/134cbf02e02742cce08ad52cdd221fef579186b8); `benchmarks` last commit [10-28](https://github.com/agentgateway/benchmarks/commit/d53aaaab52cccefd8d220f727bccf3cb53a85b65), `community` [08-25](https://github.com/agentgateway/community/commit/0bd822d6ee2cc52c46fe6e13dfbd94f7c651074c).

## Merged PRs by repo

**None in window** — nothing merged in any of the four repos since 2026-11-26. Newly **opened** queue instead (all unmerged):

| Repo | PR | Title | Author | Opened |
| --- | --- | --- | --- | --- |
| agentgateway | [#4584](https://github.com/agentgateway/agentgateway/pull/4584) | Count CEL failures during authorization decisions | xixi-box | 11-27T12:58Z |
| agentgateway | [#4582](https://github.com/agentgateway/agentgateway/pull/4582) | feat(llm): price Vertex AI regional endpoints | matvas-txf | 11-27T09:05Z |
| agentgateway | [#4581](https://github.com/agentgateway/agentgateway/pull/4581) | Add a loopback readiness probe command | xixi-box | 11-26T23:33Z |
| agentgateway | [#4579](https://github.com/agentgateway/agentgateway/pull/4579) | feat(controller): configure AgentCore MCP targets by runtime ARN | xixi-box | 11-26T22:52Z |
| agentgateway | [#4578](https://github.com/agentgateway/agentgateway/pull/4578) | fix: propagate evaluated trace attributes to child spans | xixi-box | 11-26T20:36Z |
| agentgateway | [#4576](https://github.com/agentgateway/agentgateway/pull/4576) | Bound MCP discovery with per-target deadlines | xixi-box | 11-26T14:55Z |
| agentgateway | [#4575](https://github.com/agentgateway/agentgateway/pull/4575) | feat(oidc): optionally forward browser session access tokens | xixi-box | 11-26T14:04Z |

+1 ([search](https://github.com/search?q=org%3Aagentgateway+is%3Apr+created%3A%3E%3D2026-11-26&type=pullrequests)): [website#1624](https://github.com/agentgateway/website/pull/1624) doc-test fixes (MCP samples → `node:22-alpine`).

## Watch list

| Item | Why |
| --- | --- |
| [#4576](https://github.com/agentgateway/agentgateway/pull/4576) | New `discoveryTimeout` MCP field — test failOpen skip vs. failClosed 504 with a hung server on omni-viper. |
| [#4575](https://github.com/agentgateway/agentgateway/pull/4575) | `forwardAccessToken` — opt-in; changes cookie contents + session lifetime. Regression-test login + exchange. |
| [#4579](https://github.com/agentgateway/agentgateway/pull/4579) | `static.agentCore` on `AgentgatewayBackend` — CRD change; regenerate CRDs, re-check AgentCore MCP (SigV4, Streamable HTTP). |
| [#4581](https://github.com/agentgateway/agentgateway/pull/4581) | `agentgateway probe` — update distroless/loopback readiness manifests in the lab. |
| [#4584](https://github.com/agentgateway/agentgateway/pull/4584) · [#4530](https://github.com/agentgateway/agentgateway/pull/4530) | Authz CEL-error counter + bounded client labels — dashboards, cardinality, silent Deny allows. |
| [#4545](https://github.com/agentgateway/agentgateway/pull/4545) · [#4394](https://github.com/agentgateway/agentgateway/pull/4394) · [#2939](https://github.com/agentgateway/agentgateway/pull/2939) | `jwks.maxAge` fail-closed, JWKS failure isolation, OIDC session-store boundary — JWT/auth demo shifts. |
| [#4570](https://github.com/agentgateway/agentgateway/pull/4570) · [#4568](https://github.com/agentgateway/agentgateway/pull/4568) | Vertex `embedContent` extraction (embeddings still need a routes override); telemetry shutdown after drain. |
| [#4561](https://github.com/agentgateway/agentgateway/pull/4561) · [#4562](https://github.com/agentgateway/agentgateway/pull/4562) · [#4132](https://github.com/agentgateway/agentgateway/pull/4132) | `BackendTLSPolicy` status for standalone backends; Anthropic structured-output schemas; MCP pinned-endpoint 404. |
| [#4539](https://github.com/agentgateway/agentgateway/pull/4539) · [#4128](https://github.com/agentgateway/agentgateway/pull/4128) · [#3875](https://github.com/agentgateway/agentgateway/pull/3875) | llm-ir Messages→Responses typed model; empty-translation `finish_reason` fix; credential-provider failure classes. |
| [website#1586](https://github.com/agentgateway/website/pull/1586) · [#1621](https://github.com/agentgateway/website/pull/1621) · [#1623](https://github.com/agentgateway/website/pull/1623) · [#1525](https://github.com/agentgateway/website/pull/1525) | 1.8 release-note roll-up (open, `behind`, unverified links) gates any `v1.8.0-alpha.1`; docs churn. |
