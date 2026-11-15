# k8s-cve - 2026-11-15

**Verdict: ACTION NEEDED** — Argo CD v3.5.3 has a CRITICAL (CVSS 3.1, scope-changing) project-restriction bypass plus a HIGH repo-server RCE, both fixed in v3.5.4; CoreDNS v1.12.3 and Flannel v0.27.2 carry multiple HIGH advisories with fixes available.

- Cluster: `omni-viper` — single node `talos-cyv-ekb`, Kubernetes **v1.36.4**, Talos **v1.13.10**, containerd **2.2.7**, kernel 6.18.48-talos
- Checked: 2026-11-15 (America/Toronto), read-only; source = OSV.dev (`/v1/query`), 25 queries used
- Every row below comes from an OSV response received in this run. Nothing inferred.

## CVE table

| Component (image) | Version | CVE / advisory | Severity (OSV) | Fixed version |
|---|---|---|---|---|
| Kubernetes control plane / kubelet (apiserver, scheduler, controller-manager, kubelet) — checked as Go `k8s.io/kubernetes` | 1.36.4 | GO-2025-3521 / CVE-2025-1767 (GitRepo volume inadvertent local repository access) | not stated in OSV record | not given |
| Kubernetes control plane — Go `k8s.io/kubernetes` | 1.36.4 | GO-2025-3547 / CVE-2024-7598 (kube-apiserver race condition) | not stated in OSV record | not given |
| kube-proxy (`registry.k8s.io/kube-proxy:v1.34.11`) — Go `k8s.io/kubernetes` | 1.34.11 | GO-2025-3521 / CVE-2025-1767 | not stated in OSV record | not given |
| CNI (`ghcr.io/siderolabs/flannel:v0.27.2`) — Go `github.com/flannel-io/flannel` | 0.27.2 | GHSA-vchx-5pr6-ffx2 / CVE-2026-32241 (cross-node RCE via extension-backend BackendData injection) | **HIGH** (CVSS 3.1) | **0.28.2** |
| Argo CD (`quay.io/argoproj/argocd:v3.5.3`: server, repo-server, application-controller, applicationset, notifications) — Go `github.com/argoproj/argo-cd/v3` | 3.5.3 | GHSA-fmxq-cgp8-87wp + GO-2026-6784 / **CVE-2026-77459** (AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete hooks) | **CRITICAL** (CVSS 3.1: AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H) | **3.5.4** (also 3.3.15 / 3.4.10 / 3.6.0-rc2) |
| Argo CD (same image) | 3.5.3 | GHSA-j6cw-g6p4-7hch + GO-2026-6701 / **CVE-2026-55797** (repo-server command injection via crafted SSH repo SOCKS5 proxy URL) | **HIGH** (CVSS 3.1: AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H) | **3.5.4** (also 3.3.15 / 3.4.10) |
| DNS (`registry.k8s.io/coredns/coredns:v1.12.3`) — Go `github.com/coredns/coredns` | 1.12.3 | GHSA-93mf-426m-g6x9 / GO-2025-3942 / CVE-2025-58063 (DNS cache pinning via etcd lease ID confusion) | HIGH | 1.12.4 |
| CoreDNS | 1.12.3 | GHSA-527x-5wrf-22m2 / GO-2026-4289 / CVE-2025-68151 (gRPC/HTTPS/HTTP3 no resource limits -> DoS) | MODERATE | 1.14.0 |
| CoreDNS | 1.12.3 | GHSA-c9v3-4pv7-87pr / GO-2026-4630 / CVE-2026-26017 (ACL bypass) | HIGH | 1.14.2 |
| CoreDNS | 1.12.3 | GHSA-h75p-j8xm-m278 / GO-2026-4635 / CVE-2026-26018 (loop-detection DoS) | HIGH | 1.14.2 |
| CoreDNS | 1.12.3 | GHSA-2wpx-qpw2-g5h5 / GO-2026-4969 / CVE-2026-32934 (DoQ worker pool unbounded stream backlog) | HIGH | 1.14.3 |
| CoreDNS | 1.12.3 | GHSA-63cw-r7xf-jmwr / GO-2026-5164 / CVE-2026-32936 (DoH GET oversized `dns=` CPU/memory amplification) | HIGH | 1.14.3 |
| CoreDNS | 1.12.3 | GHSA-qhmp-q7xh-99rh / GO-2026-5583 / CVE-2026-33190 (TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC) | HIGH | 1.14.3 |
| CoreDNS | 1.12.3 | GHSA-h8mm-c463-wjq3 / GO-2026-5417 / CVE-2026-33489 (transfer stanza lexicographic compare -> subzone ACL bypass) | HIGH | 1.14.3 |
| CoreDNS | 1.12.3 | GHSA-vp29-5652-4fw9 / GO-2026-5667 / CVE-2026-35579 (TSIG auth bypass on gRPC + QUIC) | HIGH | 1.14.3 |
| CoreDNS | 1.12.3 | GHSA-74w3-63xv-x9mv / GO-2026-6805 / CVE-2026-62994 (`k8s_external` headless AXFR panics `transfer` plugin) | LOW | 1.14.5 |
| CoreDNS | 1.12.3 | GHSA-mrg3-qvqr-jw29 / GO-2026-6507 / CVE-2026-82399 (unauthenticated memory exhaustion in custom transports) | HIGH | 1.14.7 |
| CoreDNS | 1.12.3 | GHSA-9gm5-9rfh-m6vx / GO-2026-6506 / CVE-2026-86003 (DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP) | HIGH | 1.14.7 |
| External Secrets (`ghcr.io/external-secrets/external-secrets:v2.9.0`: operator, webhook, cert-controller) — Go `github.com/external-secrets/external-secrets` | 2.9.0 | GO-2026-5723 / GHSA-wv26-88m5-6h59 / CVE-2026-42875 (namespace isolation bypass in CAProvider ConfigMap resolution for SecretStore) | not stated in OSV record | OSV range `fixed: 2.4.0` is **below** the running 2.9.0, so this entry likely targets a different module in the advisory — treat as needs-triage, not confirmed exposure |
| Dex (`ghcr.io/dexidp/dex:v2.45.1`, argocd-dex-server) — Go `github.com/dexidp/dex` | 2.45.1 | GO-2024-2476 / GHSA-gr79-9v6v-gc9r / CVE-2024-23656 (TLSConfig discarded; serves deprecated TLS 1.0/1.1 + weak ciphers) | not stated in OSV record | OSV range `2.37.0 -> 2.38.0`; running 2.45.1 is outside that range — likely not applicable, confirm manually |
| cert-manager (`quay.io/jetstack/cert-manager-controller\|webhook\|cainjector:v1.21.2`) — Go `github.com/cert-manager/cert-manager` | 1.21.2 | none returned | — | — |
| OpenTelemetry Collector (`docker.io/otel/opentelemetry-collector-contrib:0.161.0`) — Go `go.opentelemetry.io/collector` and `github.com/open-telemetry/opentelemetry-collector-contrib` | 0.161.0 | no advisories for those Go modules at this version | — | — |
| kagent tools (`ghcr.io/kagent-dev/kagent/tools:0.3.0`) — Go `github.com/kagent-dev/kagent` | 0.3.0 | no advisories returned for that module/version | — | — |

