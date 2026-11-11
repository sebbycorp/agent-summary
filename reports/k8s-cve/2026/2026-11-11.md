# k8s-cve - 2026-11-11

**Verdict: ACTION NEEDED — 2 CRITICAL and several HIGH findings with fixes available (Argo CD 3.5.3, Flannel 0.27.2, local-path-provisioner 0.0.31).**

- Cluster: `omni-viper` — Talos (v1.13.10), single node `talos-cyv-ekb`, containerd 2.2.7, kernel 6.18.48-talos
- Kubernetes server / control plane: **v1.36.4** (kube-apiserver, controller-manager, scheduler all v1.36.4; kubectl job v1.36.2)
- Checked: 2026-11-11 (America/Toronto), source = OSV.dev API only (21 queries)
- Every CVE below is copied from an OSV.dev response in this run. No CVE was inferred or recalled.

## Findings by component

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Argo CD (`quay.io/argoproj/argocd`) — server, repo-server, application-controller, notifications | v3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp / GO-2026-6784 — AppProject destination-namespace + resource restrictions bypassed by PreDelete/PostDelete hooks | **CRITICAL** | **3.5.4** (3.3.15 / 3.4.10 / 3.6.0-rc2) |
| Argo CD (`quay.io/argoproj/argocd`) | v3.5.3 | CVE-2026-55797 / GHSA-j6cw-g6p4-7hch / GO-2026-6701 — repo-server command injection via crafted SSH repo SOCKS5 proxy URL | HIGH | **3.5.4** |
| Flannel CNI (`ghcr.io/siderolabs/flannel`) | v0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2 / GO-2026-4894 — cross-node RCE via extension backend BackendData injection | HIGH | **0.28.2** |
| local-path-provisioner (`rancher/local-path-provisioner`) | v0.0.31 | CVE-2025-62878 / GHSA-jr3w-9vfr-c746 / GO-2026-4425 — path traversal via `parameters.pathPattern` | **CRITICAL** | **0.0.34** |
| local-path-provisioner | v0.0.31 | CVE-2026-44543 / GHSA-7fxv-8wr2-mfc4 / GO-2026-5218 — HelperPod template injection | HIGH | **0.0.36** |
| CoreDNS (`registry.k8s.io/coredns/coredns`) | v1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9 / GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | HIGH | **1.12.4** |
| CoreDNS | v1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr / GO-2026-4630 — ACL bypass | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GHSA-h75p-j8xm-m278 / GO-2026-4635 — loop-detection DoS | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GHSA-527x-5wrf-22m2 / GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-32934 / GHSA-2wpx-qpw2-g5h5 / GO-2026-4969 — DoQ worker pool does not bound stream backlog | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GHSA-63cw-r7xf-jmwr / GO-2026-5164 — DoH GET oversized `dns=` amplification | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GHSA-h8mm-c463-wjq3 / GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh / GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GHSA-vp29-5652-4fw9 / GO-2026-5667 — TSIG auth bypass on gRPC/QUIC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx / GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29 / GO-2026-6507 — unauthenticated memory exhaustion in custom transports | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv / GO-2026-6805 — `k8s_external` headless AXFR panics `transfer` plugin | LOW | 1.14.5 |
| Kubernetes (`k8s.io/kubernetes`) — apiserver / controller-manager / scheduler / kubectl | v1.36.4 | CVE-2025-1767 / GO-2025-3521 (GHSA-3wgm-2gw2-vh5m) — GitRepo volume inadvertent local repository access | not given by OSV | none listed in OSV (introduced 0) |
| Kubernetes | v1.36.4 | CVE-2024-7598 / GO-2025-3547 (GHSA-r56h-j38w-hrqq) — kube-apiserver race condition | not given by OSV | none listed in OSV (introduced 1.3.0) |
| kube-proxy (`registry.k8s.io/kube-proxy`) | **v1.34.11** | Same two Kubernetes advisories as above (GO-2025-3521, GO-2025-3547) | not given by OSV | none listed in OSV |
| containerd (node runtime, Talos) | 2.2.7 | CVE-2026-53495 / GHSA-7jxh-36q5-gcqv / GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | MODERATE | **2.2.8** |
| containerd | 2.2.7 | CVE-2026-95838 / GHSA-rp3h-jf77-q9p4 / GO-2026-6796 — unpack DoS via repeated opaque whiteouts | MODERATE | **2.2.8** |
| containerd | 2.2.7 | CVE-2026-53493 / GHSA-pg57-6jwg-q645 / GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | MODERATE | **2.2.9** |

Note on kube-proxy: it reports **v1.34.11** while the control plane is **v1.36.4** — a 2-minor skew that OSV flags against the older version. Worth reconciling (Talos component release channel).

## Clean (queried OSV, no matching vulnerabilities)

