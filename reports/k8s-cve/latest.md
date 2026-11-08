# k8s-cve - 2026-11-08

**Verdict: ACTION NEEDED** — 1 CRITICAL Argo CD privilege-boundary bypass (CVE-2026-77459), 1 CRITICAL local-path-provisioner path traversal (CVE-2025-62878), 1 HIGH Flannel cross-node RCE (CVE-2026-32241) and 11 HIGH-class advisories against CoreDNS 1.12.3 are all unpatched in the cluster today.

Cluster: `omni-viper` · checked 2026-11-08 (America/Toronto) · read-only inventory · OSV.dev (api.osv.dev) as sole source · 24 OSV queries used (budget 25).

---

## 1. Inventory

| Item | Value |
|---|---|
| Kubernetes server / node version | **v1.36.4** (single control-plane node `talos-cyv-ekb`) |
| Node OS | Talos **v1.13.10**, kernel 6.18.48-talos (amd64) |
| Container runtime | containerd **2.2.7** |
| Control-plane images | kube-apiserver / kube-controller-manager / kube-scheduler `registry.k8s.io/...:v1.36.4` |
| Node-level images | kube-proxy `registry.k8s.io/kube-proxy:v1.34.11` (⚠ **2-minor version skew** behind the 1.36.4 control plane), flannel `ghcr.io/siderolabs/flannel:v0.27.2`, coredns `registry.k8s.io/coredns/coredns:v1.12.3` |

Namespaces surveyed: `kagent`, `ate-system`, `agentgateway-system`, `argocd`, `external-secrets`, `cert-manager`, `kube-system` (+ cluster-wide DaemonSets/StatefulSets for completeness).

## 2. CVE table (every row is from an OSV.dev response in this session)

