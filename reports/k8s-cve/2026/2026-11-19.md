# k8s-cve - 2026-11-19

**Verdict: ACTION NEEDED** — 2 exploitable-severity advisories in the GitOps control plane (Argo CD v3.5.3, CVSS 10.0/9.9, fix in 3.5.4), plus an unpatched cross-node RCE in flannel v0.27.2, an unpatched privilege-escalation path in local-path-provisioner v0.0.31, 3 unpatched CVEs in the node's containerd 2.2.7, and 12 unpatched CVEs in CoreDNS v1.12.3.

- **Cluster:** `omni-viper` — Kubernetes **v1.36.4**, single control-plane node `talos-cyv-ekb`
- **Node OS:** Talos **v1.13.10** · kernel `6.18.48-talos` (amd64) · runtime `containerd://2.2.7`
- **Date (America/Toronto):** 2026-11-19 · **Source:** OSV.dev API (`api.osv.dev/v1/query`) · **OSV queries used:** 23 / 25
- **Mode:** read-only inventory + OSV lookup; nothing in the cluster was modified.

---

## 1. Image inventory (deduplicated)

| Image | Namespace(s) |
|---|---|
| `registry.k8s.io/kube-proxy:v1.34.11`, `registry.k8s.io/coredns/coredns:v1.12.3` | kube-system |
| `ghcr.io/siderolabs/flannel:v0.27.2` | kube-system |
| `quay.io/argoproj/argocd:v3.5.3`, `ghcr.io/dexidp/dex:v2.45.1`, `public.ecr.aws/docker/library/redis:8.2.3-alpine` | argocd |
| `quay.io/jetstack/cert-manager-{controller,cainjector,webhook}:v1.21.2` | cert-manager |
| `ghcr.io/external-secrets/external-secrets:v2.9.0` | external-secrets |
| `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12`, `us-docker.pkg.dev/solo-public/solo-enterprise/solo-enterprise-ui-frontend:1.0.0-alpha12`, `ghcr.io/kagent-dev/kagent/tools:0.3.0`, `clickhouse/clickhouse-server:26.3.17-alpine`, `nginx:1.27-alpine`, `python:3.13-slim` | kagent |
| `us-docker.pkg.dev/solo-public/substrate-enterprise/ate{api,controller,let}:v0.6.0-b15382d4`, `.../ate-idp-server:v0.6.0-b15382d4`, `.../ateworker-gvisor:v0.6.0-b15382d4`, `.../kubernetes-secrets:v0.6.0-b15382d4`, `.../podcertcontroller:v0.6.0-b15382d4`, `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e`, `rustfs/rustfs:1.0.0-beta.3` | ate-system |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/{agentgateway-enterprise,enterprise-agentgateway-controller,ext-auth-service,rate-limiter,waf-server}:2026.11.0`, `.../redis:8.6.7-alpine`, `.../solo-enterprise-ui-{frontend,backend}:0.5.9`, `.../solo-enterprise-autoauth:v0.2.3`, `docker.io/otel/opentelemetry-collector-contrib:0.161.0`, `clickhouse/clickhouse-server:26.3.35-alpine` | agentgateway-system |
| `rancher/local-path-provisioner:v0.0.31` | local-path-storage |
| `pgvector/pgvector:pg16` | postgres |
| `ghcr.io/tailscale/tailscale:v1.102.4` | tailscale |

> Note: `kube-proxy:v1.34.11` runs two minor versions behind the v1.36.4 control plane (Talos-pinned skew).

---

## 2. CVE table

Severity is derived from the CVSS vector returned by OSV (`critical` ≥9.0, `high` 7.0–8.9, `medium` 4.0–6.9, `low` 0.1–3.9). "Unrated by OSV" = the OSV record carries no CVSS vector.

| Component | Version | CVE / advisory | Severity | Fixed version | Status |
|---|---|---|---|---|---|
| **Argo CD** (`quay.io/argoproj/argocd:v3.5.3`) | 3.5.3 | **CVE-2026-77459** · GHSA-fmxq-cgp8-87wp · GO-2026-6784 — AppProject destination-namespace/resource restrictions bypassed by PreDelete/PostDelete hooks | **Critical** (CVSS:3.1 10.0, `AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H`) | **3.5.4** | ❌ vulnerable |
| Argo CD | 3.5.3 | **CVE-2026-55797** · GHSA-j6cw-g6p4-7hch · GO-2026-6701 — repo-server command injection via crafted SSH repo SOCKS5 proxy URL | **Critical** (CVSS:3.1 9.9, `AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H`) | **3.5.4** | ❌ vulnerable |
| **flannel** (`ghcr.io/siderolabs/flannel:v0.27.2`) | 0.27.2 | **CVE-2026-32241** · GHSA-vchx-5pr6-ffx2 · GO-2026-4894 — cross-node RCE via extension backend BackendData injection | **High** (CVSS:3.1 8.5, `AV:N/AC:H/PR:L/UI:N/S:U/C:H/I:H/A:H`) | **0.28.2** | ❌ vulnerable |
| **containerd** (node runtime) | 2.2.7 | **CVE-2026-53493** · GHSA-pg57-6jwg-q645 · GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | **High** (CVSS:4.0, `AV:N/AC:L/AT:N/PR:N/UI:N/VA:L`) | **2.2.9** | ❌ vulnerable |
| containerd | 2.2.7 | **CVE-2026-53495** · GHSA-7jxh-36q5-gcqv · GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | **Medium** (CVSS:4.0, `AV:L/AC:L/PR:L/UI:N/VA:H`) | **2.2.8** | ❌ vulnerable |
| containerd | 2.2.7 | **CVE-2026-95838** · GHSA-rp3h-jf77-q9p4 · GO-2026-6796 — unpack DoS via repeated opaque whiteouts | **Low** (CVSS:4.0, `AV:L/AC:L/PR:N/UI:A/VA:L`) | **2.2.8** | ❌ vulnerable |
| **local-path-provisioner** (`rancher/local-path-provisioner:v0.0.31`) | 0.0.31 | **CVE-2025-62878** · GHSA-jr3w-9vfr-c746 · GO-2026-4425 — path traversal via `parameters.pathPattern` | **High** (CVSS:3.1 9.9, `AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H`) | **0.0.34** | ❌ vulnerable |
| local-path-provisioner | 0.0.31 | **CVE-2026-44543** · GHSA-7fxv-8wr2-mfc4 · GO-2026-5218 — HelperPod template injection | **High** (CVSS:3.1 7.7, `AV:N/AC:L/PR:H/UI:N/S:C/C:H/I:H/A:N`) | **0.0.36** | ❌ vulnerable |
| **CoreDNS** (`registry.k8s.io/coredns/coredns:v1.12.3`) | 1.12.3 | **CVE-2026-86003** · GHSA-9gm5-9rfh-m6vx · GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection | **High** (CVSS:3.1 8.2, `S:U/C:N/I:H/A:N`) | **1.14.7** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-26017** · GHSA-c9v3-4pv7-87pr · GO-2026-4630 — ACL bypass | **High** (CVSS:3.1 8.1, `PR:L/S:C/C:H`) | **1.14.2** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-33489** · GHSA-h8mm-c463-wjq3 · GO-2026-5417 | **High** (CVSS:3.1 8.1, `PR:N/S:U/C:H`) | **1.14.3** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-33190** · GHSA-qhmp-q7xh-99rh · GO-2026-5583 | **High** (CVSS:3.1 7.5, `PR:N/S:U/C:H`) | **1.14.3** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-35579** · GHSA-vp29-5652-4fw9 · GO-2026-5667 | **High** (CVSS:3.1 7.5, `PR:N/S:U/C:H`) | **1.14.3** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-82399** · GHSA-mrg3-qvqr-jw29 · GO-2026-6507 | **High** (CVSS:3.1 7.5, `AV:N/AC:L/PR:N/UI:N`) | **1.14.7** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-26018** · GHSA-h75p-j8xm-m278 · GO-2026-4635 | **High** (CVSS:3.1 7.5, `AV:N/AC:L/PR:N/UI:N/A:H`) | **1.14.2** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-32934** · GHSA-2wpx-qpw2-g5h5 · GO-2026-4969 — DoQ worker pool does not bound stream backlog | **High** (CVSS:3.1 7.5 / CVSS:4.0) | **1.14.3** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-32936** · GHSA-63cw-r7xf-jmwr · GO-2026-5164 — DoH GET oversized `dns=` param CPU/memory amplification | **High** (CVSS:3.1 7.5 / CVSS:4.0) | **1.14.3** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2025-68151** · GHSA-527x-5wrf-22m2 · GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | **High** (CVSS:4.0) | **1.14.0** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2025-58063** · GHSA-93mf-426m-g6x9 · GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | **Medium** (CVSS:3.1 7.1, `PR:L`) | **1.12.4** | ❌ vulnerable |
| CoreDNS | 1.12.3 | **CVE-2026-62994** · GHSA-74w3-63xv-x9mv · GO-2026-6805 — `k8s_external` headless AXFR empty batch panics `transfer` plugin | **Medium** (CVSS:3.1 5.3, `AC:H`) | **1.14.5** | ❌ vulnerable |
| **Kubernetes** (apiserver/controller-manager/scheduler) | v1.36.4 | **CVE-2025-1767** · GO-2025-3521 · GHSA-3wgm-2gw2-vh5m — GitRepo volume inadvertent local repository access | Unrated by OSV | open-ended range (`introduced: 0`) — not patched in any release ≤ 1.36.4 | ⚠️ exposed, no fix |
| Kubernetes | v1.36.4 | **CVE-2024-7598** · GO-2025-3547 · GHSA-r56h-j38w-hrqq — kube-apiserver race condition | Unrated by OSV | open-ended range (`introduced: 1.3.0`) — not patched in any release ≤ 1.36.4 | ⚠️ exposed, no fix |
| **kube-proxy** image | v1.34.11 | CVE-2025-1767, CVE-2024-7598 (same two OSV records as above, returned for 1.34.11) | Unrated by OSV | none | ⚠️ also on skewed image |
| **Talos Linux** (node OS) | v1.13.10 | **CVE-2026-77564** · GHSA-7cxh-94cf-mfp6 — `talosctl support` bundle does not redact private-registry credentials | **Low** (CVSS:3.1 2.6, `AV:L/PR:N/UI:R/C:L`) — GHSA `severity: LOW` | Talos **1.14+** (OSV fixed event is a 1.14.0-alpha preref) | ⚠️ low risk; operational control below |

### Checked and clean (zero OSV results at the running version)

| Component | Version | Query (ecosystem/package) | Result |
|---|---|---|---|
| cert-manager (controller/cainjector/webhook) | v1.21.2 | Go · `github.com/cert-manager/cert-manager` | ✅ no vulnerabilities |
| external-secrets (operator/cert-controller/webhook) | v2.9.0 | Go · `github.com/external-secrets/external-secrets` | ✅ not affected — only record **GO-2026-5723 / CVE-2026-42875** (namespace-isolation bypass in CAProvider ConfigMap resolution) is fixed in **2.4.0**; v2.9.0 is above the fixed version |
| Dex (argocd-dex-server) | v2.45.1 | Go · `github.com/dexidp/dex` | ✅ not affected — only record **GO-2024-2476 / CVE-2024-23656** covers only `>=2.37.0 <2.38.0` |
| opentelemetry-collector-contrib | 0.161.0 | Go · `github.com/open-telemetry/opentelemetry-collector-contrib` | ✅ no vulnerabilities |
| kagent (OSS module) | 0.3.0 | Go · `github.com/kagent-dev/kagent` | ✅ no vulnerabilities |
| Tailscale | v1.102.4 | Go · `tailscale.com` | ✅ no vulnerabilities |

---

## 3. Not checked

Images that could not be mapped to an OSV package/version, deliberately deprioritised, or that returned no usable dataset. No CVE claims are made for any of these.

| Image | Why not checked |
|---|---|
| `us-docker.pkg.dev/solo-public/substrate-enterprise/ate-api`, `atecontroller`, `atelet`, `ate-idp-server`, `ateworker-gvisor`, `kubernetes-secrets`, `podcertcontroller` @ `v0.6.0-b15382d4` | **Closed-source Solo substrate/ate builds**; no public Go module path resolvable in OSV (the `b15382d4` build hash is not a release tag) |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/*` @ `2026.11.0` (agentgateway proxy, controller, ext-auth, rate-limiter, waf-server) | **Closed-source enterprise agentgateway**; upstream agentgateway is Rust and the enterprise fork is not published to any OSV ecosystem |
| `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` (atenet-router / atenet-egress) | Pinned to a **commit-SHA tag**; not a semver release, so no OSV version to query |
| `us-docker.pkg.dev/solo-public/solo-enterprise/solo-enterprise-ui-frontend`, `-ui-backend`, `solo-enterprise-autoauth` @ `1.0.0-alpha12` / `0.5.9` / `v0.2.3`, `kagent-enterprise-controller:1.0.0-alpha12` | **Closed-source Solo enterprise/UI components**; no upstream OSV package |
| `rustfs/rustfs:1.0.0-beta.3` | Not in OSV; also a **beta** S3-compatible store — recommend vendor advisory feed instead |
| `clickhouse/clickhouse-server:26.3.17-alpine` and `26.3.35-alpine` | **OSV has no ClickHouse coverage**: no `ClickHouse` ecosystem exists (HTTP 400) and the `OSS-Fuzz` query for both versions returned no records |
| `registry.k8s.io/kube-proxy:v1.34.11` beyond the two k8s.io/kubernetes records above | Queried, but OSV only carries the two open-ended advisories listed |
| `public.ecr.aws/docker/library/redis:8.2.3-alpine`, `us-docker.pkg.dev/.../redis:8.6.7-alpine` | Base-image CVEs live in OSV's distro ecosystems, which need the exact **digest + OS release** to query — deliberately deprioritised to stay in budget |
| `nginx:1.27-alpine` (kagent-ui-tls) | OSV query rejected with **HTTP 400** (unmappable package/ecosystem pair); needs the Alpine digest-level query |
| `python:3.13-slim` (typesafe-jev-mcp), `python:3.11-slim` (agent-desk) | Out-of-platform lab/workload images; PyPI/stdlib CVEs need the image's exact digest — deprioritised |
| `pgvector/pgvector:pg16` (postgres) | Out-of-platform; Postgres CVEs need the exact minor (`16.x`) which the `pg16` tag does not expose |
| `127.0.0.1:5000/fortigate-mcp:dev` (fortigate-mcp), `registry:2`, `ghcr.io/kagent-dev/substrate/agentgateway` sidecars | **Private / `:dev` / floating-tag builds** — no resolvable version to check |
| Talos kernel `6.18.48-talos` | OS/kernel CVEs are tracked by Sidero, not in OSV Go ecosystems |

