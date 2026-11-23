# agentgateway-updates - 2026-11-23

**Still no code merged into `agentgateway` — `main` is untouched at [#4501](https://github.com/agentgateway/agentgateway/commit/888badbbe91f2a57ec8e7bf5a5ef3acda34b1fe5) (11-20T22:04Z) and no releases. The one merge in window is docs: Christian Posta closes out the credential-injection series with [Part 4, Eliminating Plaintext](https://github.com/agentgateway/website/pull/1583) — the best new demo narrative in the org. Two new code PRs open (HBONE CONNECT host resolution [#4516](https://github.com/agentgateway/agentgateway/pull/4516), `backendAuth` short-form credentials [#4514](https://github.com/agentgateway/agentgateway/pull/4514)), and the 1.8 release-note train has left the station ([website#1586](https://github.com/agentgateway/website/pull/1586)).**

_Window: since the 2026-11-22 digest (closed at 2026-11-22T13:00Z). `benchmarks` and `community` remain idle._

## Releases

**None published** in window. Cut stays Oct 27; everything after is nightly / `:latest-dev`.

| Repo | Latest stable | Latest pre-release |
| --- | --- | --- |
| agentgateway | [v1.7.0](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0) (10-27) | [v1.7.0-rc.1](https://github.com/agentgateway/agentgateway/releases/tag/v1.7.0-rc.1) (10-26) |
| website | [v1.7.0](https://github.com/agentgateway/website/releases/tag/v1.7.0) (10-27) | - |

No `v1.8.0-alpha.1` tag yet — but [website#1586](https://github.com/agentgateway/website/pull/1586) is now assembling 1.8 release notes from the PR tracker, so the tag is being staged, not forgotten.

## Highlights

- **Credential brokering Part 4 is live** ([website#1583](https://github.com/agentgateway/website/pull/1583), christian-posta, merged 11-22T23:15Z, +118/-1 across 7 files incl. 5 new diagrams). The argument: the gateway *already* sees plaintext on every request, so let it do the final AES-GCM unwrap and strip every KMS/Transit permission from the controller — a compromised controller then reads only ciphertext. Then two hardening directions: an agent-signed **WIMSE Workload Proof Token** (SPIRE WIT + `aud`/`wth`/`oth` claims) bound to the path — or, for virtual MCP servers where one endpoint fronts many upstreams, to the **`Mcp-Name`** header from SEP-2243 — so the gateway stops being a confused deputy that can ask for any of a user's rows; and a **TEE injector** (TDX / SEV-SNP / Nitro, KMS `Recipient` attested `Decrypt`) so the plaintext never lands on the gateway host. Explicitly names this as the Agentgateway Enterprise direction. **This is the single best whiteboard flow for a security-minded audience this week.**
- **`backendAuth` short-form key silently dropped extra credentials** ([agentgateway#4514](https://github.com/agentgateway/agentgateway/pull/4514), ymesika, open, +366/-314). With `key: <string|file:|secretRef:>` plus a sibling `credentials:` list, the extra credentials never reached the backend — validated clean, primary key sent, second header missing. The fix adds `credentials` to the compat arm **and makes `BackendAuthCompat` reject unknown fields**, so a typo like `credentails` now fails at startup instead of loading with *no key at all*. Flag it as a soft breaking change: stray/misspelled fields under `backendAuth` that used to load will now crash-loop.
- **HBONE target resolution bug** ([agentgateway#4516](https://github.com/agentgateway/agentgateway/pull/4516), rajatvig, open, fixes #4515, +142/-10). Replaces `HboneAddress::socket_addr` with `gateway_socket_addr` so the CONNECT listener resolves the **Service host**, not the pod IP. Verified against a `kind` + Istio setup. Relevant to ambient/mesh-interop demos on omni-viper.
- **1.8 release notes are being auto-assembled** ([website#1586](https://github.com/agentgateway/website/pull/1586), bot, opened 11-23T10:23Z): 10 fix entries (#4381 extproc `END_STREAM`, HBONE deadlock fixes, 400 on malformed `Host`, catalog v2 endpoint, MCP `clientId` registration, buffer policy hierarchy, Copilot integrator id) plus the **#4300 Bedrock-native-endpoint breaking note** and #4416 Vertex `anthropic-beta` filtering. The bot flags env vars (`AGENTGATEWAY_BEDROCK_RUNTIME_CONVERSE`, `AGENTGATEWAY_VERTEX_ANTHROPIC_BETA_HEADERS`) documented for standalone but not kubernetes mode — check which mode you're demoing before quoting a knob. Checks are stuck at `action_required` until someone approves them.
- **Request-log retention advanced** ([agentgateway#4512](https://github.com/agentgateway/agentgateway/pull/4512), dongjiang1989, updated 11-22T13:37Z). Yesterday `dirty`, today **`blocked`, with howardjohn, lgadban and danehans as requested reviewers** — i.e. in the review queue. Body is unchanged and thorough: chunked 10k-row deletes, 100 ms spacing, 5-minute budget, abort after 3 failed chunks, payload cascade, unset = keep forever. Remember `2160h`, not `90d`.
- **1.8 doc drafts now eight deep** in website: the six from yesterday ([1576](https://github.com/agentgateway/website/pull/1576)–[1581](https://github.com/agentgateway/website/pull/1581)) plus [1582](https://github.com/agentgateway/website/pull/1582) (AWS native-endpoint sync, touched 11-23T10:23Z), [1585](https://github.com/agentgateway/website/pull/1585) (Vertex `anthropic-beta`, opened 11-23T10:23Z), on top of [1575](https://github.com/agentgateway/website/pull/1575) regenerated reference docs.
- **No new CVEs against org-owned code, no security merges in window.** Nothing in the code repo moved; `git log` for the window is empty.

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| website | [#1583](https://github.com/agentgateway/website/pull/1583) | Add credential injection patterns part 4: eliminating plaintext | [christian-posta](https://github.com/christian-posta) | 2026-11-22 23:15Z |

`agentgateway`, `benchmarks`, `community`: **none in window** — zero merged PRs and zero direct commits (`main` still `888badb`, last commit 11-20T22:04Z).

## Watch list

| Item | Why |
| --- | --- |
| [4514](https://github.com/agentgateway/agentgateway/pull/4514) | Strict `backendAuth` parsing → configs with typos/stray fields that load today will **fail at startup** once merged. Audit your demo YAMLs now. |
| [4516](https://github.com/agentgateway/agentgateway/pull/4516) | HBONE CONNECT host-target fix (fixes #4515). Re-test ambient-style routing on omni-viper when it lands. |
| [4512](https://github.com/agentgateway/agentgateway/pull/4512) | Now in review (`blocked`, 3 reviewers). First retention knob for the request-log DB; test `2160h` and confirm pruning can't starve UI queries. |
| [4505](https://github.com/agentgateway/agentgateway/pull/4505) | Substrate router/egress rewrite still a draft, updated 11-23T01:13Z, **no live e2e lanes run** — keep Substrate egress out of customer flows. |
| [4142](https://github.com/agentgateway/agentgateway/pull/4142) | JWT `maxAge`: changes *default* remote-JWKS error handling (non-2xx keeps cached keys; aged keys → 503, no `WWW-Authenticate`). Still `blocked`. Re-run the IdP-outage auth demo if it merges. |
| website [1577](https://github.com/agentgateway/website/pull/1577) · [1586](https://github.com/agentgateway/website/pull/1586) | Gateway API v1.7 move = CRD churn; rolling release notes list breaking #4300. Regenerate/smoke manifests and confirm env-var docs per mode before the 1.8 RC. |

Nothing new to re-test on `:latest-dev` today — the code repo hasn't moved. Yesterday's outstanding list (#4300 Bedrock IAM role, #4430 user attribution split, #4205 azureContentSafety CRD guards, #4497 fallback during rollout, #4483 OTel `gen_ai.response.id`, #4501 MCP stateful default) is unchanged.
