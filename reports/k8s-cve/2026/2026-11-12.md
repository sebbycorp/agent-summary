# k8s-cve - 2026-11-12

**Verdict: ACTION NEEDED** — CRITICAL/HIGH advisories with published fixes in Argo CD, local-path-provisioner, CoreDNS and flannel; Kubernetes, cert-manager and external-secrets are clean or advisory-only.

- Cluster: **omni-viper** — single node `talos-cyv-ekb`, Kubernetes **v1.36.4**, Talos **v1.13.10**, containerd **2.2.7**
- Scope: namespaces kagent, ate-system, agentgateway-system, argocd, external-secrets, cert-manager, kube-system (+ cluster-wide workload inventory for images)
- Source: **OSV.dev** `POST /v1/query` (reachable, 23 queries). Every row below is copied from an OSV response in this session.
- Read-only run: no cluster mutation, no Secret data read or printed.

## Findings (action needed)

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Argo CD (`quay.io/argoproj/argocd`: repo-server/server/application-controller/applicationset/notifications) | v3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp, GO-2026-6784 — AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete hooks | **CRITICAL** | 3.5.4 (also 3.4.10 / 3.3.15 / 3.6.0-rc2) |
| Argo CD (same image) | v3.5.3 | CVE-2026-55797 / GHSA-j6cw-g6p4-7hch, GO-2026-6701 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | **HIGH** | 3.5.4 |
| local-path-provisioner (`rancher/local-path-provisioner`) | v0.0.31 | CVE-2025-62878 / GHSA-jr3w-9vfr-c746, GO-2026-4425 — path traversal via `parameters.pathPattern` | **CRITICAL** | 0.0.34 |
| local-path-provisioner | v0.0.31 | CVE-2026-44543 / GHSA-7fxv-8wr2-mfc4, GO-2026-5218 — HelperPod template injection | **HIGH** | 0.0.36 |
| CoreDNS (`registry.k8s.io/coredns/coredns`, kube-system) | v1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr, GO-2026-4630 — ACL bypass | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GHSA-h75p-j8xm-m278, GO-2026-4635 — loop-detection DoS | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-32934 / GHSA-2wpx-qpw2-g5h5, GO-2026-4969 — DoQ worker pool unbounded stream backlog | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GHSA-63cw-r7xf-jmwr, GO-2026-5164 — DoH GET oversized `dns=` CPU/memory amplification | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GHSA-h8mm-c463-wjq3, GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh, GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GHSA-vp29-5652-4fw9, GO-2026-5667 — TSIG auth bypass on gRPC and QUIC | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29, GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx, GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9, GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | **HIGH** | 1.12.4 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GHSA-527x-5wrf-22m2, GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv, GO-2026-6805 — `k8s_external` headless AXFR panics `transfer` plugin | LOW | 1.14.5 |
| flannel (`ghcr.io/siderolabs/flannel`, CNI) | v0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2, GO-2026-4894 — cross-node RCE via extension-backend BackendData injection | **HIGH** | 0.28.2 |
| containerd (node runtime, Talos) | 2.2.7 | CVE-2026-53493 / GHSA-pg57-6jwg-q645, GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | MODERATE | 2.2.9 |
| containerd | 2.2.7 | CVE-2026-53495 / GHSA-7jxh-36q5-gcqv, GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | MODERATE | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-95838 / GHSA-rp3h-jf77-q9p4, GO-2026-6796 — unpack DoS via repeated opaque whiteouts | MODERATE | 2.2.8 |

**Recommended:** Argo CD → v3.5.4+, local-path-provisioner → v0.0.36+, CoreDNS → v1.14.7+ (Talos control-plane manifest), flannel → v0.28.2+, containerd → 2.2.9+ (via a Talos patch update).

## Review only (no immediate action)

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Kubernetes API server / kubelet | v1.36.4 | CVE-2025-1767 / GO-2025-3521 — GitRepo volume inadvertent local repository access | unrated (Go advisory; range `introduced:0`, no fixed version) | none given |
| kube-proxy image (`registry.k8s.io/kube-proxy`) | v1.34.11 | CVE-2024-7598 / GO-2025-3547 — kube-apiserver race condition | unrated (open-ended range) | none given |
| Talos Linux (node OS) | v1.13.10 | CVE-2026-77564 / GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | LOW | 1.14.0-alpha only (no stable fix in OSV data) |
| external-secrets (`ghcr.io/external-secrets/external-secrets`) | v2.9.0 | CVE-2026-42875 / GO-2026-5723 — namespace-isolation bypass in CAProvider ConfigMap resolution for SecretStore | unrated, **`review_status: UNREVIEWED`** | OSV custom range says `fixed: 2.4.0` — **likely false positive**: running 2.9.0 already post-dates 2.4.0 while the standard Go range is open-ended `introduced:0`. Confirm upstream rather than downgrading. |
| dex (`ghcr.io/dexidp/dex:v2.45.1`, argocd-dex-server) | v2.45.1 | CVE-2024-23656 / GO-2024-2476 — discards TLS config, serves TLS 1.0/1.1 + weak ciphers | unrated | **Not affected** — OSV `custom_ranges` scope this to `>=2.37.0 <2.38.0`; the flat `introduced:0` is a known Go-importer imprecision. No action. |

