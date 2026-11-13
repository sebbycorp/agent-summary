# k8s-cve - 2026-11-13

**VERDICT: ACTION NEEDED** — 1 CRITICAL + several HIGH findings in Argo CD, CoreDNS, Flannel and local-path-provisioner; private (Solo/substrate/kagent-enterprise) images remain unchecked.

- **Cluster:** omni-viper (single node `talos-cyv-ekb`, Talos v1.13.10, kernel 6.18.48-talos, containerd 2.2.7)
- **Kubernetes:** kubelet v1.36.4 (Talos image) / **kube-proxy v1.34.11** ← version skew, see notes
- **Data source:** https://api.osv.dev (`/v1/query`), 25/25 queries budget used
- **Reported:** 2026-11-13 ~14:25 America/Toronto
- **Mode:** read-only; no cluster mutation, no Secret reads. Only `id` / `aliases` / `summary` / `severity` / `fixed` retained from each OSV response.

---

## 1. Findings (every row below comes from an OSV response received in this session)

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| **Argo CD** (`quay.io/argoproj/argocd:v3.5.3` — server, repo-server, application-controller, applicationset-controller, notifications-controller) | v3.5.3 | **CVE-2026-77459** / GO-2026-6784 / GHSA-fmxq-cgp8-87wp — AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete resource hooks | **CRITICAL** | **3.5.4** (also 3.3.15, 3.4.10, 3.6.0-rc2) |
| **Argo CD** (same image) | v3.5.3 | **CVE-2026-55797** / GO-2026-6701 / GHSA-j6cw-g6p4-7hch — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | **HIGH** | **3.5.4** (also 3.3.15, 3.4.10, 3.6.0-rc2) |
| **local-path-provisioner** (`rancher/local-path-provisioner:v0.0.31`, ns `local-path-storage`) | v0.0.31 | **CVE-2025-62878** / GO-2026-4425 / GHSA-jr3w-9vfr-c746 — Path traversal via `parameters.pathPattern` | **CRITICAL** | **0.0.34** |
| **local-path-provisioner** | v0.0.31 | **CVE-2026-44543** / GO-2026-5218 / GHSA-7fxv-8wr2-mfc4 — HelperPod template injection | **HIGH** | **0.0.36** |
| **Flannel** (`ghcr.io/siderolabs/flannel:v0.27.2`, DaemonSet `kube-system/kube-flannel`, vxlan backend) | v0.27.2 | **CVE-2026-32241** / GO-2026-4894 / GHSA-vchx-5pr6-ffx2 — cross-node RCE via extension-backend BackendData injection | **HIGH** | **0.28.2** |
| **CoreDNS** (`registry.k8s.io/coredns/coredns:v1.12.3`, ns `kube-system`, 2 replicas) | v1.12.3 | CVE-2026-82399 / GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-86003 / GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GO-2026-5583 — TSIG authentication bypass on DoT/DoH/DoH3/DoQ/gRPC | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GO-2026-5667 — TSIG authentication bypass on gRPC and QUIC transports | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32934 / GO-2026-4969 — DoQ worker pool does not bound stream backlog | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GO-2026-5164 — DoH GET oversized `dns=` parameter CPU/memory amplification | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GO-2026-5417 — transfer stanza selection uses lexicographic compare (subzone ACL bypass) | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-26017 / GO-2026-4630 — ACL bypass | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GO-2026-4635 — loop-detection denial of service | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GO-2026-6805 — `k8s_external` headless AXFR emits empty transfer batch, panics `transfer` plugin | LOW | 1.14.5 |
| CoreDNS | v1.12.3 | CVE-2025-58063 / GO-2025-3942 / GHSA-93mf-426m-g6x9 — DNS cache pinning via etcd lease-ID confusion | **HIGH** | 1.12.4 |
| **agentgateway** (`ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e` — `atenet-router`, `atenet-egress`) | commit `cd65181fd52e` | CVE-2026-29791 / GHSA-v2x6-wwfw-r2rq — missing parameter sanitisation in MCP→OpenAPI conversion (path/query/header injection) | MODERATE | 0.12.0 |

Note on agentgateway: the tag is a commit SHA, not a semver release, so OSV cannot reliably decide whether `cd65181fd52e` predates the 0.12.0 fix. **Treat as "review"** and confirm the commit date against the 0.12.0 release.

### Findings returned by OSV but assessed as NOT affecting the deployed version

These advisories were returned because the Go vulnDB range for the module is unbounded (`introduced: 0` with no `fixed` event, or an ecosystem range that ends below our version). I pulled the raw `ranges`/`custom_ranges` events to verify. No action, listed for transparency:

