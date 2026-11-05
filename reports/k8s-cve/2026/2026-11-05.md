# k8s-cve - 2026-11-05

**Verdict: ACTION NEEDED** — Argo CD v3.5.3 carries a CRITICAL AppProject-restriction bypass (CVE-2026-77459, fixed in 3.5.4), and CoreDNS 1.12.3, flannel v0.27.2, the Argo Redis 8.2.3 and the nginx:1.27-alpine TLS sidecar are all multiple releases behind their fixed versions.

- Cluster: **omni-viper** / node `talos-cyv-ekb`
- Kubernetes server: **v1.36.4** (Talos v1.13.10, containerd 2.2.7, kernel 6.18.48-talos, amd64)
- Namespaces inventoried: `kagent`, `ate-system`, `agentgateway-system`, `argocd`, `external-secrets`, `cert-manager`, `kube-system`
- Data source: OSV.dev (`https://api.osv.dev/v1/query`) — reachable, 23 queries used
- Every row below is taken verbatim from an OSV response obtained in this session. No CVE was inferred.

## CVE table

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Kubernetes (`k8s.io/kubernetes`, node v1.36.4) | 1.36.4 | GO-2025-3521 / CVE-2025-1767 — GitRepo volume inadvertent local repository access | not given by OSV | not given |
| Kubernetes (`k8s.io/kubernetes`, node v1.36.4) | 1.36.4 | GO-2025-3547 / CVE-2024-7598 — kube-apiserver race condition | not given by OSV | not given |
| kube-proxy (`registry.k8s.io/kube-proxy`) | v1.34.11 | GO-2025-3521 / CVE-2025-1767 | not given by OSV | not given |
| kube-proxy (`registry.k8s.io/kube-proxy`) | v1.34.11 | GO-2025-3547 / CVE-2024-7598 | not given by OSV | not given |
| **Argo CD** (`quay.io/argoproj/argocd`) | **v3.5.3** | **GHSA-fmxq-cgp8-87wp / CVE-2026-77459 / GO-2026-6784 — AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete hooks** | **CRITICAL** | **3.5.4** (also 3.4.10 / 3.3.15) |
| Argo CD (`quay.io/argoproj/argocd`) | v3.5.3 | GHSA-j6cw-g6p4-7hch / CVE-2026-55797 / GO-2026-6701 — repo-server command injection via crafted SSH repo SOCKS5 proxy URL | HIGH | 3.5.4 (also 3.4.10 / 3.3.15) |
| CoreDNS (`registry.k8s.io/coredns/coredns`) | v1.12.3 | GHSA-9gm5-9rfh-m6vx / CVE-2026-86003 — DoH/DoQ/gRPC bypass UPDATE rejection | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | GHSA-mrg3-qvqr-jw29 / CVE-2026-82399 — unauthenticated memory exhaustion in custom transports | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | GHSA-74w3-63xv-x9mv / CVE-2026-62994 — `k8s_external` headless AXFR panics `transfer` plugin | LOW | 1.14.5 |
| CoreDNS | v1.12.3 | GHSA-2wpx-qpw2-g5h5 / CVE-2026-32934 — DoQ worker pool does not bound stream backlog | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-63cw-r7xf-jmwr / CVE-2026-32936 — DoH GET oversized `dns=` param CPU/memory amplification | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-h8mm-c463-wjq3 / CVE-2026-33489 — transfer stanza lexicographic compare (subzone ACL bypass) | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-qhmp-q7xh-99rh / CVE-2026-33190 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-vp29-5652-4fw9 / CVE-2026-35579 — TSIG auth bypass on gRPC and QUIC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-c9v3-4pv7-87pr / CVE-2026-26017 — ACL bypass | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | GHSA-h75p-j8xm-m278 / CVE-2026-26018 — loop-detection DoS | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | GHSA-527x-5wrf-22m2 / CVE-2025-68151 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | GHSA-93mf-426m-g6x9 / CVE-2025-58063 — DNS cache pinning via etcd lease ID confusion | HIGH | 1.12.4 |
| flannel (`ghcr.io/siderolabs/flannel`) | v0.27.2 | GHSA-vchx-5pr6-ffx2 / CVE-2026-32241 / GO-2026-4894 — cross-node RCE via extension backend BackendData injection | HIGH | 0.28.2 |
| Redis (`public.ecr.aws/docker/library/redis`, argocd-redis) | 8.2.3-alpine | BIT-redis-2026-23479 / CVE-2026-23479 — use-after-free in unblock client flow, RCE | High | 8.2.6 |
| Redis (argocd-redis) | 8.2.3-alpine | BIT-redis-2026-23631 / CVE-2026-23631 — Lua use-after-free, RCE | High | 8.2.6 |
| Redis (argocd-redis) | 8.2.3-alpine | BIT-redis-2026-25243 / CVE-2026-25243 — RESTORE invalid memory access, RCE | High | 8.2.6 |
| Redis (argocd-redis) | 8.2.3-alpine | BIT-redis-2026-25588 / CVE-2026-25588 — RedisTimeSeries RESTORE invalid memory access, RCE | High | 8.2.6 |
| Redis (argocd-redis) | 8.2.3-alpine | BIT-redis-2026-25589 / CVE-2026-25589 — RedisBloom RESTORE invalid memory access, RCE | High | 8.2.6 |
| nginx (`nginx:1.27-alpine`, kagent-ui-tls sidecar) — queried as Bitnami nginx `1.27` | 1.27-alpine | BIT-nginx-2026-15176 / CVE-2026-15176 — `ngx_http_core_module` | Critical | 1.30.6 / 1.31.7 |
| nginx (kagent-ui-tls) | 1.27-alpine | BIT-nginx-2026-42533 / CVE-2026-42533 — map directive & regex matching | Critical | 1.30.4 / 1.31.3 |
| nginx (kagent-ui-tls) | 1.27-alpine | BIT-nginx-2026-42945 / CVE-2026-42945 — `ngx_http_rewrite_module` | Critical | 1.30.1 |
| nginx (kagent-ui-tls) | 1.27-alpine | 18 further OSV advisories (CVE-2024-7347, CVE-2025-23419, CVE-2025-53859, CVE-2026-1642, CVE-2026-27651, CVE-2026-27654, CVE-2026-27784, CVE-2026-28753, CVE-2026-32647, CVE-2026-40460, CVE-2026-40701, CVE-2026-42934, CVE-2026-42946, CVE-2026-48142, CVE-2026-56434, CVE-2026-60005, CVE-2026-95101, CVE-2026-97629) | High / Medium | 1.28.x – 1.31.7 depending on advisory |
| dex (`ghcr.io/dexidp/dex`, argocd-dex-server) | v2.45.1 | GO-2024-2476 / CVE-2024-23656 — TLSConfig discarded, serves TLS 1.0/1.1 and weak ciphers | not given by OSV | 2.38.0 — **verify**: advisory's ecosystem range is `introduced 2.37.0 / fixed 2.38.0`, so 2.45.1 is likely **not** affected (matched only via the unbounded base range) |
| external-secrets (`ghcr.io/external-secrets/external-secrets`) | v2.9.0 | GO-2026-5723 / CVE-2026-42875 / GHSA-wv26-88m5-6h59 — namespace-isolation bypass in CAProvider ConfigMap resolution | not given by OSV | 2.4.0 — **verify**: deployed 2.9.0 is above the fixed version, so likely **not** applicable |
| cert-manager (controller / cainjector / webhook) | v1.21.2 | none returned by OSV | — | — |
| kagent tools (`ghcr.io/kagent-dev/kagent/tools`) | 0.3.0 | none returned by OSV (modules `github.com/kagent-dev/kagent/tools` and `github.com/kagent-dev/kagent`) | — | — |
| otel collector-contrib (`docker.io/otel/opentelemetry-collector-contrib`) | 0.161.0 | none returned by OSV (tried `github.com/open-telemetry/opentelemetry-collector-contrib`, `go.opentelemetry.io/collector`, `go.opentelemetry.io/collector/contrib`) | — | — |
| Redis (`.../enterprise-agentgateway/redis`) | 8.6.7-alpine | none returned by OSV | — | — |