## Checked and clean

| Component | Version | OSV result |
|---|---|---|
| cert-manager controller / cainjector / webhook | v1.21.2 | 0 vulns. Module path validated with a positive control (v1.12.0 correctly returned CVE-2024-12401), so the empty result is genuine. |
| opentelemetry-collector-contrib (`otel/opentelemetry-collector-contrib`) | 0.161.0 | 0 vulns (core `go.opentelemetry.io/collector` also 0) |
| kagent tools (`ghcr.io/kagent-dev/kagent/tools`) | 0.3.0 | 0 vulns |
| tailscale (`ghcr.io/tailscale/tailscale`, tailscale/homelab-router) | v1.102.4 | 0 vulns |
| distribution/registry (`registry:2`, registry namespace) | queried at 2.8.3 | 0 vulns — caveat: `registry:2` is a floating tag, so the running build could be any 2.x; pin to a digest. |

## Not checked (image could not be mapped to an OSV package)

- **Solo.io / kagent enterprise (closed-source, no public OSV module):** `kagent-enterprise-controller:1.0.0-alpha12`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3`, `solo-enterprise-ui-frontend:1.0.0-alpha12` / `0.5.9`, `solo-enterprise-ui-backend:0.5.9`
- **Enterprise agentgateway:** `agentgateway-enterprise:2026.10.2`, `enterprise-agentgateway-controller:2026.10.2`, `ext-auth-service:2026.10.2`, `rate-limiter:2026.10.2`, `waf-server:2026.10.2`
- **Substrate / ate (no Go module published at these tags):** `ateapi:v0.6.0-b15382d4`, `atecontroller:v0.6.0-b15382d4`, `ate-idp-server:v0.6.0-b15382d4`, `atelet:v0.6.0-b15382d4`, `kubernetes-secrets:v0.6.0-b15382d4`, `podcertcontroller:v0.6.0-b15382d4`
- **Commit-pinned proxy:** `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` — commit-SHA tag, no release version to query
- **gVisor runtime:** `ateworker-gvisor:v0.6.0-b15382d4` — sandbox is gVisor but its version is not derivable from the image tag, so no defensible query (a probe at a guessed `github.com/google/gvisor` version returning empty is **not** evidence of "clean")
- **Non-Go / unmodelled stacks:** `rustfs/rustfs:1.0.0-beta.3` (Rust), `clickhouse/clickhouse-server:26.3.35-alpine` and `:26.3.17-alpine`, `pgvector/pgvector:pg16`
- **Redis:** `public.ecr.aws/docker/library/redis:8.2.3-alpine` (argocd-redis) and `…/enterprise-agentgateway/redis:8.6.7-alpine` (ext-cache) — OSV's Redis ecosystem endpoint returned **HTTP 400** for these versions; use distro/OS scanner data instead
- **nginx:** `nginx:1.27-alpine` (kagent-ui-tls) — OSV Alpine query returned empty; tag→distro package version not resolvable
- **Python / local app images:** `python:3.13-slim` (typesafe-jev-mcp), `python:3.11-slim` (remarkable-diary/agent-desk), `127.0.0.1:5000/fortigate-mcp:dev` (local-only mutable `:dev` tag, unpinned)

## Notes / caveats

- OSV.dev was reachable throughout; no query was skipped for connectivity reasons. Query budget used: **23/25**.
- Severity shown is OSV's `database_specific.severity` (GHSA-derived). `unrated` means the OSV entry has no severity field — typically Go-only advisories — and is **not** "no risk"; those need manual CVSS triage.
- Priority order followed per runbook: Kubernetes version → platform images (kagent, agentgateway, substrate/ate, argocd, cert-manager, external-secrets) → cluster add-ons; everything else is listed under "not checked".
- Images inventory came from deployment/daemonset/statefulset specs (pod listings do not expose image columns).
