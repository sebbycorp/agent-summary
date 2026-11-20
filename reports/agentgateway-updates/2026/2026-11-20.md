# agentgateway-updates - 2026-11-20

**File-access lockdown shipped: config deny/allowlist (#4431) + backend mTLS from pod files with hot reload (#3872). MCP `clientId` registration now works with any IdP (#4477), CoreWeave Serverless added (#4463), catalog refreshed (#4480). 18 merges in ~24h (11 `agentgateway`, 7 `website`). Still no 1.8 tag.**

_Window: merged `>=2026-11-19T13:00Z` (yesterday's digest covered `#4365`, `#1564`). `benchmarks` and `community` idle._

## Releases

**None published in window** — below the Oct 27 cut everything is nightly / `:latest-dev`.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | – |

## Highlights

- **Config can't read arbitrary files — new `fileAccess`** ([4431](https://github.com/agentgateway/agentgateway/pull/4431), howardjohn). Top-level `config.fileAccess: {allow, deny}` gates every `ResourceRef::File` read in config-API validation, cached and uncached. Builtin deny `/dev`, `/proc`, `/sys`; longest prefix wins, ties → `deny`; the path *and* its canonicalized target must be a regular file; empty `allow` = anything passing deny. New `schema/config.json` key — update hardened-tenant demo YAML. Corrects yesterday's note: merged, not open.
- **Backend mTLS from pod files, hot-reloaded** ([3872](https://github.com/agentgateway/agentgateway/pull/3872), mingcui-yang, fixes #3001). `mtlsCertificateRef` on `AgentgatewayBackend`/`AgentgatewayPolicy` takes `kind: File` (`cert`/`key`/optional `root`); the controller only forwards paths over xDS — the gateway reads and watches them. Unblocks external cert rotators with no Secret-sync component; pairs with #4431 for the zero-trust slide.
- **MCP `clientId` works with every provider, or none** ([4477](https://github.com/agentgateway/agentgateway/pull/4477), jamesilse). Only Keycloak/Okta/Descope/Authentik/Entra used to suppress the IdP's dynamic-registration endpoint, so Auth0/PingFederate clients died there. Now `clientId` makes the gateway advertise its own registration endpoint and itself as authorization server in both metadata docs, keeping the IdP's authorize/token endpoints; plus OpenID-config discovery fallback, RFC 8414 trailing-slash matching, `signed_metadata` dropped on issuer rewrite. **Fixes the non-Okta/Entra MCP auth demo.**
- **CoreWeave Serverless Inference** ([4463](https://github.com/agentgateway/agentgateway/pull/4463), jmcshane): Completions-only preset; LoRA via `wandb-artifact:///<team>/<project>/<artifact>:<version>` in `model`. New proto enum + CRD `ModelProvider: CoreWeave`, UI icon, LiteLLM `wandb/`+`coreweave/`. Docs open ([website#1562](https://github.com/agentgateway/website/pull/1562)).
- **OAuth token-exchange cache keys input-complete** ([4468](https://github.com/agentgateway/agentgateway/pull/4468)) — differing initial vs chained parameter layouts can no longer share a cache entry.
- **Catalog refresh** ([4480](https://github.com/agentgateway/agentgateway/pull/4480)): GPT-6.2 Astra/Sol (`ultrafast`/`flex` tiers, `cacheWriteDetails.12h`), Gemini 4 Neon, MAI Code 2, GPT-Live 1.1, Hy4 flash, Qwen3-30B-A3B, Llama 3.1/3.2. `gpt-6.2-sol` is `temperature: false` (the block it displaced was `true`) — re-shoot the cost dashboard.
- **Deps**: tiktoken git fork → crates.io 5.0.1 ([4479](https://github.com/agentgateway/agentgateway/pull/4479)); bindgen/prettyplease on `syn 3`, fixing FIPS builds ([4475](https://github.com/agentgateway/agentgateway/pull/4475)); weekly bump ([4458](https://github.com/agentgateway/agentgateway/pull/4458)) = pingora 0.9.1 (panic on every accepted connection with tokio ≥1.54; zstd 0.14.3 past RustSec IDs), arc-swap 1.9.5.
- **UI/docs**: UI off the monolithic API toward resource list APIs ([4461](https://github.com/agentgateway/agentgateway/pull/4461)); hybrid note that local rate limits are per-instance, use Remote for a shared ceiling ([4466](https://github.com/agentgateway/agentgateway/pull/4466)) — gotcha for 3-replica demos. docs-theme-extras v0.5.2 fixes anchors under the sticky header ([1563](https://github.com/agentgateway/website/pull/1563)); the screenshot refresh merged **with a failed `latest main` capture** ([1559](https://github.com/agentgateway/website/pull/1559)).

No CVEs against org-owned code in window; security items are dependency-level or hardening (#4431, #4468).

## Merged PRs by repo

**18 merges** (11 `agentgateway`, 7 `website`); none in `benchmarks`/`community`.

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [3872](https://github.com/agentgateway/agentgateway/pull/3872) | backend mTLS from local files + hot reload | mingcui-yang | 11-20 |
| agentgateway | [4431](https://github.com/agentgateway/agentgateway/pull/4431) | config: deny/allowlist referenced files | howardjohn | 11-20 |
| agentgateway | [4480](https://github.com/agentgateway/agentgateway/pull/4480) | Fix model catalog refresh | howardjohn | 11-19 |
| agentgateway | [4477](https://github.com/agentgateway/agentgateway/pull/4477) | fix(mcp): clientId registration, any provider | jamesilse | 11-19 |
| agentgateway | [4463](https://github.com/agentgateway/agentgateway/pull/4463) | llm: add coreweave provider | jmcshane | 11-19 |
| agentgateway | [4468](https://github.com/agentgateway/agentgateway/pull/4468) | oauth: token exchange cache inputs | markuskobler | 11-19 |
| agentgateway | [4479](https://github.com/agentgateway/agentgateway/pull/4479) | Move off tiktoken fork | howardjohn | 11-19 |
| agentgateway | [4461](https://github.com/agentgateway/agentgateway/pull/4461) | ui: resource list APIs | howardjohn | 11-19 |
| agentgateway | [4466](https://github.com/agentgateway/agentgateway/pull/4466) | ui: per-instance rate limits in hybrid | jamesilse | 11-19 |
| agentgateway | [4475](https://github.com/agentgateway/agentgateway/pull/4475) · [4458](https://github.com/agentgateway/agentgateway/pull/4458) | build: FIPS syn 3; cargo-weekly | markuskobler, dependabot | 11-19 |
| website | [1565](https://github.com/agentgateway/website/pull/1565) | docs(blog): Codex Entra ID sign-in (part 2) | sebbycorp | 11-19 |
| website | [1563](https://github.com/agentgateway/website/pull/1563) | Bump docs-theme-extras to v0.5.2 | kristin-kronstain-brown | 11-19 |
| website | [1559](https://github.com/agentgateway/website/pull/1559) | product-UI screenshots (partial) | github-actions[bot] | 11-19 |

+3 more: blog covers [1566](https://github.com/agentgateway/website/pull/1566), doc-test scoping [1540](https://github.com/agentgateway/website/pull/1540), reference docs [1557](https://github.com/agentgateway/website/pull/1557)/[1558](https://github.com/agentgateway/website/pull/1558).

## Watch list

| Item | Why |
| --- | --- |
| [4430](https://github.com/agentgateway/agentgateway/pull/4430) | **(breaking, open)** default `agentgateway.user` → `jwt.email` → `jwt.sub` → API-key metadata, dropping `apiKey.name`: splits request-log history across the upgrade, changes UI API-key ownership. Pin `standardAttributes.user` for old behaviour. |
| [#4205](https://github.com/agentgateway/agentgateway/pull/4205) · [4394](https://github.com/agentgateway/agentgateway/pull/4394) | Azure Content Safety guards + dropping the deprecated nested guard `policies` from #4387 (next CRD/API change, real size win); isolate remote JWKS fetch failures — last unmerged auth change now #4431 is in. |
| [4469](https://github.com/agentgateway/agentgateway/pull/4469) · [4455](https://github.com/agentgateway/agentgateway/pull/4455) · [4369](https://github.com/agentgateway/agentgateway/pull/4369) | Delegated-provider health separation; Entra federated credentials; `privateKeyJwt` capped at 300s — enterprise delegated/OAuth demos. |
| [4464](https://github.com/agentgateway/agentgateway/pull/4464) · [4473](https://github.com/agentgateway/agentgateway/pull/4473) · [4453](https://github.com/agentgateway/agentgateway/pull/4453) | Stable config-store UIDs; **new draft** multiple MCP endpoints in standalone; reject `server/discover` for stdio/SSE. |
| [4471](https://github.com/agentgateway/agentgateway/pull/4471) | KEDA `ScaledObject` in standalone Helm **closed unmerged 11-19** — drop from the autoscaling slide unless resubmitted. |
| website [1568](https://github.com/agentgateway/website/pull/1568)–[1573](https://github.com/agentgateway/website/pull/1573) | Azure/AKS docs: scopes, managed identity, embeddings, capability matrix, AKS CRDs. |

Re-test on omni-viper with `:latest-dev`: #4431 `/dev` + non-regular-file blocks; #3872 cert rotation; #4477 vs a no-provider IdP; #4463 `wandb-artifact://` model; #4468 delegated OAuth; #4480 cost dashboard.
