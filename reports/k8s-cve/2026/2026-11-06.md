# k8s-cve - 2026-11-06

**Verdict: ACTION NEEDED** — unpatched CRITICAL + HIGH in Argo CD, 12 advisories (10 HIGH) in CoreDNS, a HIGH cross-node RCE in Flannel, and 3 MODERATE in the containerd node runtime; every other mappable component is clean or already past the fix.

## Cluster / scope

| Item | Value |
|---|---|
| Cluster | omni-viper (single node `talos-cyv-ekb`) |
| Kubernetes | **v1.36.4** |
| OS / image | Talos (v1.13.10), kernel 6.18.48-talos (amd64) |
| Container runtime | **containerd 2.2.7** |
| Namespaces inventoried | kagent, ate-system, agentgateway-system, argocd, external-secrets, cert-manager, kube-system (+ cluster-wide workload scan) |
| Data source | OSV.dev `/v1/query` (24 queries, all successful) |

## Findings

| Component | Version | CVE / advisory | Severity | Fixed version |
|---|---|---|---|---|
| **Argo CD** (`quay.io/argoproj/argocd`) – server, repo-server, application-controller, applicationset, notifications | v3.5.3 | CVE-2026-77459 (GHSA-fmxq-cgp8-87wp / GO-2026-6784) – AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete hooks | **CRITICAL** (`CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H`) | **3.5.4** |
| **Argo CD** (same image) | v3.5.3 | CVE-2026-55797 (GHSA-j6cw-g6p4-7hch / GO-2026-6701) – repo-server command injection via crafted SSH repo SOCKS5 proxy URL | **HIGH** | **3.5.4** |
| **CoreDNS** (`registry.k8s.io/coredns/coredns`) | v1.12.3 | CVE-2025-58063 (GHSA-93mf-426m-g6x9 / GO-2025-3942) – DNS cache pinning via etcd lease ID confusion | HIGH | 1.12.4 |
| CoreDNS | v1.12.3 | CVE-2026-26017 (GHSA-c9v3-4pv7-87pr / GO-2026-4630) – ACL bypass | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 (GHSA-h75p-j8xm-m278 / GO-2026-4635) – loop-detection DoS | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2025-68151 (GHSA-527x-5wrf-22m2 / GO-2026-4289) – gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-32934 (GHSA-2wpx-qpw2-g5h5 / GO-2026-4969) – DoQ worker pool does not bound stream backlog | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 (GHSA-63cw-r7xf-jmwr / GO-2026-5164) – DoH GET oversized `dns=` param CPU/memory amplification | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 (GHSA-qhmp-q7xh-99rh / GO-2026-5583) – TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 (GHSA-vp29-5652-4fw9 / GO-2026-5667) – TSIG auth bypass on gRPC and QUIC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 (GHSA-h8mm-c463-wjq3 / GO-2026-5417) – transfer stanza lexicographic compare (subzone ACL bypass) | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-82399 (GHSA-mrg3-qvqr-jw29 / GO-2026-6507) – unauthenticated memory exhaustion in custom transports | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-86003 (GHSA-9gm5-9rfh-m6vx / GO-2026-6506) – DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-62994 (GHSA-74w3-63xv-x9mv / GO-2026-6805) – `k8s_external` headless AXFR panics `transfer` plugin | LOW | 1.14.5 |
| **Flannel** (`ghcr.io/siderolabs/flannel`, kube-flannel DS) | v0.27.2 | CVE-2026-32241 (GHSA-vchx-5pr6-ffx2 / GO-2026-4894) – cross-node RCE via extension-backend `BackendData` injection | **HIGH** | **0.28.2** (via Talos update) |
| **containerd** (node runtime, from Talos) | 2.2.7 | CVE-2026-53495 (GHSA-7jxh-36q5-gcqv / GO-2026-6444) | MODERATE (CVSS4.0) | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-95838 (GHSA-rp3h-jf77-q9p4 / GO-2026-6796) | MODERATE (CVSS4.0) | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-53493 (GHSA-pg57-6jwg-q645 / GO-2026-6597) | MODERATE (CVSS4.0) | 2.2.9 |
| Kubernetes – all control-plane components + `kube-proxy` (v1.34.11) | v1.36.4 / v1.34.11 | CVE-2025-1767 (GHSA-3wgm-2gw2-vh5m / GO-2025-3521) – GitRepo volume inadvertent local repository access | not stated in OSV Go record | none given (range `introduced: 0`, no fixed event) |
| Kubernetes – kube-apiserver | v1.36.4 / v1.34.11 | CVE-2024-7598 (GHSA-r56h-j38w-hrqq / GO-2025-3547) – kube-apiserver race condition | not stated in OSV Go record | none given (range `introduced: 1.3.0`, no fixed event) |
| External Secrets Operator (`ghcr.io/external-secrets/external-secrets`) | v2.9.0 | CVE-2026-42875 (GHSA-wv26-88m5-6h59 / GO-2026-5723) – namespace-isolation bypass in CAProvider ConfigMap resolution | not stated in OSV | 2.4.0 (advisory `ECOSYSTEM` range) → **already fixed at 2.9.0, informational only** |
| Dex (`ghcr.io/dexidp/dex`, argocd-dex-server) | v2.45.1 | GO-2024-2476 / CVE-2024-23656 – OSV range is `introduced 2.37.0 → fixed 2.38.0` | n/a | 2.38.0 → **not affected at 2.45.1** |
| cert-manager (controller, cainjector, webhook) | v1.21.2 | OSV: **no advisories match v1.21.2** (package has 3 advisories total – CVE-2024-12401, CVE-2026-25518, CVE-2026-62290 – all capped at ≤1.20.x) | – | – |