| Component | Version | CVE / advisory | Severity | Fixed version |
|---|---|---|---|---|
| **Argo CD** (`quay.io/argoproj/argocd:v3.5.3` — server, repo-server, application-controller, applicationset-controller, notifications-controller) | v3.5.3 | **CVE-2026-77459** · GHSA-fmxq-cgp8-87wp / GO-2026-6784 — AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete hooks | **CRITICAL** | **3.5.4** (also 3.4.10 / 3.3.15 / 3.6.0-rc2) |
| **Argo CD** (same image) | v3.5.3 | **CVE-2026-55797** (+ CVE-2026-113708, CVE-2026-113709 aliases) · GHSA-j6cw-g6p4-7hch / GO-2026-6701 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | **HIGH** | **3.5.4** |
| **Flannel** (`ghcr.io/siderolabs/flannel:v0.27.2`) | v0.27.2 | **CVE-2026-32241** · GHSA-vchx-5pr6-ffx2 / GO-2026-4894 — cross-node RCE via extension backend BackendData injection | **HIGH** | **0.28.2** |
| **local-path-provisioner** (`rancher/local-path-provisioner:v0.0.31`, ns `local-path-storage`) | v0.0.31 | **CVE-2025-62878** · GHSA-jr3w-9vfr-c746 / GO-2026-4425 — path traversal via `parameters.pathPattern` | **CRITICAL** | **0.0.34** |
| **local-path-provisioner** (same) | v0.0.31 | **CVE-2026-44543** · GHSA-7fxv-8wr2-mfc4 / GO-2026-5218 — HelperPod template injection | **HIGH** | **0.0.36** |
| **CoreDNS** (`registry.k8s.io/coredns/coredns:v1.12.3`) | v1.12.3 | CVE-2025-58063 · GHSA-93mf-426m-g6x9 / GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | **HIGH** | 1.12.4 |
| CoreDNS | v1.12.3 | CVE-2026-26017 · GHSA-c9v3-4pv7-87pr / GO-2026-4630 — ACL bypass | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 · GHSA-h75p-j8xm-m278 / GO-2026-4635 — loop-detection DoS | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-32934 · GHSA-2wpx-qpw2-g5h5 / GO-2026-4969 — DoQ worker pool unbounded stream backlog | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 · GHSA-63cw-r7xf-jmwr / GO-2026-5164 — DoH GET oversized `dns=` pre-validation CPU/memory amplification | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 · GHSA-h8mm-c463-wjq3 / GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 · GHSA-qhmp-q7xh-99rh / GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 · GHSA-vp29-5652-4fw9 / GO-2026-5667 — TSIG auth bypass on gRPC and QUIC | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-82399 · GHSA-mrg3-qvqr-jw29 / GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-86003 · GHSA-9gm5-9rfh-m6vx / GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2025-68151 · GHSA-527x-5wrf-22m2 / GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-62994 · GHSA-74w3-63xv-x9mv / GO-2026-6805 — `k8s_external` headless AXFR panics `transfer` plugin | LOW | 1.14.5 |
| | | → **single upgrade target covering all CoreDNS rows: `registry.k8s.io/coredns/coredns:1.14.7`** | | |
| **containerd** (node runtime, Talos 1.13.10) | 2.2.7 | CVE-2026-53495 · GHSA-7jxh-36q5-gcqv / GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | MODERATE | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-95838 · GHSA-rp3h-jf77-q9p4 / GO-2026-6796 — unpack DoS via repeated opaque whiteouts | MODERATE | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-53493 · GHSA-pg57-6jwg-q645 / GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | MODERATE | 2.2.9 |
| **Kubernetes** (k8s.io/kubernetes, Go ecosystem) | 1.36.4 | CVE-2025-1767 · GO-2025-3521 (GHSA-3wgm-2gw2-vh5m) — GitRepo volume inadvertent local repository access | n/a (none listed in OSV) | none listed in OSV |
| **Kubernetes** | 1.36.4 | CVE-2024-7598 · GO-2025-3547 (GHSA-r56h-j38w-hrqq) — kube-apiserver race condition | n/a (none listed in OSV) | none listed in OSV |
| **kube-proxy** (`registry.k8s.io/kube-proxy:v1.34.11`) | v1.34.11 | same two Go advisories as above (CVE-2025-1767, CVE-2024-7598) | n/a | none listed in OSV |
| **Talos Linux** (node OS) | v1.13.10 | CVE-2026-77564 · GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | LOW | 1.14.0 (alpha tag in OSV) |
| **nginx** (`nginx:1.27-alpine`, ns `kagent`, deployment `kagent-ui-tls`) | 1.27-alpine (patch level unpinned; queried `1.27.4-r0` @ Alpine v3.22) | ALPINE-CVE-2025-53859, ALPINE-CVE-2026-1642 (fixed 1.28.2-r0); ALPINE-CVE-2026-27651 / -27654 / -27784 / -28753 / -28755 / -32647 (fixed 1.28.3-r0); ALPINE-CVE-2026-40460 / -42934 / -42945 / -42946 / -9256 / -40701 / -42055 / -42533 / -48142 / -49975 / -56434 / -60005 | n/a (Alpine DB entries carry no severity) | all fixes ≥ 1.28.x → move to `nginx:1.28.3` or newer (e.g. 1.30.x) |
| **External Secrets Operator** (`ghcr.io/external-secrets/external-secrets:v2.9.0`) | v2.9.0 | ⚠ CVE-2026-42875 · GHSA-wv26-88m5-6h59 / GO-2026-5723 — namespace isolation bypass in CAProvider ConfigMap resolution for SecretStore. **Likely FALSE POSITIVE**: OSV's only fixed event is `2.4.0` (custom range "before v2.4.0") and the entry is `review_status: UNREVIEWED`; v2.9.0 is above it. The Go module's canonical range has no fixed event, so OSV returns it. | n/a | 2.4.0 per OSV (already satisfied by v2.9.0) |
| **Dex** (`ghcr.io/dexidp/dex:v2.45.1`, Argo CD SSO) | v2.45.1 | ⚠ CVE-2024-23656 · GO-2024-2476 — discards TLSConfig, serves TLS 1.0/1.1 and insecure ciphers. **Likely FALSE POSITIVE**: fixed at 2.38.0, which v2.45.1 already exceeds (Go major-version `/v2` module-path mismatch). | n/a | 2.38.0 (already satisfied) |
| **cert-manager** (controller / cainjector / webhook) | v1.21.2 | no OSV records returned for `github.com/cert-manager/cert-manager` | clean | — |
| **Tailscale** (`ghcr.io/tailscale/tailscale:v1.102.4`) | v1.102.4 | no OSV records returned for `tailscale.com` | clean | — |

## 3. Not checked (could not map to an OSV package/version)

Images left unchecked — either closed-source/vendor builds whose version tag is not a Go module version, unversioned tags, or distro queries that returned no records for the assumed source package:

