# kagent-updates - 2026-11-11

**No new releases anywhere — kagent still v1.0.0 GA — but main moved: harness/sandbox images jump to Alpine 3.24 (`#3618`), current Gemini IDs enter the model picker (`#3525`), and the docs finally catch up to 1.0 with a UI Policies page, A2A `ListTasks`, Foundry-Claude and PDB/`extraObjects` guidance (`website#719`). 15 PRs merged org-wide, 11 new since yesterday (kagent 5, website 6).**

Window: 2026-11-10 → 2026-11-11 (since `latest.md` @ 11-10). Not repeated: substrate [#167](https://github.com/kagent-dev/substrate/pull/167), website [#705](https://github.com/kagent-dev/website/pull/705), doc2vec [#142](https://github.com/kagent-dev/doc2vec/pull/142), kagent [#3577](https://github.com/kagent-dev/kagent/pull/3577).

## Releases

No releases published, and none pending (kagent and substrate publish RCs as non-prerelease; both still list rc4 as their last). Current tips: [kagent v1.0.0](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0) (11-09, nothing tagged since — work is landing on `main`), [substrate v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) (11-08; `release-1.0` is ahead via [#167](https://github.com/kagent-dev/substrate/pull/167)), [doc2vec v2.17.4](https://github.com/kagent-dev/doc2vec/releases/tag/v2.17.4) (11-10, reported yesterday), [tools v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1), [kmcp v0.4.0](https://github.com/kagent-dev/kmcp/releases/tag/v0.4.0).

## Highlights

- **Harness/sandbox images: Alpine 3.22 → 3.24.** The main, claude/codex harness and sandbox-guest Dockerfiles now share one `ARG ALPINE_VERSION=3.24` fed by the Makefile; the nested files had drifted because Dependabot only watches `/go` ([#3618](https://github.com/kagent-dev/kagent/pull/3618), [commit](https://github.com/kagent-dev/kagent/commit/10dd11f5a693bb2f38e6209d5287b553bb40b3d6)). For omni-viper: those tags off `main` get **new digests on a new base** — re-pin digest-pinned demo YAML.
- **Gemini picker current:** `gemini-4-argon` + `gemini-3.8-flash` (function calling) added to the Gemini and Vertex Gemini catalogs; Bedrock untouched ([#3525](https://github.com/kagent-dev/kagent/pull/3525)).
- **Stop actually stops:** Stop before the first status frame sent no `CancelTask`, so the runtime ran (and billed) the turn; the stream now waits for the task id, then cancels ([#3623](https://github.com/kagent-dev/kagent/pull/3623)). Gap: a hard reload before the task is named still loses it.
- **The 1.0 docs gap closed** in "Various updates" ([#719](https://github.com/kagent-dev/website/pull/719)), `latest` **and** `main` — most demo-relevant item of the day:
  - **EgressPolicy CRUD in the UI** (new **Policies → Egress Policies** page; auth None / Secret key / Actor JWT) + a verify-by-`/dev/tcp` recipe (no `curl` in the runtime image).
  - **A2A `ListTasks` documented** — recover a lost task ID via `contextId`/`status`/paging; `pageSize` >100 errors rather than capping. Push rules: any 2xx acks (redirect = failure), 10 attempts per state, `config.push.*` knobs.
  - **Azure AI Foundry can serve Claude:** `foundry.apiFormat: Anthropic` → `<endpoint>/anthropic/v1/messages`; `apiVersion` ignored there.
  - **Ops:** `controller.pdb.*` (`minAvailable` needs `maxUnavailable: null`; chart fails if both set) and **`extraObjects`** — ship your ExternalSecret/HTTPRoute/NetworkPolicy inside the kagent release, rendered with `tpl`.
  - Crash recovery clarified: a `CRASHED` actor is recovered only via **`RevertSession` (SessionService gRPC)** — no CLI/UI — and the in-flight turn is marked failed.
- **Docs-site security bumps taken:** Next.js 16.4.3 → 16.4.5 (Nov-3 advisory batch: `'use cache'` cross-tenant disclosure, cache poisoning, Edge HttpOnly cookie leak, SVG bypass); `@opennextjs/cloudflare` 1.20.16 now *requires* ≥16.4.5 ([#717](https://github.com/kagent-dev/website/pull/717)).
- Intake tightened: blank issues maintainers-only; the template chooser links the private `SECURITY.md` path ([#3619](https://github.com/kagent-dev/kagent/pull/3619)).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3618](https://github.com/kagent-dev/kagent/pull/3618) | build: one Alpine version across the Go images (→3.24) | VISHNU0906 | 11-11 |
| kagent | [#3525](https://github.com/kagent-dev/kagent/pull/3525) | feat(models): suggest current Gemini model IDs | jayunit100 | 11-10 |
| kagent | [#3623](https://github.com/kagent-dev/kagent/pull/3623) | fix(ui): cancel the task when Stop is pressed before it is named | alan-bentley-pgh | 11-10 |
| kagent | [#3619](https://github.com/kagent-dev/kagent/pull/3619) | chore: restrict blank issues to maintainers | timflannagan | 11-10 |
| kagent | [#3046](https://github.com/kagent-dev/kagent/pull/3046) | chore(make): print Python and Buf versions | yashrajshuklaaa | 11-11 |
| website | [#719](https://github.com/kagent-dev/website/pull/719) | Various updates (Policies UI, `ListTasks`, Foundry-Claude, PDB, `extraObjects`) | Nadine2016 | 11-10 |
| website | [#721](https://github.com/kagent-dev/website/pull/721) | Link fixes | Nadine2016 | 11-10 |
| website | [#720](https://github.com/kagent-dev/website/pull/720) / [#702](https://github.com/kagent-dev/website/pull/702) | [Automated] Refresh kagent UI screenshots | github-actions[bot] | 11-10 |
| website | [#717](https://github.com/kagent-dev/website/pull/717) | deps: npm-minor-patch ×6 (next 16.4.5 security release) | dependabot[bot] | 11-10 |
| website | [#716](https://github.com/kagent-dev/website/pull/716) | deps: @playwright/test 1.64.1 → 1.65.0 | dependabot[bot] | 11-10 |

All PRs merged in window are shown, except the four already reported on 11-10 listed at the top. Nothing merged in tools, kmcp, khook, mockllm, mockmcp, community, byo-ext.

## Watch list (kagent Enterprise alpha12 / Agent Substrate on omni-viper)

- **Cross-namespace WorkerPools (draft) — matches omni-viper's tenant layout:** `substrate.workerPoolNamespaces` lets a Harness use WorkerPools in another namespace ([#3631](https://github.com/kagent-dev/kagent/pull/3631)); exercise off `main`.
- **API-surface churn:** typed refs for models/MCP servers ([#3604](https://github.com/kagent-dev/kagent/pull/3604)) + UI keeps group/kind ([#3607](https://github.com/kagent-dev/kagent/pull/3607)); the user-set `kagent.dev/prompt-library` label moves to `v1alpha3.PromptLibraryLabel`, i.e. becomes reviewed API ([#3638](https://github.com/kagent-dev/kagent/pull/3638)).
- **Actor/session lifecycle, all open:** suspend after abandoned delivery ([#3635](https://github.com/kagent-dev/kagent/pull/3635)), interrupted-checkpoint recovery ([#3519](https://github.com/kagent-dev/kagent/pull/3519)), opt-in crash recovery on send ([#3399](https://github.com/kagent-dev/kagent/pull/3399)), session/push recovery telemetry ([#3593](https://github.com/kagent-dev/kagent/pull/3593)).
- **MCP identity plumbing:** session ID header to MCP servers ([#3628](https://github.com/kagent-dev/kagent/pull/3628)), forward the caller credential on MCP/model calls ([#3018](https://github.com/kagent-dev/kagent/pull/3018)), caller context on every span ([#2575](https://github.com/kagent-dev/kagent/pull/2575)).
- **Security-adjacent, open:** runtime cert trust for v0.10.x ([#3320](https://github.com/kagent-dev/kagent/pull/3320)), Python/native runtime trust ([#3321](https://github.com/kagent-dev/kagent/pull/3321)), extra egress CAs for the gateway upstream TLS ([substrate#101](https://github.com/kagent-dev/substrate/pull/101)).
- **Substrate still tag-less past v1.0.1** (`release-1.0` carries [#167](https://github.com/kagent-dev/substrate/pull/167), no newer commits). Open: RustFS `rustfs.existingSecret` ([#169](https://github.com/kagent-dev/substrate/pull/169)), AKS profile ([#161](https://github.com/kagent-dev/substrate/pull/161)). Installer rule stands: keep the install `-c` config — `upgrade` without it applies bundled defaults silently.
- **Demo features in flight:** kagent-tools skills + `k8s-agent` in the agent form ([#3554](https://github.com/kagent-dev/kagent/pull/3554)), xAI provider ([#3561](https://github.com/kagent-dev/kagent/pull/3561)), custom-`baseUrl` host leak ([#3537](https://github.com/kagent-dev/kagent/pull/3537)), runlab microVM PR lab ([#3637](https://github.com/kagent-dev/kagent/pull/3637)).
- **Re-test on demo agents (PRs bumped today):** Ollama stream-after-stop ([#3373](https://github.com/kagent-dev/kagent/pull/3373)), langgraph list-block text ([#3365](https://github.com/kagent-dev/kagent/pull/3365)), Anthropic tool-call order ([#3301](https://github.com/kagent-dev/kagent/pull/3301)) / multi-turn role ([#3289](https://github.com/kagent-dev/kagent/pull/3289)).
- Carried over: kagent 1.0.0 pins substrate **v1.0.0** — an "Agent Substrate 0.6" cluster is not what GA pairs with; expect alpha12 on kagent 1.0.x + substrate 1.0.1/1.0.2. And substrate `upgrade` still doesn't roll WorkerPool images.
