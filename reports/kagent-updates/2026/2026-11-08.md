# kagent-updates - 2026-11-08

**Substrate v1.0.1 shipped with the `kubectl ate` install/upgrade/uninstall/TUI/config work backported to `release-1.0`; kagent added Anthropic self-hosted sandboxes; 16 PRs merged across 3 repos.**

Window: 2026-11-07 → 2026-11-08 (since the 2026-11-07 digest; substrate#137 and the rc2→rc3 runbook items reported there are not repeated).

## Releases

| Repo | Tag | Date | Highlights |
| --- | --- | --- | --- |
| substrate | [v1.0.1](https://github.com/kagent-dev/substrate/releases/tag/v1.0.1) | 2026-11-08 | Patch on the GA line (`v1.0.0...v1.0.1`): `kubectl ate` lifecycle + helm-e2e backport ([#148](https://github.com/kagent-dev/substrate/pull/148)), upgrade-e2e image-check fix ([#150](https://github.com/kagent-dev/substrate/pull/150)); OCI charts at `1.0.1` (`substrate-crds`, `substrate`, `substrate-podcert`), amd64/arm64 images, `kubectl-ate` attached with `.sha256` |

Latest per main repo: substrate stable `v1.0.1` (last 0.x `v0.6.0`), kagent `v0.10.6` / `v1.0.0-rc3` (no new kagent tag in window), tools `v0.3.1` (outside window). No releases in window elsewhere in the org.

## Highlights

- **Substrate finally has a one-command installer.** `kubectl ate install` applies the four releases in order from charts embedded in the binary, creates CA pools, identity secrets and object-store credentials, defaults to a single-replica evaluation PostgreSQL (`--postgres external` validates connection Secrets), stages Kata/Cloud Hypervisor assets for `--microvm`, and resumes an interrupted install; `kubectl ate status` lists releases and workloads ([#138](https://github.com/kagent-dev/substrate/pull/138)). This is the path to run omni-viper on instead of the manual `ate-setup` + Helm sequence.
- **Upgrade/uninstall scripted against the version-skew policy.** `kubectl ate upgrade` goes cert controller → atelet → CRDs → substrate, keeps per-release values, refuses downgrades, skipped minors and CRD changes that would strand stored versions; `--dry-run` prints manifest diffs ([#139](https://github.com/kagent-dev/substrate/pull/139)). **Known gap: it does not roll WorkerPool worker images.** `uninstall` deletes WorkerPools then releases in reverse but keeps CRDs, CA keys, secrets and volumes unless `--include-crds`/`--purge`; the RustFS PVC is kept via `helm.sh/resource-policy: keep` *in the release manifest* — annotating the live PVC does nothing ([#140](https://github.com/kagent-dev/substrate/pull/140)).
- **Flags → YAML config.** Install/upgrade/uninstall read one config (`-c install.yaml`) or stdin (`-c -`) with per-release `valuesFiles`/`values` (`releases.all` except `substrate-crds`). This **replaces** `--postgres`, `--object-store`, `--microvm`, `--microvm-asset-bucket`, `-f`, `--set`, `--timeout`; unknown fields are hard errors. Rewrite demo/CI scripts before the next redeploy ([#144](https://github.com/kagent-dev/substrate/pull/144)).
- **Demo-friendly TUI**: live step list, Ctrl+C cancel, and a wizard that prints the equivalent command; `--no-tui`/`-v` stay plain ([#141](https://github.com/kagent-dev/substrate/pull/141)).
- **New kagent capability — Anthropic self-hosted sandboxes.** A leader-elected controller loop polls Anthropic for sessions requesting sandboxes, provisions a sandbox running the `ant` CLI, receives bash tool calls, and suspends sandboxes whose CLI exited. All config via env vars; the author flags install UX as "quite bad" — demo-able, not turnkey ([kagent#3521](https://github.com/kagent-dev/kagent/pull/3521)).
- **Egress credential providers chart-configurable** — merged to main *after* v1.0.1: `credentialProvider.enabled` gates the bundled k8s provider and its `k8s.io` route; `atenetEgress.extraCredentialProviders` adds `{name, address}` peers over the same mTLS, matching `ate-setup --credential-provider` (e.g. `credprovider-gcp-secret-manager`). No providers → injection fails closed; chart rejects `k8s.io`, duplicates, portless addresses ([substrate#152](https://github.com/kagent-dev/substrate/pull/152)).
- **0.10.x can no longer hijack "latest"**: the `release/v0.10.x` workflow is `workflow_dispatch`-driven with tag validation and `make_latest: false` — previously each 0.10 patch stole the marker the CLI install script resolves ([kagent#3567](https://github.com/kagent-dev/kagent/pull/3567)).
- **Canonical install one-liner is live**: `kagent.dev/install` 307-redirects to `scripts/get-kagent` on main, so `curl -sL https://kagent.dev/install | bash && kagent install` is the quick-start tab ([website#704](https://github.com/kagent-dev/website/pull/704)).
- **Helm e2e now matches the PR e2e**: every suite on both runtimes, `egresscredinject` on both, `E2E_PARALLELISM=8`, 120-min job ([substrate#146](https://github.com/kagent-dev/substrate/pull/146)).

## Merged PRs by repo

| Repo | PR | Title | Author | Merged |
| --- | --- | --- | --- | --- |
| kagent | [#3521](https://github.com/kagent-dev/kagent/pull/3521) | Anthropic Self Hosted Sandboxes | Stevenjin8 | 2026-11-07 |
| substrate | [#138](https://github.com/kagent-dev/substrate/pull/138) | kubectl-ate: add install and status commands | EItanya | 2026-11-07 |
| substrate | [#139](https://github.com/kagent-dev/substrate/pull/139) | kubectl-ate: add upgrade command | EItanya | 2026-11-07 |
| substrate | [#140](https://github.com/kagent-dev/substrate/pull/140) | kubectl-ate: add uninstall command | EItanya | 2026-11-07 |
| substrate | [#144](https://github.com/kagent-dev/substrate/pull/144) | kubectl-ate: configure install/upgrade/uninstall with a config file | EItanya | 2026-11-07 |
| substrate | [#141](https://github.com/kagent-dev/substrate/pull/141) | kubectl-ate: show install progress in a TUI | EItanya | 2026-11-07 |
| substrate | [#148](https://github.com/kagent-dev/substrate/pull/148) | [release-1.0] Backport kubectl ate install, upgrade and uninstall | EItanya | 2026-11-07 |
| substrate | [#152](https://github.com/kagent-dev/substrate/pull/152) | charts: let egress use other credential providers | EItanya | 2026-11-08 |
| substrate | [#146](https://github.com/kagent-dev/substrate/pull/146) | helm-e2e: run every suite on both runtimes | EItanya | 2026-11-07 |
| kagent | [#3567](https://github.com/kagent-dev/kagent/pull/3567) | ci(release): release v0.10.x by dispatch, never mark it latest | EItanya | 2026-11-07 |
| website | [#704](https://github.com/kagent-dev/website/pull/704) | Add docs favicon and kagent.dev/install redirect | EItanya | 2026-11-07 |
| substrate | [#149](https://github.com/kagent-dev/substrate/pull/149) / [#150](https://github.com/kagent-dev/substrate/pull/150) | Skip third-party images in the upgrade e2e image check (+ release-1.0) | EItanya | 2026-11-08 |
| substrate | [#147](https://github.com/kagent-dev/substrate/pull/147) | ci: restore the Go build cache from main in run-tests and helm-e2e | EItanya | 2026-11-07 |
| substrate | [#151](https://github.com/kagent-dev/substrate/pull/151) | docs: list substrate-atelet among the published charts | EItanya | 2026-11-08 |
| kagent | [#3557](https://github.com/kagent-dev/kagent/pull/3557) | chore: add GitHub PR and issue creation skills | EItanya | 2026-11-08 |

All 16 merged PRs shown. Only those two kagent main commits landed in window, so **kagent has not yet bumped to Substrate v1.0.1**. None in tools, doc2vec, kmcp, khook, mockllm, community, byo-ext.

## Watch list

- **Pin substrate `v1.0.1` for alpha12**, not `v1.0.0` — the `kubectl ate` lifecycle is what makes a repeatable omni-viper rebuild possible. kagent main still pins `v1.0.0` ([#3555](https://github.com/kagent-dev/kagent/pull/3555)); watch for the bump and a kagent `v1.0.0` tag.
- **Upgrade won't roll WorkerPool worker images** — still manual per the skew policy after atelet; verify on omni-viper.
- **#152 chart values are not in v1.0.1** — need `main`/`1.0.2` before testing `credentialProvider.enabled: false` or BYO providers; confirm fail-closed with zero providers.
- **Install surface still moving**: open follow-ups [#153](https://github.com/kagent-dev/substrate/pull/153) (typed postgres/objectStore config) and [#154](https://github.com/kagent-dev/substrate/pull/154) (cluster access and charts from the caller) — retest any saved `-c` config after they land.
- **Anthropic sandboxes**: env-var-only config, docs still open at [website#706](https://github.com/kagent-dev/website/pull/706); single leader-elected controller, so test controller restarts before demoing suspend/resume.
- **Docs pre-GA**: `1.0.0 release switchover` ([website#703](https://github.com/kagent-dev/website/pull/703)), install-docs update ([website#705](https://github.com/kagent-dev/website/pull/705)) and the onboarding PR itself ([kagent#3566](https://github.com/kagent-dev/kagent/pull/3566)) are open — verify the copied one-liner before a customer walkthrough.
- **Open fixes worth a local test build**: interrupted checkpoint recovery ([#3519](https://github.com/kagent-dev/kagent/pull/3519)), Postgres login retries as Unavailable ([#3550](https://github.com/kagent-dev/kagent/pull/3550)), Anthropic streaming tool-call order ([#3568](https://github.com/kagent-dev/kagent/pull/3568)), duplicate `RemoteMCPServer` headers ([#3564](https://github.com/kagent-dev/kagent/pull/3564)).
- **Carried over**: Substrate unsupported on kagent 0.10.x; arm64 Wolfi gVisor PAC issue; tools `LOG_LEVEL` fix still open ([tools#100](https://github.com/kagent-dev/tools/pull/100)).