**Budget:** 23 / 25 OSV queries used.

---

## 4. Recommended actions (priority order)

1. **Bump Argo CD `v3.5.3 → v3.5.4`** — closes both CVE-2026-77459 (10.0) and CVE-2026-55797 (9.9). Highest priority: repo-server is directly reachable from Git input and the hook bypass defeats AppProject tenancy.
2. **Bump local-path-provisioner `v0.0.31 → ≥ v0.0.36`** — closes both CVEs (9.9 path traversal, 7.7 helper-pod injection).
3. **Bump CoreDNS `v1.12.3 → ≥ v1.14.7`** — clears all 12 CVEs at once.
4. **Talos `v1.13.10 → 1.14.x patch release`** — raises flannel past **0.28.2** (cross-node RCE) and containerd past **2.2.9** in the same upgrade. Verify the Talos patch release actually carries flannel ≥ 0.28.2 / containerd ≥ 2.2.9, otherwise pin those images separately.
5. **Reconcile kube-proxy skew** (`v1.34.11` vs control plane `v1.36.4`) as part of the Talos upgrade.
6. **Operational control for CVE-2026-77564:** avoid sharing `talosctl support` bundles containing private-registry credentials until on Talos 1.14+ (which encrypts bundles to age recipients).
7. **Kubernetes CVE-2025-1767 / CVE-2024-7598:** OSV shows open-ended affected ranges with no fixed release — track the upstream kubernetes-security announce threads referenced in the OSV records; mitigate by not using `gitRepo` volumes.
8. **Next run:** add digest-aware distro queries for the base-image set (redis, nginx, python, pgvector, clickhouse) so those can leave the "not checked" list.

> Every row above is sourced from an OSV.dev `/v1/query` response collected in this session. Raw responses retained locally under `osv/` for audit.