## Operating notes

- **Version skew:** the node reports Kubernetes **v1.36.4**, but `kube-proxy` is still **v1.34.11**. Beyond the two stale OSV matches, this two-minor-version skew is outside supported skew and is itself worth remediating.
- The two Kubernetes advisories (CVE-2025-1767, CVE-2024-7598) are returned for *both* 1.36.4 and 1.34.11 with unbounded `introduced` ranges and **no `fixed` event** in the Go ecosystem data. They are old, low-actionability Go-DB entries rather than indicators that 1.36.4 is unpatched; treat as informational and confirm against the upstream Kubernetes fixed releases before acting.
- Highest-value quick wins: Argo CD → **v3.5.4**, CoreDNS → **1.14.7**, flannel → **0.28.2**, argocd Redis → **8.2.6**, kagent-ui-tls nginx → a currently supported nginx branch (the 1.27 line is EOL).
- `nginx:1.27-alpine` is a floating tag; the exact patch level cannot be resolved from the tag, so the Bitnami nginx results are indicative for the 1.27 line rather than a precise patch-level match. Resolve the image digest to confirm.
- Several pods in `argocd`, `agentgateway-system`, `cert-manager`, `external-secrets` and `kagent` are in `Completed`/`Error` state (stale replicas from prior rollouts), which is a hygiene issue rather than a CVE.