## Recommended actions (highest first)

1. **Argo CD v3.5.3 -> v3.5.4** — CVE-2026-77459 (CRITICAL, scope-changing: anyone with Git write access can get the application-controller to create objects the AppProject forbids, including cluster-scoped objects such as `ClusterRoleBinding`, on Application delete) and CVE-2026-55797 (HIGH repo-server command injection). One image serves all five argocd workloads, so a single bump closes both. OSV notes 2.x and 3.0-3.2 are out of support.
2. **Flannel v0.27.2 -> v0.28.2** — CVE-2026-32241 is a HIGH cross-node RCE in the CNI, which runs privileged on the node (single-node cluster, so the blast radius is the whole node). Delivered via the Talos cluster manifest (`ghcr.io/siderolabs/flannel`).
3. **CoreDNS v1.12.3 -> v1.14.7** — 12 CVEs, mostly HIGH (TSIG auth bypass, ACL bypass, several unauthenticated DoS). Cluster DNS is on the request path; the version is pinned by the Talos machine config, so confirm the Talos-supported CoreDNS version before bumping.
4. **kube-proxy image skew** — `kube-proxy:v1.34.11` against an apiserver at v1.36.4 is roughly a 2 minor-version skew and also carries CVE-2025-1767 per OSV. Align it with the control plane version via Talos.
5. **Dex v2.45.1 / External Secrets v2.9.0** — re-check manually; the OSV-returned advisories have fixed-version ranges that do not appear applicable to the running versions (see table). Do not treat as confirmed.