| Component | Version | OSV module queried |
|---|---|---|
| Talos (node OS) | v1.13.10 | `github.com/siderolabs/talos` (Go) — no vulns |
| cert-manager controller / cainjector / webhook | v1.21.2 | `github.com/cert-manager/cert-manager` (Go) — no vulns |
| external-secrets operator / cert-controller / webhook | v2.9.0 | `github.com/external-secrets/external-secrets` (Go) — see false positive below |
| Tailscale (`ghcr.io/tailscale/tailscale`) | v1.102.4 | `github.com/tailscale/tailscale` (Go) — no vulns |
| OpenTelemetry Collector contrib (telemetry + k8sobjects collectors) | 0.161.0 | `github.com/open-telemetry/opentelemetry-collector-contrib` and `go.opentelemetry.io/collector` (Go) — no vulns |
| kagent tools (`ghcr.io/kagent-dev/kagent/tools`) | 0.3.0 | `github.com/kagent-dev/kagent` (Go) — no vulns (module path assumed from image repo) |
| Argo CD dex server (`ghcr.io/dexidp/dex`) | v2.45.1 | see false positive below |

## Returned by OSV but NOT affecting the deployed version (scanner false positives — no action)

| Component | Version | Advisory | Why not affected |
|---|---|---|---|
| external-secrets | v2.9.0 | CVE-2026-42875 / GHSA-wv26-88m5-6h59 / GO-2026-5723 — namespace isolation bypass in CAProvider ConfigMap resolution (MODERATE) | OSV affected range is "before v2.4.0"; deployed 2.9.0 is already past the fix |
| dex (argocd-dex-server) | v2.45.1 | CVE-2024-23656 / GHSA-gr79-9v6v-gc9r / GO-2024-2476 — TLS config discarded, serves TLS 1.0/1.1 (HIGH) | OSV affected range is 2.37.0 → fixed in 2.38.0; deployed 2.45.1 is past the fix |

## Not checked (could not map an image to an OSV package)

Base-layer / distroless / binary images need a container image scanner (Trivy/Grype), not an OSV package-version query:

- **agentgateway enterprise** — `agentgateway-enterprise:2026.10.2`, `enterprise-agentgateway-controller:2026.10.2`, `ext-auth-service:2026.10.2`, `rate-limiter:2026.10.2`, `waf-server:2026.10.2` (proprietary Solo/Solo.io builds, no public module)
- **substrate / ate** — `ateapi:v0.6.0-b15382d4`, `atecontroller:v0.6.0-b15382d4`, `ate-idp-server:v0.6.0-b15382d4`, `atelet:v0.6.0-b15382d4`, `podcertcontroller:v0.6.0-b15382d4`, `kubernetes-secrets:v0.6.0-b15382d4`, `ateworker-gvisor:v0.6.0-b15382d4` (proprietary; no published Go module path to query)
- **atenet egress/router** — `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` (Rust/agentgateway pinned to a git SHA — no resolvable semver version for OSV)
- **solo-enterprise UI** — `solo-enterprise-ui-frontend:0.5.9`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-ui-frontend:1.0.0-alpha12`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3` (proprietary web UI / backend)
- **kagent-controller** — `kagent-enterprise-controller:1.0.0-alpha12` (proprietary)
- **fortigate-mcp** — `127.0.0.1:5000/fortigate-mcp:dev` (private cluster-local build, mutable `:dev` tag, no upstream)
- **Databases / caches** — `clickhouse/clickhouse-server:26.3.35-alpine`, `clickhouse/clickhouse-server:26.3.17-alpine`, `pgvector/pgvector:pg16`, `redis:8.6.7-alpine`, `redis:8.2.3-alpine` (C/C++ projects; no OSV Go/npm package — needs distro-layer scanning)
- **rustfs** — `rustfs/rustfs:1.0.0-beta.3@sha256:378642b0…` (Rust binary, no OSV-mappable crate version)
- **Side/base images** — `nginx:1.27-alpine`, `python:3.13-slim`, `python:3.11-slim`, `amazon/aws-cli:2.17.0`, `registry:2` (distro base images — needs distro-layer scanning)

## Recommended actions (priority order)

1. **Argo CD → v3.5.4** (CRITICAL CVE-2026-77459 AppProject restriction bypass + HIGH CVE-2026-55797 repo-server command injection). Single patch bump, fixes both.
2. **Flannel → v0.28.2** (HIGH CVE-2026-32241 cross-node RCE) — this is the CNI on the only node; schedule a drain/restart window.
3. **local-path-provisioner → ≥ v0.0.36** (CRITICAL CVE-2025-62878 path traversal + HIGH CVE-2026-44543 helper-pod injection).
4. **Talos → containerd 2.2.9** via a Talos patch/channel bump (three MODERATE DoS CVEs; Talos 1.13.10 itself has no OSV entries).
5. **CoreDNS → v1.14.7**: 12 advisories still affect 1.12.3; only CVE-2025-58063 is fixed in the 1.12.x line (1.12.4), the rest require the 1.14.x line.
6. Reconcile **kube-proxy 1.34.11 vs control plane 1.36.4** version skew.
7. Add an **image-scanner** job for the ~20 proprietary/base images that OSV package queries cannot cover.
