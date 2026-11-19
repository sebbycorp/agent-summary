# agentgateway-updates - 2026-11-19

**Prompt-guard CRD reshuffle (#4387) finally merged, model catalog moved to models.dev v2 pricing (#4457/#4447), new CEL `recurse()` unlocks full RFC 8693 delegation checks, UI rebuilt on antd — 17 merges in 36h (15 `agentgateway`, 2 `website`). Still no 1.8 tag; newest artifact is the Oct 27 v1.7.0 cut.**

_Window: merged `>=2026-11-18T00:00Z`. `benchmarks` and `community` idle._

## Releases

**None published in window** — everything below the Oct 27 cut is nightly / `:latest-dev` only.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) "Docs for v1.7.0 release" (10-27) | – |

## Highlights

- **Prompt-guard policies at top level — CRD shape change is in `main`** ([4387](https://github.com/agentgateway/agentgateway/pull/4387), jgreeer). `policies` left the nested guard block; per-type CEL validation now rejects `policies` on regex/webhook guards, limits `openAIModeration` to key/secretRef, `bedrockGuardrails` to key/secretRef/aws, `googleModelArmor` to gcp only. Old nested fields came back deprecated, so **no CRD size win yet** (1.23→1.08 MB once removed); prep for [#4205](https://github.com/agentgateway/agentgateway/pull/4205). Update demo YAML that nests policies.
- **`jwt.act.recurse("act")` = real delegation-chain authorization** ([4365](https://github.com/agentgateway/agentgateway/pull/4365), ymesika, 11-19, newest commit on `main`). One `require` rule now covers *every* actor in an RFC 8693 chain instead of hand-spelled depths (`jwt.act.sub`, `jwt.act.act.sub`, …) where a chain one level deeper went unchecked; malformed `act` or >32 actors fail closed. Best new security-demo line for multi-agent stories.
- **Model catalog now from models.dev `experimental/v2.0/api.json`** ([4457](https://github.com/agentgateway/agentgateway/pull/4457), howardjohn; +7.3k/−0.7k on `model-catalog.json`, new 608-line `source_modelsdev_pricing.go`). Rewritten price ingestion: structured reasoning capabilities, date/region/service-tier overrides, multipliers, per-minute audio units, TTL-aware cache-write schedules, `decision`/`systemone` endpoints; Typesafe and `gemini-decision-1` overrides deleted as upstream now supplies them. **Quoted rates shift — re-screenshot the cost dashboard.** Related hardening: unknown `serviceTier` values no longer fail the whole catalog decode on older releases ([4447](https://github.com/agentgateway/agentgateway/pull/4447)), plus catalog-test fix [4462](https://github.com/agentgateway/agentgateway/pull/4462).
- **gRPC errors behind ext-proc reach clients correctly** ([4381](https://github.com/agentgateway/agentgateway/pull/4381), AlecGoncharow). Trailers-only responses lost `END_STREAM` — `mutate_response_inner` always swapped in a `StreamBody` whose `is_end_stream()` is false — so clients dropped `grpc-status` and saw `Internal: server closed the stream without sending trailers` instead of e.g. `PermissionDenied`.
- **UI rebuilt on antd** ([4409](https://github.com/agentgateway/agentgateway/pull/4409), Charlesthebird): hand-rolled select/drawer/date-range/multi-select/dialog replaced, −1,279 net lines in `ui/src`, **+16% gzipped JS+CSS**, more colors re-picked for 4.5:1 / 3:1 contrast, date picker now a calendar. [4364](https://github.com/agentgateway/agentgateway/pull/4364) (stevenctl) exposes CEL MCP guardrails in the UI, previously API-only. **Deck screenshots are stale.**
- **Also merged**: Gemini decision models serve OpenAI Decisions/SystemOne via Google's native `/v1beta/decisions` ([4402](https://github.com/agentgateway/agentgateway/pull/4402), best-effort — assistant/tool history, image assets, multipart deferred); Postgres migration connection now exempt from session timeout, so a hardened role no longer crash-loops startup ([4448](https://github.com/agentgateway/agentgateway/pull/4448), pairs with #4368); AWS SDK at `BehaviorVersion::v2026_11_17` with Retry 2.1 and a default ~5-min credential read timeout ([4449](https://github.com/agentgateway/agentgateway/pull/4449)) + 7-day `cargo update` cooldown ([4451](https://github.com/agentgateway/agentgateway/pull/4451)); Anthropic `display` reasoning passthrough ([4459](https://github.com/agentgateway/agentgateway/pull/4459)); Cognito JWT example ([4410](https://github.com/agentgateway/agentgateway/pull/4410)); catalog adds GPT-6.2 Astra/Sol, Qwen4-Max, GLM-4.5/4.6V ([4445](https://github.com/agentgateway/agentgateway/pull/4445)); blog tutorial + quickstart fixes ([website#1564](https://github.com/agentgateway/website/pull/1564), [#1560](https://github.com/agentgateway/website/pull/1560)).

No CVEs or advisories in window; security-adjacent work (config-file deny/allowlist, JWKS isolation, breaking `agentgateway.user` default) is all still **open**.

## Merged PRs by repo

**17 merges** (15 `agentgateway`, 2 `website`); none in `benchmarks`/`community`.

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| agentgateway | [4365](https://github.com/agentgateway/agentgateway/pull/4365) | feat(cel): add recurse() to walk nested fields | ymesika | 11-19 |
| agentgateway | [4387](https://github.com/agentgateway/agentgateway/pull/4387) | controller: move prompt guard policies to top level | jgreeer | 11-18 |
| agentgateway | [4457](https://github.com/agentgateway/agentgateway/pull/4457) | catalog: move to v2 endpoint | howardjohn | 11-18 |
| agentgateway | [4447](https://github.com/agentgateway/agentgateway/pull/4447) | Drop catalog tiers with an unrecognized serviceTier | ymesika | 11-18 |
| agentgateway | [4381](https://github.com/agentgateway/agentgateway/pull/4381) | fix(extproc): preserve END_STREAM for bodyless responses | AlecGoncharow | 11-18 |
| agentgateway | [4409](https://github.com/agentgateway/agentgateway/pull/4409) | ui: move to antd components | Charlesthebird | 11-18 |
| agentgateway | [4402](https://github.com/agentgateway/agentgateway/pull/4402) | feat(llm): route Decisions/SystemOne to Gemini Decisions API | markuskobler | 11-18 |
| agentgateway | [4448](https://github.com/agentgateway/agentgateway/pull/4448) | no session timeout when running migration | simon-engledew | 11-18 |
| agentgateway | [4449](https://github.com/agentgateway/agentgateway/pull/4449) | deps(aws): adopt BehaviorVersion v2026_11_17 | markuskobler | 11-18 |
| agentgateway | [4364](https://github.com/agentgateway/agentgateway/pull/4364) | ui: allow editing CEL MCP guardrails | stevenctl | 11-18 |
| agentgateway | [4459](https://github.com/agentgateway/agentgateway/pull/4459) | llm: passthrough anthropic display reasoning field | howardjohn | 11-18 |
| agentgateway | [4410](https://github.com/agentgateway/agentgateway/pull/4410) | docs: add Amazon Cognito JWT example | jamesilse | 11-18 |
| agentgateway | [4462](https://github.com/agentgateway/agentgateway/pull/4462) | fix merge conflict on catalog test | howardjohn | 11-18 |
| agentgateway | [4451](https://github.com/agentgateway/agentgateway/pull/4451) | cargo update 7d cooldown | markuskobler | 11-18 |
| agentgateway | [4445](https://github.com/agentgateway/agentgateway/pull/4445) | Update model catalog | github-actions[bot] | 11-18 |
| website | [1564](https://github.com/agentgateway/website/pull/1564) | docs(blog): deploy Codex CLI with Bedrock through agentgateway | sebbycorp | 11-19 |
| website | [1560](https://github.com/agentgateway/website/pull/1560) | Quickstart prerequisites, port-forward note, system theme | kristin-kronstain-brown | 11-18 |

## Watch list

| PR | Why |
| --- | --- |
| [4430](https://github.com/agentgateway/agentgateway/pull/4430) | **(breaking, open)** default `agentgateway.user` → `jwt.email` → `jwt.sub` → API-key metadata, dropping `apiKey.name`: splits request-log history per person across the upgrade, changes UI API-key ownership, attributes JWT+key requests to the JWT. Pin `standardAttributes.user` for old behaviour. |
| [4431](https://github.com/agentgateway/agentgateway/pull/4431) · [4394](https://github.com/agentgateway/agentgateway/pull/4394) | Unmerged hardening: deny/allowlist files referenced from config (blocks `/dev/…` refs); isolate remote JWKS fetch failures. Both change auth-demo failure modes. |
| [#4205](https://github.com/agentgateway/agentgateway/pull/4205) | Follow-on to #4387: removing the deprecated nested guard `policies` is the next CRD/API change (and the real size win). |
| [4463](https://github.com/agentgateway/agentgateway/pull/4463) · [website 1562](https://github.com/agentgateway/website/pull/1562) | New **CoreWeave Serverless Inference** provider (code + docs), opened 11-19 — next provider to try on omni-viper. |
| [4469](https://github.com/agentgateway/agentgateway/pull/4469) · [4468](https://github.com/agentgateway/agentgateway/pull/4468) · [4455](https://github.com/agentgateway/agentgateway/pull/4455) | Delegated-provider health tracked separately; OAuth token-exchange cache-key separation; Entra federated credentials — delegated/OAuth paths used in enterprise demos. |
| [4464](https://github.com/agentgateway/agentgateway/pull/4464) · [4461](https://github.com/agentgateway/agentgateway/pull/4461) · [4466](https://github.com/agentgateway/agentgateway/pull/4466) | Stable config-store UIDs; UI off the monolithic API toward resource list APIs; per-instance local rate-limit notes in hybrid mode. Re-check after any `:latest-dev` bump. |
| [4453](https://github.com/agentgateway/agentgateway/pull/4453) · [4261](https://github.com/agentgateway/agentgateway/pull/4261) · [3872](https://github.com/agentgateway/agentgateway/pull/3872) · [4417](https://github.com/agentgateway/agentgateway/pull/4417) | Still open (4453 is **not** merged despite yesterday's note): reject `server/discover` for stdio/SSE; Gemini `propertyOrdering` vs constrained-decoding loops; backend mTLS from local files + hot reload; Devin Desktop Claude Agent. |
| [4471](https://github.com/agentgateway/agentgateway/pull/4471) | Configurable KEDA `ScaledObject` in the standalone Helm chart — the standalone/autoscaling slide. |

Re-test on omni-viper with `:latest-dev`: top-level `policies` + CEL validation (#4387), `jwt.act.recurse("act")` require rules (#4365), catalog rates after the v2 switch (#4457/#4447), ext-proc in front of gRPC (#4381), 3 replicas + shared Postgres cold start incl. migration timeout (#4368 + #4448), refreshed UI screenshots (#4409/#4364).