## Not checked (could not be mapped to an OSV package)

Private / proprietary Solo & Substrate images — no public upstream package exists to query:
- `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12`
- `us-docker.pkg.dev/solo-public/solo-enterprise/solo-enterprise-ui-frontend:1.0.0-alpha12` and `:0.5.9`
- `us-docker.pkg.dev/solo-public/solo-enterprise/solo-enterprise-ui-backend:0.5.9`
- `us-docker.pkg.dev/solo-public/solo-enterprise/solo-enterprise-autoauth:v0.2.2` and `:v0.2.3`
- `us-docker.pkg.dev/solo-public/substrate-enterprise/ateapi:v0.6.0-b15382d4`
- `us-docker.pkg.dev/solo-public/substrate-enterprise/atecontroller:v0.6.0-b15382d4`
- `us-docker.pkg.dev/solo-public/substrate-enterprise/ate-idp-server:v0.6.0-b15382d4`
- `us-docker.pkg.dev/solo-public/substrate-enterprise/atelet:v0.6.0-b15382d4`
- `us-docker.pkg.dev/solo-public/substrate-enterprise/ateworker-gvisor:v0.6.0-b15382d4`
- `us-docker.pkg.dev/solo-public/substrate-enterprise/kubernetes-secrets:v0.6.0-b15382d4`
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/agentgateway-enterprise:2026.10.2`
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/enterprise-agentgateway-controller:2026.10.2`
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/ext-auth-service:2026.10.2`
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/rate-limiter:2026.10.2`
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/waf-server:2026.10.2`

Other unmapable / out-of-scope images:
- `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` — commit-pinned tag, no semantic version to resolve (atenet-egress, atenet-router). Also note these run **without** the hardened worker sandbox.
- `clickhouse/clickhouse-server:26.3.17-alpine` (kagent) and `:26.3.35-alpine` (agentgateway-system) — not a Go module; not queried (query budget).
- `rustfs/rustfs:1.0.0-beta.3@sha256:378642b0…` — no known OSV package name.
- `python:3.13-slim` (typesafe-jev-mcp) — base image plus `pip install` of `mcp[cli]`/`httpx` at container start, so neither the base patch level nor the resolved Python deps can be pinned from the tag.
- `127.0.0.1:5000/fortigate-mcp:dev` — locally built, mutable `dev` tag, no upstream identity.
- Talos v1.13.10, containerd 2.2.7, node kernel 6.18.48-talos — node-layer components not queryable via Go-module OSV lookups.

## Notes on the run

- Read-only throughout: `get`/`list` calls only against the cluster; no cluster mutation, no Secret data read or printed.
- OSV.dev was reachable for all 23 attempts; two calls returned HTTP 400 (Docker-ecosystem queries for `nginx:1.27-alpine` and `redis:8.2.3-alpine`, which OSV does not index in that form) and were re-run against the Bitnami ecosystem where applicable.
- Query budget: 23 of the 25 allowed OSV queries used; the Kubernetes version and all requested platform images (kagent, agentgateway, substrate/ate, argocd, cert-manager, external-secrets) were prioritised, with everything else listed above as not checked.