| Component | Version | Advisory | Why not counted |
|---|---|---|---|
| Kubernetes (k8s.io/kubernetes) | 1.36.4 | CVE-2025-1767 / GO-2025-3521 (GitRepo volume local repo access) | Unbounded Go range (`introduced: 0`, no `fixed`) matches every version incl. 1.36.4; advisory is long-fixed in the 1.32.x line. Informational only. |
| Kubernetes (k8s.io/kubernetes) | 1.36.4 | CVE-2024-7598 / GO-2025-3547 (kube-apiserver race condition) | Same unbounded-range artefact; fixed well before 1.36.4. Informational only. |
| kube-proxy (`registry.k8s.io/kube-proxy`) | v1.34.11 | same two advisories as above | Same artefact — **but the version skew itself is a real finding, see §2** |
| containerd (node runtime `containerd://2.2.7`) | 2.2.7 | CVE-2026-53492 / GO-2026-5064; CVE-2026-50195 / GO-2026-5338; CVE-2026-53489 / GO-2026-5622 (CRI checkpoint restore issues) | Verified `custom_ranges`: 2.2.0 → fixed **2.2.5**. 2.2.7 is already past the fix. |
| containerd | 2.2.7 | CVE-2026-95837 / GO-2026-6791 (checkpoint restore bypasses destination security context) | Verified `custom_ranges`: 2.1.0 → fixed **2.2.7**; 2.2.7 *is* the fixing release. |
| external-secrets (`ghcr.io/external-secrets/external-secrets`) | v2.9.0 (deployed) / v2.10.0 (cached) | CVE-2026-42875 / GO-2026-5723 / GHSA-wv26-88m5-6h59 (Namespace isolation bypass in CAProvider ConfigMap resolution) | Verified: `custom_ranges` end at fixed **2.4.0**, so 2.9.0/2.10.0 are outside the vulnerable range. Advisory is `review_status: UNREVIEWED`. No action. |
| dex (`ghcr.io/dexidp/dex:v2.45.1`, `argocd-dex-server`) | v2.45.1 | CVE-2024-23656 / GHSA-gr79-9v6v-gc9r (TLS config discarded, TLS 1.0/1.1 + weak ciphers) | Fixed in **2.38.0**; 2.45.1 is far newer. Not applicable. |
| cert-manager (controller / cainjector / webhook) | v1.21.2 | — | `{"vulns":[]}` → **clean** (OSV does have data for this module: a control query on 1.14.0 returned GHSA-r4pg-vg54-wxx4, so the clean result is meaningful). |
| Tailscale (`ghcr.io/tailscale/tailscale:v1.102.4`) | v1.102.4 | — | `{"vulns":[]}` → clean for `tailscale.com@1.102.4`. |

---

## 2. Non-CVE configuration observations worth a look

