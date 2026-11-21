# k8s-cve - 2026-11-21

**Verdict: ACTION NEEDED** — 2 unpatched CRITICAL advisories in in-cluster control-plane components (Argo CD v3.5.3 CVE-2026-77459; local-path-provisioner v0.0.31 CVE-2025-62878), plus an unpatched HIGH cross-node RCE in flannel v0.27.2, an unpatched HIGH in the node's containerd 2.2.7, and 12 unpatched CVEs in CoreDNS v1.12.3.

- **Cluster:** `omni-viper` — Kubernetes **v1.36.4** (node `kubeletVersion=v1.36.4`; all static control-plane pods `:v1.36.4`)
- **Single node:** `talos-cyv-ekb` (control-plane) · Talos **v1.13.10** · kernel `6.18.48-talos` (amd64) · `containerd://2.2.7`
- **Date (America/Toronto):** 2026-11-21 (checked 14:32 UTC = 09:32 EST — same calendar date in both zones; `zoneinfo` is absent in the sandbox so the conversion was verified by offset rather than by `TZ=`)
- **Source:** OSV.dev `https://api.osv.dev/v1/query` via BusyBox wget · **OSV queries used: 21 / 25**
- **Mode:** read-only. No cluster resource was created, modified, or deleted; no Secret data was read or printed.
- Every row below is copied from an OSV `/v1/query` response collected in this session. Zero-hit responses were verified to be literal `{}` bodies (not timeouts/HTTP errors) before being called clean.

---

## 1. Deduplicated image inventory

| Namespace(s) | Images |
|---|---|
| kube-system | `registry.k8s.io/coredns/coredns:v1.12.3`, `registry.k8s.io/kube-proxy:v1.34.11`, `registry.k8s.io/kube-apiserver:v1.36.4` (+ scheduler / controller-manager `:v1.36.4`), `ghcr.io/siderolabs/flannel:v0.27.2` |
| argocd | `quay.io/argoproj/argocd:v3.5.3` (server, repo-server, application-controller, applicationset, notifications), `ghcr.io/dexidp/dex:v2.45.1`, `public.ecr.aws/docker/library/redis:8.2.3-alpine` |
| cert-manager | `quay.io/jetstack/cert-manager-controller:v1.21.2`, `-cainjector:v1.21.2`, `-webhook:v1.21.2` |
| external-secrets | `ghcr.io/external-secrets/external-secrets:v2.9.0` (operator, cert-controller, webhook) |
| kagent | `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12`, `.../solo-enterprise/solo-enterprise-ui-frontend:1.0.0-alpha12`, `.../solo-enterprise-autoauth:v0.2.2`, `ghcr.io/kagent-dev/kagent/tools:0.3.0`, `.../substrate-enterprise/ateworker-gvisor:v0.6.0-b15382d4`, `clickhouse/clickhouse-server:26.3.17-alpine`, `nginx:1.27-alpine`, `python:3.13-slim`, `127.0.0.1:5000/fortigate-mcp:dev` |
| ate-system | `.../substrate-enterprise/ateapi:v0.6.0-b15382d4`, `atecontroller`, `ate-idp-server`, `atelet`, `kubernetes-secrets` @ `v0.6.0-b15382d4`, `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` (atenet-router/egress), `rustfs/rustfs:1.0.0-beta.3`, `docker.io/otel/opentelemetry-collector-contrib:0.161.0`, `.../solo-enterprise-ui-{frontend,backend}:0.5.9`, `.../solo-enterprise-autoauth:v0.2.3` |
| agentgateway-system | `.../enterprise-agentgateway/agentgateway-enterprise:2026.11.1` (proxy + bedrock-proxy), `enterprise-agentgateway-controller:2026.11.1`, `ext-auth-service`, `rate-limiter`, `waf-server` @ `2026.11.1`, `.../redis:8.6.8-alpine`, `docker.io/otel/opentelemetry-collector-contrib:0.161.0`, `clickhouse/clickhouse-server:26.3.35-alpine` |
| podcertificate-controller-system | `.../substrate-enterprise/podcertcontroller:v0.6.0-b15382d4` |
| local-path-storage | `rancher/local-path-provisioner:v0.0.31` |
| tailscale | `ghcr.io/tailscale/tailscale:v1.102.4` |
| postgres | `pgvector/pgvector:pg16` |
| registry / remarkable-diary | `registry:2`, `python:3.11-slim` |

> **Skew note:** the `kube-proxy` image is pinned at **v1.34.11** while the control plane runs **v1.36.4** — a two-minor drift (Talos-pinned), so kube-proxy inherits a different OSV version than the rest of the cluster.