## Not checked / could not map

**Queried or mappable, but no usable OSV coverage:**
- `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` — proprietary Solo/kagent-enterprise build; no published Go module/version in OSV.
- `us-docker.pkg.dev/solo-public/substrate-enterprise/*` at `v0.6.0-b15382d4` — `ateapi`, `atecontroller`, `ate-idp-server`, `atelet`, `kubernetes-secrets`, `podcertcontroller`, `ateworker-gvisor` — internal Substrate builds; commit-suffixed tags not resolvable to a Go module version.
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/*` at `2026.11.0` — `agentgateway-enterprise`, `enterprise-agentgateway-controller`, `ext-auth-service`, `rate-limiter`, `waf-server` — proprietary enterprise builds, no OSV package mapping.
- `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7...` (atenet-egress, atenet-router) — tag is a commit hash, upstream agentgateway release unknown, so no version could be chosen to query.
- `us-docker.pkg.dev/solo-public/solo-enterprise/*` — `solo-enterprise-ui-frontend:1.0.0-alpha12`, `solo-enterprise-ui-frontend:0.5.9`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-autoauth:v0.2.2`, `solo-enterprise-autoauth:v0.2.3`.
- Talos **v1.13.10** and **containerd 2.2.7** — no OSV-mappable package queried within budget; Sidero/Talos advisories sit outside the OSV ecosystems used here. Not covered by this run.
- `docker.io/otel/opentelemetry-collector-contrib:0.161.0` and `ghcr.io/kagent-dev/kagent/tools:0.3.0` — Go modules queried with empty results; their base/OS layers were not scanned.

**Not queried (25-query budget spent on the Kubernetes version + platform components):**
- `public.ecr.aws/docker/library/redis:8.2.3-alpine` (argocd-redis), `us-docker.pkg.dev/.../redis:8.6.7-alpine` (agentgateway ext-cache)
- `nginx:1.27-alpine` (kagent-ui-tls)
- `clickhouse/clickhouse-server:26.3.17-alpine` (kagent), `clickhouse/clickhouse-server:26.3.35-alpine` (agentgateway-system)
- `rustfs/rustfs:1.0.0-beta.3@sha256:378642b0...`
- `python:3.13-slim` (typesafe-jev-mcp); `python:3.11-slim` (agent-desk, outside the requested namespaces)
- `127.0.0.1:5000/fortigate-mcp:dev` — locally built, `:dev` tag, no version to resolve
- Outside the requested namespaces, listed for completeness only, unchecked: `pgvector/pgvector:pg16`, `registry:2`, `rancher/local-path-provisioner:v0.0.31`, `ghcr.io/tailscale/tailscale:v1.102.4`

**Caveat:** OSV results cover the application/Go module only — no base-image or OS-package (Alpine/Debian layer) scanning was performed. A distro scanner (e.g. trivy/grype) is needed to cover the "not queried" list.