- **Solo.io enterprise agentgateway stack** (`us-docker.pkg.dev/solo-public/enterprise-agentgateway/…:2026.10.2`): `agentgateway-enterprise` (agentgateway-proxy, bedrock-proxy), `enterprise-agentgateway-controller`, `ext-auth-service`, `rate-limiter`, `waf-server`. A probe of `github.com/agentgateway/agentgateway` returned no data and the `2026.10.2` tag is not a module version → not checked.
- **Substrate / ate stack** (`us-docker.pkg.dev/solo-public/substrate-enterprise/…:v0.6.0-b15382d4`): `ateapi`, `atecontroller`, `ate-idp-server`, `atelet`, `ateworker-gvisor`, `kubernetes-secrets` (k8s-credential-provider), `podcertcontroller` — commit-stamped private builds, no upstream module version.
- **Substrate net** (`ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…`) — pinned to a commit SHA (atenet-router, atenet-egress).
- **Solo.io enterprise UI / auth**: `solo-enterprise-ui-frontend:0.5.9` & `:1.0.0-alpha12`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3`.
- **kagent**: `kagent-enterprise-controller:1.0.0-alpha12` (private); `ghcr.io/kagent-dev/kagent/tools:0.3.0` (Go probe `github.com/kagent-dev/kagent@v0.3.0` returned no data — module path unverified).
- **OpenTelemetry Collector Contrib** (`docker.io/otel/opentelemetry-collector-contrib:0.161.0`, ×3 deployments/statefulsets) — queries for `github.com/open-telemetry/opentelemetry-collector-contrib` and `go.opentelemetry.io/collector` @ v0.161.0 returned no records; the shipped third-party Go dependencies are not enumerable from outside the image.
- **Redis**: `public.ecr.aws/docker/library/redis:8.2.3-alpine` (argocd-redis), `us-docker.pkg.dev/solo-public/enterprise-agentgateway/redis:8.6.7-alpine` (ext-cache) — Alpine package probes returned no records (base-release assumption unverified).
- **ClickHouse**: `clickhouse/clickhouse-server:26.3.17-alpine` (kagent), `:26.3.35-alpine` (agentgateway management) — Debian package probe returned no records.
- **PostgreSQL/pgvector**: `pgvector/pgvector:pg16` (ns `postgres`) — no exact PostgreSQL patch version is discoverable from the image tag; Debian probe returned no records.
- **Registry**: `registry:2` (ns `registry`) — the `2` tag does not resolve to a distribution patch release, so no version could be queried. For manual follow-up: OSV records exist for `github.com/distribution/distribution` (v3.x probes) — CVE-2026-33540 & CVE-2026-35172 (HIGH, fixed 3.1.0), CVE-2026-41888 (MODERATE, fixed 3.1.1), CVE-2026-85747 (MODERATE, fixed 3.1.2). Verify the real version before acting.
- **Python bases**: `python:3.13-slim` (typesafe-jev-mcp), `python:3.11-slim` (agent-desk) — Debian/OS package version not discoverable from the tag; skipped to stay inside the OSV query budget.
- **Locally built**: `127.0.0.1:5000/fortigate-mcp:dev` (ns `kagent`) — `dev` tag, no version to match.

## 4. Recommended actions (priority order)

1. **Argo CD → v3.5.4** (CRITICAL CVE-2026-77459 + HIGH repo-server command injection CVE-2026-55797). Argo CD has cluster-wide write access, so the AppProject-restriction bypass and the repo-server RCE chain are the highest-impact items in this report.
2. **Flannel → v0.28.2** (CVE-2026-32241, cross-node RCE; runs host-networked with NET_ADMIN/NET_RAW).
3. **local-path-provisioner → v0.0.36** (CRITICAL path traversal + HIGH helper-pod template injection; it creates privileged helper pods on the node).
4. **CoreDNS → 1.14.7** (12 advisories, 11 HIGH; cluster-wide DNS is a prerequisite for every workload).
5. **containerd / Talos**: containerd 2.2.7 → ≥2.2.9 and Talos 1.13.10 → 1.14.x will also close CVE-2026-77564 (support-bundle credential leak — relevant because this node hosts a private registry).
6. **kube-proxy image skew**: `registry.k8s.io/kube-proxy:v1.34.11` against a 1.36.4 control plane — align it to v1.36.4.
7. Re-scan the vendor images (agentgateway / substrate / solo-enterprise / otel-collector) with an image-scanner that can read SBOMs inside the images — they are ~60% of the running workload and are currently invisible to tag-based OSV lookups.

---
*Method: `k8s_get_resources` (pods/deployments/daemonsets/statefulsets, wide+yaml) for inventory; OSV.dev `/v1/query` for all advisory data. Kubernetes queried as Go ecosystem, package `k8s.io/kubernetes`, version without leading `v`. Only `id`, `aliases`, `summary`, `severity` and fixed-version events were retained; no CVE was inferred or carried over from any other source.*