---

## 2. CVE table

Severity is the label reported by OSV itself (`database_specific.severity` / `normalized_severity`); the CVSS vector is quoted verbatim where OSV returns one. "Unrated by OSV" = the record carries no severity field at all — no score has been inferred.

### Action needed (unpatched at the running version)

| Component | Version | CVE / advisory | Severity | Fixed version |
|---|---|---|---|---|
| **Argo CD** `quay.io/argoproj/argocd:v3.5.3` | 3.5.3 | **CVE-2026-77459** · GHSA-fmxq-cgp8-87wp · GO-2026-6784 — AppProject destination-namespace/resource restrictions bypassed by PreDelete/PostDelete hooks | **CRITICAL** (`CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H`) | **3.5.4** (also 3.4.10 / 3.3.15) |
| Argo CD | 3.5.3 | **CVE-2026-55797** · GHSA-j6cw-g6p4-7hch · GO-2026-6701 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | **HIGH** (`CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H`) | **3.5.4** |
| **local-path-provisioner** `rancher/local-path-provisioner:v0.0.31` | 0.0.31 | **CVE-2025-62878** · GHSA-jr3w-9vfr-c746 · GO-2026-4425 — path traversal via `parameters.pathPattern` | **CRITICAL** | **0.0.34** |
| local-path-provisioner | 0.0.31 | **CVE-2026-44543** · GHSA-7fxv-8wr2-mfc4 · GO-2026-5218 — HelperPod template injection | **HIGH** | **0.0.36** |
| **flannel** `ghcr.io/siderolabs/flannel:v0.27.2` | 0.27.2 | **CVE-2026-32241** · GHSA-vchx-5pr6-ffx2 · GO-2026-4894 — cross-node RCE via extension backend BackendData injection | **HIGH** (`CVSS:3.1/AV:N/AC:H/PR:L/UI:N/S:U/C:H/I:H/A:H`) | **0.28.2** |
| **containerd** (node runtime `2.2.7`) | 2.2.7 | **CVE-2026-95839** · GHSA-vwhp-8p66-mq4f — image-triggered daemon crash via malformed digest in subject-annotated descriptor | **HIGH** (`CVSS:4.0/AV:N/AC:L/AT:N/PR:L/UI:N/VC:N/VI:N/VA:H`) | **2.2.10** |
| containerd | 2.2.7 | **CVE-2026-53493** · GHSA-pg57-6jwg-q645 · GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | MODERATE (CVSS:4.0, `AV:N/…/VA:L`) | **2.2.9** |
| containerd | 2.2.7 | **CVE-2026-53495** · GHSA-7jxh-36q5-gcqv · GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | MODERATE (CVSS:4.0, `AV:L/…/VA:H`) | **2.2.8** |
| containerd | 2.2.7 | **CVE-2026-95838** · GHSA-rp3h-jf77-q9p4 · GO-2026-6796 — unpack DoS via repeated opaque whiteouts | MODERATE (CVSS:4.0, `AV:L/…/UI:A`) | **2.2.8** |
| **CoreDNS** `registry.k8s.io/coredns/coredns:v1.12.3` | 1.12.3 | **CVE-2026-86003** · GHSA-9gm5-9rfh-m6vx · GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | **HIGH** | **1.14.7** |
| CoreDNS | 1.12.3 | **CVE-2026-82399** · GHSA-mrg3-qvqr-jw29 · GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | **1.14.7** |
| CoreDNS | 1.12.3 | **CVE-2026-26017** · GHSA-c9v3-4pv7-87pr · GO-2026-4630 — ACL bypass | **HIGH** | **1.14.2** |
| CoreDNS | 1.12.3 | **CVE-2026-26018** · GHSA-h75p-j8xm-m278 · GO-2026-4635 — loop-detection DoS | **HIGH** | **1.14.2** |
| CoreDNS | 1.12.3 | **CVE-2026-32934** · GHSA-2wpx-qpw2-g5h5 · GO-2026-4969 — DoQ worker pool does not bound stream backlog | **HIGH** | **1.14.3** |
| CoreDNS | 1.12.3 | **CVE-2026-32936** · GHSA-63cw-r7xf-jmwr · GO-2026-5164 — DoH GET oversized `dns=` param CPU/memory amplification | **HIGH** | **1.14.3** |
| CoreDNS | 1.12.3 | **CVE-2026-33190** · GHSA-qhmp-q7xh-99rh · GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | **HIGH** | **1.14.3** |
| CoreDNS | 1.12.3 | **CVE-2026-35579** · GHSA-vp29-5652-4fw9 · GO-2026-5667 — TSIG auth bypass on gRPC and QUIC | **HIGH** | **1.14.3** |
| CoreDNS | 1.12.3 | **CVE-2026-33489** · GHSA-h8mm-c463-wjq3 · GO-2026-5417 — transfer-stanza lexicographic compare (subzone ACL bypass) | **HIGH** | **1.14.3** |
| CoreDNS | 1.12.3 | **CVE-2025-58063** · GHSA-93mf-426m-g6x9 · GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | **HIGH** | **1.12.4** |
| CoreDNS | 1.12.3 | **CVE-2025-68151** · GHSA-527x-5wrf-22m2 · GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | **1.14.0** |
| CoreDNS | 1.12.3 | **CVE-2026-62994** · GHSA-74w3-63xv-x9mv · GO-2026-6805 — `k8s_external` headless AXFR empty batch panics `transfer` plugin | LOW | **1.14.5** |
| **Kubernetes** (apiserver, controller-manager, scheduler) | v1.36.4 | **CVE-2025-1767** · GO-2025-3521 · GHSA-3wgm-2gw2-vh5m — GitRepo volume inadvertent local repository access | Unrated by OSV | none — OSV range is open-ended (`introduced: 0`), no fixed release ≤ 1.36.4 |
| Kubernetes | v1.36.4 | **CVE-2024-7598** · GO-2025-3547 · GHSA-r56h-j38w-hrqq — kube-apiserver race condition | Unrated by OSV | none — OSV range open-ended (`introduced: 1.3.0`) |
| **kube-proxy** image | v1.34.11 | CVE-2025-1767, CVE-2024-7598 — the same two `k8s.io/kubernetes` records, re-returned for 1.34.11 | Unrated by OSV | none (also two minors behind the control plane) |
| **Talos Linux** (node OS) | v1.13.10 | **CVE-2026-77564** · GHSA-7cxh-94cf-mfp6 — `talosctl support` bundle does not redact registry credentials | LOW (`CVSS:3.1` low; GHSA `severity: LOW`) | OSV fixed event is a **1.14.0-alpha prerelease** commit, not a GA patch → treat as *no GA fix yet* |