### Recommended actions (priority order)
1. **Argo CD v3.5.3 → v3.5.4** (or ≥3.6.0): closes the CRITICAL AppProject restriction bypass CVE-2026-77459 and the repo-server command injection CVE-2026-55797.
2. **CoreDNS v1.12.3 → ≥1.14.7** (or the highest CoreDNS pinned for k8s 1.36): closes 10 HIGH + 1 MODERATE + 1 LOW, incl. two TSIG authentication bypasses.
3. **Talos 1.13.10 → a build carrying Flannel ≥0.27.3/0.28.2 and containerd ≥2.2.9**: fixes the HIGH Flannel cross-node RCE CVE-2026-32241 and the three MODERATE containerd issues. Flannel runs host-networked on the only node, so treat as priority.
4. **kube-proxy v1.34.11 → v1.36.4** to match the control plane (skew; also keeps it on a supported patch line).
5. Kubernetes CVE-2025-1767 / CVE-2024-7598: OSV lists no fixed release and the cluster is already on the newest 1.36.x — track the upstream advisories (both are REVIEWED, with upstream PR/issue referenced) rather than chasing a patch that OSV does not name.
6. External Secrets v2.9.0 and Dex v2.45.1 need no action — both are past the fixed versions reported by OSV.

## Not checked (could not be mapped to an OSV package)

| Image | Where | Why not checked |
|---|---|---|
| `us-docker.pkg.dev/solo-public/substrate-enterprise/{ateapi,atecontroller,ate-idp-server,atelet,kubernetes-secrets,ateworker-gvisor}:v0.6.0-b15382d4` | ate-system, kagent | Private Solo Labs build; upstream `github.com/kagent-dev/substrate` has **zero** advisories in OSV (verified with an unfiltered query), and the image tag is a build hash, not a module version |
| `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` | atenet-egress, atenet-router | Commit-pinned Rust build; `agentgateway` is not present on crates.io/OSV — cannot map to a versioned package |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/{agentgateway-enterprise,enterprise-agentgateway-controller,ext-auth-service,rate-limiter,waf-server}:2026.10.2` | agentgateway-system | Private enterprise images, no public OSV package |
| `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12`, `solo-enterprise/{solo-enterprise-ui-frontend:1.0.0-alpha12, solo-enterprise-ui-frontend:0.5.9, solo-enterprise-ui-backend:0.5.9, solo-enterprise-autoauth:v0.2.2, solo-enterprise-autoauth:v0.2.3}` | kagent, agentgateway-system | Private Solo Labs images; `github.com/kagent-dev/kagent` returned no OSV data at all (verified unfiltered) |
| `us-docker.pkg.dev/solo-public/substrate-enterprise/podcertcontroller:v0.6.0-b15382d4` | podcertificate-controller-system | Private build, unmappable |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/redis:8.6.7-alpine`, `public.ecr.aws/docker/library/redis:8.2.3-alpine` | agentgateway-system, argocd | Alpine package probe (`Alpine`/`redis`) returned no data → base-OS layer not covered |
| `nginx:1.27-alpine` | kagent-ui-tls | Alpine probe returned no data; floating minor tag |
| `clickhouse/clickhouse-server:26.3.17-alpine`, `:26.3.35-alpine` | kagent, agentgateway-system | Not present in OSV |
| `docker.io/otel/opentelemetry-collector-contrib:0.161.0` | kagent, agentgateway-system | Go module query returned no advisories (no OSV data for this release); OS/base layer not covered |
| `python:3.13-slim`, `python:3.11-slim` | kagent (typesafe-jev-mcp), remarkable-diary (agent-desk) | Base images with no patch-level tag; needs an image scanner, not a Go/OSV module query |
| `127.0.0.1:5000/fortigate-mcp:dev` | kagent | Local, untagged dev build |
| `registry:2`, `rancher/local-path-provisioner:v0.0.31`, `ghcr.io/tailscale/tailscale:v1.102.4` | registry, local-path-storage, tailscale | Outside the priority scope this run; not queried |
| Talos OS 1.13.10 / kernel 6.18.48-talos | node | No OSV ecosystem for Talos — use `talosctl health`/Talos SecureScan for the OS layer |

## Method & caveats
- Kubernetes queried as `Go` / `k8s.io/kubernetes` at `1.36.4` (and `1.34.11` for the lagging kube-proxy); platform components queried at their image tag versions (`argo-cd/v3@3.5.3`, `cert-manager@1.21.2`, `external-secrets@2.9.0`, `coredns@1.12.3`, `dex@2.45.1`, `flannel@0.27.2`, `containerd/containerd/v2@2.2.7`).
- Only `id`, `aliases`, `summary`, `severity`/CVSS and fixed-version events are reported; every row above comes from an OSV response in this session. Rows where OSV supplies no severity are marked as such rather than inferred.
- Go-module-level checks only: the OS/base-image layer of each container (Alpine/Debian packages, Python stdlib, bundled binaries) is **not** covered by these queries and needs a registry image scanner (e.g. trivy/grype).
- Severity for the containerd rows comes from CVSS4.0 vectors in the OSV record; no findings are fabricated where OSV omitted a rating.
