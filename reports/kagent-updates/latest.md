# kagent-updates - 2026-11-09

**No new tags. Substrate turned the `kubectl ate` installer into an importable library (`#153`, `#154`, `#157`, `#162`, `#163`) and backported it to `release-1.0` (`#159`) — the pieces kagent will use to install Substrate itself; tools published a 5 KiB `tools-skills` image; kagent still pins substrate `v1.0.0`. 10 PRs merged across 4 repos.**

Window: 2026-11-08 → 2026-11-09. Not repeated from the 2026-11-08 digest: substrate v1.0.1, substrate `#149`–`#152`, kagent `#3521`/`#3557`/`#3567`, website `#704`.

## Releases

None published in window. Latest per main repo:

| Repo | Tag | Notes |
| --- | --- | --- |
| substrate | [v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) (2026-11-08) | `release-1.0` is already ahead of the tag via `#159` — needs a `v1.0.2` |
| kagent | [v1.0.0-rc3](https://github.com/kagent-dev/kagent/releases/tag/v1.0.0-rc3) (2026-11-06) | last 0.x `v0.10.6`; no `v1.0.0` tag yet |
| tools | [v0.3.1](https://github.com/kagent-dev/tools/releases/tag/v0.3.1) (2026-11-04) | `tools-skills` only pushed by the next release |

## Highlights

- **The installer is now a library — the path for kagent to install Substrate.** `Step`/`Run`/`InstallSteps` exported, so a caller inserts steps before `StepCheckDatabase` (kagent's DB after the cert controller, before Substrate); `Upgrade`/`Uninstall` take per-operation options; charts can come from an `fs.FS` ([#154](https://github.com/kagent-dev/substrate/pull/154), [#163](https://github.com/kagent-dev/substrate/pull/163)); it moved out of `cmd/kubectl-ate/internal` to `pkg/kagent/install` ([#157](https://github.com/kagent-dev/substrate/pull/157)) — import paths changed, rebase local tooling.
- **Embedded-use fix that matters for alpha12:** only the kubectl-ate release build sets `version.Version`, so an importing binary stamped its embedded charts `0.1.0` and pulled `:0.1.0` images that don't exist. The caller now passes the substrate release it depends on via `Options.Version` ([#162](https://github.com/kagent-dev/substrate/pull/162)).
- **`objectStore` is typed config:** `{type: s3|gcs|bundled, bucket, s3: {region, endpoint, pathStyle, credentialsSecret}}` fills the atelet *and* substrate values — before, the substrate object-store plugin got only a backend name. `postgres: external`, `objectStore: s3` and `rustfs` still parse ([#153](https://github.com/kagent-dev/substrate/pull/153)). Use it for an S3-backed snapshot store on omni-viper.
- **All of it is on `release-1.0`** (cherry-picks of #151/#152/#153/#154/#157), so credential providers + installer library are queued for the next patch, not main-only ([#159](https://github.com/kagent-dev/substrate/pull/159)).
- **New image `ghcr.io/kagent-dev/kagent/tools-skills`** — skills-only Agent Plugin root from `scratch`, one 5 KiB layer, vs the 195 MiB compressed tools image for the same 24 KiB of skills; cold pull 12–13 ms vs 2.06–2.43 s. Tools image unchanged, back-compat ([tools#98](https://github.com/kagent-dev/tools/pull/98)).
- **CRD condition hygiene:** `ModelConfig` stops bumping `lastTransitionTime` when only reason/message/generation change under an unchanged status — matches Agent/SandboxTemplate and API conventions (labels `bug`, `cherrpick-1.0`) ([kagent#3340](https://github.com/kagent-dev/kagent/pull/3340)).
- **New kagent.dev homepage** around kagent on Agent Substrate: clickable UI replica (dashboard, chat with tool calls/approvals/suspend-resume, models, MCP servers, worker pools) plus the 150 ms resume / 500+/s / 10× density band — a no-cluster demo prop ([website#691](https://github.com/kagent-dev/website/pull/691); [#707](https://github.com/kagent-dev/website/pull/707) fixes a duplicate navbar logo on mobile).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| substrate | [#153](https://github.com/kagent-dev/substrate/pull/153) | typed postgres and objectStore config for install | EItanya | 2026-11-08 |
| substrate | [#154](https://github.com/kagent-dev/substrate/pull/154) | per-operation installer options and exported steps | EItanya | 2026-11-08 |
| substrate | [#157](https://github.com/kagent-dev/substrate/pull/157) | move the installer to pkg/kagent/install | EItanya | 2026-11-08 |
| substrate | [#159](https://github.com/kagent-dev/substrate/pull/159) | [release-1.0] backport installer library + egress credential providers | EItanya | 2026-11-08 |
| substrate | [#162](https://github.com/kagent-dev/substrate/pull/162) | take the chart version from Options.Version | EItanya | 2026-11-09 |
| substrate | [#163](https://github.com/kagent-dev/substrate/pull/163) | export the upgrade and uninstall steps | EItanya | 2026-11-09 |
| kagent | [#3340](https://github.com/kagent-dev/kagent/pull/3340) | ModelConfig transition time only on a status change | QuentinBisson | 2026-11-08 |
| tools | [#98](https://github.com/kagent-dev/tools/pull/98) | publish the skills-only plugin root as its own image | danielle-mustillo | 2026-11-08 |
| website | [#691](https://github.com/kagent-dev/website/pull/691) | redesign homepage around kagent on Agent Substrate | sebbycorp | 2026-11-08 |
| website | [#707](https://github.com/kagent-dev/website/pull/707) | fix duplicate navbar logo on mobile | sebbycorp | 2026-11-08 |

All 10 merged PRs shown. `#3340` is kagent's only `main` commit in window — **kagent has neither bumped to substrate v1.0.1 nor started importing the installer**. None in kmcp, doc2vec, khook, mockllm, community, byo-ext.

## Watch list (kagent Enterprise alpha12 / Substrate 0.6 on omni-viper)

- **Pin still `v1.0.0`:** kagent `main`'s `go/go.mod` ends `replace go.ate.dev/substrate => github.com/kagent-dev/substrate v1.0.0`. Watch for the bump to `v1.0.1` (or `v1.0.2` once `#159` ships) and a kagent `v1.0.0` tag before demoing the installer end to end.
- **[#165](https://github.com/kagent-dev/substrate/pull/165) (open) breaks saved configs:** drops Helm `ResetThenReuseValues`, so values are chart+config only — stale values get removed and switching `postgres`/`objectStore` mode on upgrade finally works. But `install` now refuses once `substrate` is deployed, and **`upgrade` needs the `-c` config used at install** (without it, bundled defaults land on an external DB). Keep the config with the cluster; `--dry-run` first.
- **[#166](https://github.com/kagent-dev/substrate/pull/166) (open):** with no `otel.endpoint`, ate-api-server/atelet/ate-controller/credential-provider push to `localhost:4317` and log a connection error ~every minute; fix sets metrics to `prometheus` (keeps `:8080`). Long demo sessions currently spam logs.
- **`#152`'s credential-provider values are not in the `v1.0.1` tag** — only `main` and `release-1.0` via `#159`. Test `credentialProvider.enabled: false` / `atenetEgress.extraCredentialProviders` off a `v1.0.2`/`main` chart; confirm fail-closed with zero providers.
- Known `v1.0.1` lifecycle gaps: `upgrade` does not roll WorkerPool worker images; `uninstall` keeps CRDs/CA keys/secrets/PVC unless `--include-crds`/`--purge`.
- Open substrate PRs to test on omni-viper: AKS installer profile [#161](https://github.com/kagent-dev/substrate/pull/161), scope the credential provider's Secret reader to listed namespaces [#131](https://github.com/kagent-dev/substrate/pull/131), extra CAs for egress upstream TLS [#101](https://github.com/kagent-dev/substrate/pull/101), `podLabels` [#48](https://github.com/kagent-dev/substrate/pull/48), fork-only package names [#156](https://github.com/kagent-dev/substrate/pull/156).
- Open kagent fixes for a local build: runtime trust [#3321](https://github.com/kagent-dev/kagent/pull/3321), interrupted checkpoints [#3519](https://github.com/kagent-dev/kagent/pull/3519), plugin caching [#3503](https://github.com/kagent-dev/kagent/pull/3503), Claude dual-manifest plugins [#3570](https://github.com/kagent-dev/kagent/pull/3570), `overlapSize` [#3574](https://github.com/kagent-dev/kagent/pull/3574), skills-init checkout [#3572](https://github.com/kagent-dev/kagent/pull/3572), Claude egress [#3439](https://github.com/kagent-dev/kagent/pull/3439).
- Unpublished docs for shipped features — don't quote until merged: Anthropic self-hosted sandboxes ([website#706](https://github.com/kagent-dev/website/pull/706)) and the `kubectl ate` blog post ([website#708](https://github.com/kagent-dev/website/pull/708), publish date 2026-11-10).
- **kmcp [#144](https://github.com/kagent-dev/kmcp/pull/144) (open):** drops `+kubebuilder:default=1` on `MCPServerDeployment.Replicas` (both CRD copies) so an HPA isn't reset to 1 each reconcile — CRD change; test before enabling autoscaling.
- Carried over: Substrate unsupported on kagent 0.10.x; arm64 Wolfi gVisor PAC issue; tools `LOG_LEVEL` open ([tools#100](https://github.com/kagent-dev/tools/pull/100)).