### Checked and clean at the running version (OSV returned literal `{}`)

| Component | Version | OSV query | Result |
|---|---|---|---|
| cert-manager (controller/cainjector/webhook) | v1.21.2 | Go · `github.com/cert-manager/cert-manager` | ✅ no vulnerabilities |
| opentelemetry-collector-contrib | 0.161.0 | Go · `github.com/open-telemetry/opentelemetry-collector-contrib` | ✅ no vulnerabilities |
| kagent (public OSS module) | 0.3.0 | Go · `github.com/kagent-dev/kagent` | ✅ no vulnerabilities |
| substrate (public OSS module) | 0.6.0 | Go · `github.com/kagent-dev/substrate` | ✅ no vulnerabilities *(covers only the public module — **not** the closed-source `v0.6.0-b15382d4` enterprise builds, see §3)* |
| Tailscale | v1.102.4 | Go · `tailscale.com` | ✅ no vulnerabilities |

### Queried but **not** affected (Go-range false positives worth recording)

| Component | Version | Advisory | Why it does not apply |
|---|---|---|---|
| external-secrets (operator/cert-controller/webhook) | **v2.9.0** | GO-2026-5723 · CVE-2026-42875 · GHSA-wv26-88m5-6h59 — namespace isolation bypass in CAProvider ConfigMap resolution | The record's Go range is open-ended (`introduced: 0`), but its ecosystem range / prose is *"before v2.4.0"*. v2.9.0 ≫ 2.4.0 → **not affected**. ✅ |
| Dex (`argocd-dex-server`) | **v2.45.1** | GO-2024-2476 · CVE-2024-23656 · GHSA-gr79-9v6v-gc9r — TLS config discarded, serves TLS 1.0/1.1 + weak ciphers | Applies only `>=2.37.0 <2.38.0`; v2.45.1 is above it → **not affected**. ✅ |

---

## 3. Not checked

No CVE claim is made for anything in this list. Images below were unmappable to an OSV package/version, or only got an inconclusive OSS-Fuzz/coverage-level miss.