- **kube-proxy v1.34.11 vs kubelet v1.36.4** — a two-minor version skew on the same node. Not an OSV finding, but it means kube-proxy is two releases behind the security baseline applied to the rest of the control plane; align it with the Talos/K8s 1.36 patch level.
- **Stale in-service versions still on the node** (cached, and in some cases still referenced by workloads): `external-secrets:v2.9.0` vs cached `v2.10.0`; `argocd:v3.5.3`; `kagent-enterprise-controller:1.0.0-alpha3 / alpha10 / alpha12`; `kmcp-enterprise-controller:1.0.0-alpha3 / alpha10`; `substrate ateapi/atelet v0.5.0-alpha3 vs v0.6.0`; `ext-auth-service 2026.9.2 / 2026.10.0 / 2026.10.2`; `kagent/tools:0.2.1` and `:0.3.0`; `clickhouse-server:26.3.17-alpine` and `:26.3.35-alpine`; `kubelet:v1.35.8` alongside `v1.36.4`; `otel/opentelemetry-collector-contrib:0.153.0` and `:0.161.0`. Pruning the old tags reduces drift and rollback-to-vulnerable-image risk.
- **Mutable / unpinned tags in use:** `docker.io/nicolaka/netshoot:latest`, `docker.io/rustfs/rustfs` (digest only in cache), `127.0.0.1:5000/fortigate-mcp:dev`, `registry:2`. Unpinable and unauditable.
- **Node label drift:** `ate.dev/substrate-version: v0.1.0-dirty` on the node while the deployed substrate images are `v0.6.0-b15382d4`.
- **Private registry image** `127.0.0.1:5000/...` (fortigate-mcp, kagent dev controller, ate/* dev images) serving over a local registry — no signature/provenance check possible from here.

---

## 3. Recommended actions (in priority order)

1. **Bump Argo CD v3.5.3 → v3.5.4** (patches CVE-2026-77459 CRITICAL AppProject restriction bypass + CVE-2026-55797 HIGH repo-server command injection). This is the most exposed pair: Argo CD holds cluster-wide write credentials and the repo-server is the network-facing component.
2. **Bump CoreDNS v1.12.3 → ≥ v1.14.7** (12 advisories, 10 HIGH, incl. TSIG auth bypass and unauthenticated memory exhaustion). CoreDNS is the in-cluster resolution path for every workload.
3. **Bump Flannel v0.27.2 → ≥ v0.28.2** (CVE-2026-32241 cross-node RCE via BackendData injection).
4. **Bump local-path-provisioner v0.0.31 → ≥ v0.0.36** (CRITICAL path traversal + HIGH helper-pod template injection). Note: provisioner pods are cluster-scoped and create helper pods on the node filesystem.
5. **Confirm the agentgateway commit** `cd65181fd52e` against release 0.12.0 for CVE-2026-29791.
6. Align kube-proxy to the 1.36 minor and prune stale image tags.
7. Ask the vendors for SBOMs / scan results for the private enterprise images (see §4) — they cannot be audited from OSV without one.

---

## 4. Not checked (could not be mapped to an OSV package)

All private `us-docker.pkg.dev/solo-public/**` builds plus the images below were **not** checked — no public OSV package exists, or no version could be resolved. These are the largest remaining blind spot:

| Image | Reason not checked |
|---|---|
| `us-docker.pkg.dev/solo-public/substrate-enterprise/{ateapi,atelet,atecontroller,ate-idp-server,kubernetes-secrets,podcertcontroller,objectstore-plugin,ateworker-gvisor}:v0.6.0-b15382d4` | Private build; probe of OSS module `github.com/kagent-dev/substrate@0.6.0` returned no OSV data |
| `us-docker.pkg.dev/solo-public/kagent-enterprise/{kagent-enterprise-controller:1.0.0-alpha12, kmcp-enterprise-controller}` | Private, no public module |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/{agentgateway-enterprise, enterprise-agentgateway-controller, ext-auth-service, waf-server, rate-limiter, redis}:2026.10.2` | Private, calendar-tagged, no public module (`redis:8.6.7-alpine` sidecar also unmapped) |
| `us-docker.pkg.dev/solo-public/solo-enterprise/{solo-enterprise-ui-frontend:0.5.9 / 1.0.0-alpha12, solo-enterprise-ui-backend:0.5.9, solo-enterprise-autoauth:v0.2.2 / v0.2.3}` | Private UI/auth stack, no public module |
| `ghcr.io/kagent-dev/kagent/controller:0.0.0-alpha.g96d16dfb5f39` (+ local `127.0.0.1:5000` build) | Pseudo-version; `github.com/kagent-dev/kagent@0.0.0-alpha.g…` returned no OSV data |
| `ghcr.io/kagent-dev/kagent/tools:0.3.0` (and cached `0.2.1`) | Umbrella toolbox image, not a single package |
| `docker.io/otel/opentelemetry-collector-contrib:0.161.0` (cached `0.153.0`) — telemetry-collector + k8sobjects-collector | Probed `go.opentelemetry.io/collector` and `github.com/open-telemetry/opentelemetry-collector-contrib` at 0.161.0: no OSV data; per-receiver module scan needed |
| `docker.io/library/redis` — `public.ecr.aws/docker/library/redis:8.2.3-alpine` (argocd-redis), `redis:8.6.7-alpine` (ext-cache) | Alpine-based Docker Hub build; Debian-ecosystem probe was version-guesswork and returned unusable matches — needs an image scanner |
| `clickhouse/clickhouse-server:26.3.35-alpine`, `:26.3.17-alpine` | Vendor C++ build, no OSV Go/Debian mapping |
| `pgvector/pgvector:pg16`, `docker.io/library/postgres:18.6-alpine3.23` (cached) | `postgresql-16` Debian query returned no data; alpine variant unmapped |
| `nginx:1.27-alpine` (`kagent-ui-tls`) | Go `github.com/nginx/nginx` probe at 1.27.4 returned no data; needs distro/OS-level scan |
| `rustfs/rustfs:1.0.0-beta.3` | Rust project, no OSV package found |
| `registry:2` (ns `registry`) | Distribution image not mapped |
| `docker.io/library/python:3.11-slim`, `python:3.13-slim` (`agent-desk`, `typesafe-jev-mcp`) | OS/Python base images; app deps unknown without an SBOM |
| `docker.io/amazon/aws-cli:2.17.56` (cached), `docker.io/nicolaka/netshoot:latest` (cached) | Not mapped; `netshoot` is `:latest` and therefore unidentifiable anyway |
| `127.0.0.1:5000/fortigate-mcp:dev` and other local-registry dev images | Locally built, no upstream package identity |
| Talos v1.13.10 image, kernel `6.18.48-talos`, `ghcr.io/siderolabs/kubelet:v1.36.4` / `v1.35.8` | Talos/Sidero and Linux kernel are not in OSV.dev's Go/Debian datasets used here; needs Talos's own advisories + kernel CVE feed |

---

## 5. Method / caveats

- Queries were made with BusyBox `wget` against `https://api.osv.dev/v1/query` only; 25 of 25 budgeted queries consumed. All responses in this session returned HTTP success (`rc=0`); no query failed and OSV was reachable throughout.
- An empty result `{}` / `"vulns":[]` is reported as *clean* only where I confirmed OSV actually holds data for that module (cert-manager). Otherwise a no-data response is reported as *not checked*.
- OSV reports on the **published upstream module at the image tag version**. It cannot see vendored/forked dependencies inside private images, nor OS/distro packages in distroless/alpine images — a real image scanner (Trivy/Grype against SBOMs) is the complement to this report, especially for §4.
- Severity is taken verbatim from the OSV `severity` field where present; where OSV gave only a CVSS vector or nothing, it is shown as reported and not upgraded. GHSA duplicates of GO- entries were collapsed onto one row.
- Every CVE ID in this document was copied from an OSV `id`/`aliases` field received in this session. Nothing was filled in from memory.
