# agentgateway-updates - 2026-11-26

**Thanksgiving Thursday went quiet: zero merged PRs and zero commits to `main` in the ~24h window — `agentgateway` HEAD is still `134cbf02` (#4556, 11-24T23:22Z). No releases (still v1.7.0 from Oct 27). What moved is the *incoming* queue: 9 new PRs opened (Vertex embeddings routing, telemetry-on-drain, BackendTLSPolicy status, Anthropic structured-output schemas) and the auth-hardening pair #4545/#4394 is still active. `benchmarks` / `community` idle.**

_Window: since the 2026-11-25 digest (last merged work closed 11-24T23:22Z). Verified with `list_commits since=2026-11-24T23:30Z` → empty on both `agentgateway` and `website`._

## Releases

**None published in window.** The cut still stands at Oct 27, so everything below the line is nightly / `:latest-dev`.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27T17:18Z) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [Docs for v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27T23:37Z) | - |

Still no `v1.8.0-alpha.1` tag, and the rolling release-note PR [website#1586](https://github.com/agentgateway/website/pull/1586) is open with `mergeable_state: behind`, last touched 11-23T15:11Z — its body now warns that **11 reference URLs it adds were never seen in the docs before**, so the 1.8 notes are not reader-safe yet.

## Highlights

- **Nothing to re-test from merges today** — same build as yesterday's digest. Anything demoed on omni-viper today equals the 11-24 `:latest-dev` image.
- **Vertex embeddings still mis-routed → fix open** ([#4570](https://github.com/agentgateway/agentgateway/pull/4570), howardjohn, 11-25T15:50Z): completes the path-routing family from #4555/#4556 — `.../embedContent` calls through the model router pick up `embedContent` as the *model name*. Until this merges, Vertex embeddings need a per-backend routes override in the demo config.
- **Drain was silently eating telemetry** ([#4568](https://github.com/agentgateway/agentgateway/pull/4568), rajatvig, fixes #4452): telemetry now shuts down *after* the drain so spans/logs emitted during a pod shutdown are recorded. Directly affects the graceful-rollout + observability story.
- **Standalone CRD status gap** ([#4561](https://github.com/agentgateway/agentgateway/pull/4561), xixi-box): `BackendTLSPolicy` status now reported for standalone `AgentgatewayBackends` — status surface change, worth checking your `kubectl get` output expectations.
- **Structured-output fidelity** ([#4562](https://github.com/agentgateway/agentgateway/pull/4562), pqminh27): normalizes Anthropic structured-output schemas — pairs with the config-time OpenAPI parse (#4551) for the "tool/schema is wrong, fail early" demo.
- **Config API UX** ([#4560](https://github.com/agentgateway/agentgateway/pull/4560), howardjohn, 11-24T23:57Z): fixes a misleading conflict error message from the config API — relevant when scripting config pushes for labs.
- **Auth hardening pair still churning** — [#4545 expire cached remote signing keys](https://github.com/agentgateway/agentgateway/pull/4545) (upd. 11-25T01:09Z) and [#4394 isolate remote JWKS fetch failures](https://github.com/agentgateway/agentgateway/pull/4394) (upd. 11-26T04:47Z): IdP-outage behaviour will change (stale-key reuse vs. fail-closed). Re-run the JWT demo when either lands. Also open: [#4530 bound client-supplied metric labels](https://github.com/agentgateway/agentgateway/pull/4530) (cardinality/DoS).
- **No new CVEs** against org-owned code in window; no security advisories published.
- **Automated churn, all still open in `website`**: [#1621 reference docs from main](https://github.com/agentgateway/website/pull/1621), [#1622 docs fixes (backend-authn-cross-app-access-standalone)](https://github.com/agentgateway/website/pull/1622), [#1623 third-party bumps — agent-sandbox, argo-cd, argo-rollouts-gatewayapi-plugin +22](https://github.com/agentgateway/website/pull/1623), [#1624 fix doc tests from a nightly run](https://github.com/agentgateway/website/pull/1624).
- **`benchmarks` last commit 10-28** ([#8 harness isolation](https://github.com/agentgateway/benchmarks/commit/d53aaaab52cccefd8d220f727bccf3cb53a85b65)); **`community` last commit 08-25**. Both idle.

## Merged PRs by repo

**None in window.** No pull request merged in `agentgateway`, `website`, `benchmarks` or `community` between 2026-11-25T00:00Z and now. (The 48 PRs in [this search](https://github.com/search?q=org%3Aagentgateway+is%3Apr+is%3Amerged+merged%3A%3E%3D2026-11-24T00%3A00%3A00Z&type=pullrequests) all closed on 11-24 and were already reported.)

## Watch list

| Item | Why |
| --- | --- |
| [#4570](https://github.com/agentgateway/agentgateway/pull/4570) | Vertex `embedContent` model extraction — open, `blocked`; embeddings routing needs a workaround until merged. |
| [#4568](https://github.com/agentgateway/agentgateway/pull/4568) | Telemetry shutdown moved after drain (fixes #4452) — validate span completeness on a rolling restart on omni-viper. |
| [#4561](https://github.com/agentgateway/agentgateway/pull/4561) | `BackendTLSPolicy` status for standalone `AgentgatewayBackend` — CRD/status change; re-check your TLS-backend status assertions. |
| [#4562](https://github.com/agentgateway/agentgateway/pull/4562) | Anthropic structured-output schema normalization — regression-test JSON-schema tool calls after merge. |
| [#4545](https://github.com/agentgateway/agentgateway/pull/4545) · [#4394](https://github.com/agentgateway/agentgateway/pull/4394) · [#4530](https://github.com/agentgateway/agentgateway/pull/4530) | JWT signing-key expiry, JWKS failure isolation, bounded client labels — auth fail-open/closed behaviour + metrics cardinality. |
| [#4539](https://github.com/agentgateway/agentgateway/pull/4539) · [#4128](https://github.com/agentgateway/agentgateway/pull/4128) | llm-ir Messages→Responses via a typed model, plus empty-translation `finish_reason` fix — the translation workstream most likely to shake the Anthropic↔OpenAI demo. |
| [#4455](https://github.com/agentgateway/agentgateway/pull/4455) · [#2939](https://github.com/agentgateway/agentgateway/pull/2939) · [#4132](https://github.com/agentgateway/agentgateway/pull/4132) · [#3875](https://github.com/agentgateway/agentgateway/pull/3875) | Entra federated credentials, OIDC browser-session store boundary, MCP pinned-endpoint 404, credential-provider failure classes — all actively reviewed 11-25/11-26. |
| [website#1586](https://github.com/agentgateway/website/pull/1586) | 1.8 release-note roll-up, open + `behind`, 11 unverified links; gate for any `v1.8.0-alpha.1` tag. |