| Image / component | Why not checked |
|---|---|
| `.../substrate-enterprise/ateapi`, `atecontroller`, `ate-idp-server`, `atelet`, `kubernetes-secrets`, `podcertcontroller`, `ateworker-gvisor` @ `v0.6.0-b15382d4` | **Closed-source Solo substrate/ate builds.** Tag is a build hash, not a release; no resolvable public module version. The `github.com/kagent-dev/substrate@0.6.0` miss above does **not** vouch for these images' vendored dependencies. |
| `.../enterprise-agentgateway/agentgateway-enterprise`, `enterprise-agentgateway-controller`, `ext-auth-service`, `rate-limiter`, `waf-server` @ `2026.11.1` | **Closed-source enterprise agentgateway** (Rust upstream); the enterprise fork is not published to any OSV ecosystem. |
| `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` (atenet-router, atenet-egress) | Pinned to a **commit-SHA tag** — no semver to query. The OSS-Fuzz attempt returned no records, which is a coverage gap, not a clean result. |
| `.../solo-enterprise/solo-enterprise-ui-{frontend,backend}:0.5.9`, `solo-enterprise-ui-frontend:1.0.0-alpha12`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3`, `kagent-enterprise-controller:1.0.0-alpha12` | **Closed-source Solo enterprise UI/auth components** — no upstream OSV package. |
| `rustfs/rustfs:1.0.0-beta.3` | Not present in OSV; beta S3-compatible store — use the vendor advisory feed instead. |
| `nginx:1.27-alpine`, `redis:8.2.3-alpine`, `redis:8.6.8-alpine`, `python:3.13-slim`, `python:3.11-slim`, `pgvector/pgvector:pg16`, `clickhouse/clickhouse-server:26.3.17-alpine` / `26.3.35-alpine`, `registry:2` | **Base-image CVEs need the exact image digest + OS release** to hit OSV's distro ecosystems. OSS-Fuzz-level probes for redis/nginx/clickhouse returned `{}` = no coverage, so they stay unchecked rather than being marked clean. |
| `127.0.0.1:5000/fortigate-mcp:dev` | Private registry `:dev` build — floating tag, no version to resolve. |
| Talos kernel `6.18.48-talos` | Kernel CVEs are tracked by Sidero, not in OSV's Go ecosystems. |

**Budget:** 21 / 25 OSV queries used.

---

## 4. Recommended actions (priority order)

1. **Argo CD `v3.5.3 → v3.5.4`** — closes CVE-2026-77459 (CRITICAL, AppProject tenancy bypass) and CVE-2026-55797 (HIGH, repo-server command injection). Top priority: repo-server consumes attacker-influenceable Git input, and the hook bypass defeats AppProject isolation across the GitOps tenant set.
2. **local-path-provisioner `v0.0.31 → ≥ v0.0.36`** — clears the CRITICAL path-traversal CVE-2025-62878 (fixed 0.0.34) *and* the HIGH HelperPod template-injection CVE-2026-44543 (fixed 0.0.36) in one bump.
3. **CoreDNS `v1.12.3 → ≥ v1.14.7`** — clears all 12 CVEs at once.
4. **Talos upgrade** — the node's flannel `v0.27.2` (HIGH cross-node RCE, fixed 0.28.2) and containerd `2.2.7` (HIGH CVE-2026-95839, fixed **2.2.10**) are both delivered by Talos images, so one node upgrade should clear both. Confirm the target Talos patch release actually carries flannel ≥ 0.28.2 and containerd ≥ 2.2.10 before rolling; otherwise pin those images separately. Note this also has to replace the *prerelease* OSV fix for CVE-2026-77564, so verify a GA Talos release exists rather than accepting an alpha-tagged fix.
5. **Reconcile the kube-proxy skew** (`v1.34.11` vs control plane `v1.36.4`) during the Talos upgrade.
6. **Kubernetes CVE-2025-1767 / CVE-2024-7598:** OSV shows open-ended affected ranges with **no fixed release**, and OSV assigns them no severity — do not read "unrated" as "low". Track the kubernetes-security announce threads referenced in GO-2025-3521 / GO-2025-3547; interim mitigation for the former is to forbid `gitRepo` volumes.
7. **Operational control for CVE-2026-77564:** do not share `talosctl support` bundles while they can contain private-registry credentials, until a GA Talos with the fix is in place.
8. **Next run:** add digest-aware distro queries (redis, nginx, python, pgvector, clickhouse, registry) so the base images can leave §3, and ask the Solo substrate / enterprise-agentgateway vendors for an advisory feed or SBOM, since 12 in-scope images are currently unverifiable from OSV.

> Every CVE row above comes from an OSV.dev `/v1/query` response collected in this session; raw responses are retained locally under `/tmp/osv/out.*` for audit. Images in §3 returned no usable OSV dataset and are explicitly **unchecked**, not clean.
