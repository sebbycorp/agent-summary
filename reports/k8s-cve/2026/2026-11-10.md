# k8s-cve - 2026-11-10

**Verdict: ACTION NEEDED** — 23 distinct OSV advisories: 1 CRITICAL + 12 HIGH (Argo CD v3.5.3, flannel v0.27.2, CoreDNS v1.12.3), 7 MODERATE, 3 LOW. Patch now: Argo CD 3.5.3→3.5.4, flannel 0.27.2→0.28.2, CoreDNS 1.12.3→1.14.7, containerd 2.2.7→2.2.9.

- **Cluster:** omni-viper — single node `talos-cyv-ekb`, Kubernetes **v1.36.4**, Talos **v1.13.10**, containerd **2.2.7**, kernel 6.18.48-talos
- **Method:** read-only inventory via Kubernetes tools (nodes + pods + deployments/statefulsets/daemonsets), images deduplicated, then per-component OSV.dev `/v1/query` lookups (Go ecosystem) — 23 requests
- **Data source:** https://api.osv.dev only (reachable). Every row below comes from an OSV response returned in this session; nothing is inferred from memory.

## Findings

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Argo CD (`quay.io/argoproj/argocd:v3.5.3` — controller, server, repo-server, applicationset, notifications) | 3.5.3 | CVE-2026-77459 · GHSA-fmxq-cgp8-87wp · GO-2026-6784 — AppProject destination-namespace/resource restrictions bypassed by PreDelete/PostDelete hooks | **CRITICAL** | **3.5.4** |
| Argo CD (same image) | 3.5.3 | CVE-2026-55797 · GHSA-j6cw-g6p4-7hch · GO-2026-6701 — repo-server command injection via crafted SSH repo SOCKS5 proxy URL | **HIGH** | **3.5.4** |
| flannel (`ghcr.io/siderolabs/flannel:v0.27.2`, kube-system) | 0.27.2 | CVE-2026-32241 · GHSA-vchx-5pr6-ffx2 · GO-2026-4894 — cross-node RCE via extension backend BackendData injection | **HIGH** | **0.28.2** |
| CoreDNS (`registry.k8s.io/coredns/coredns:v1.12.3`) | 1.12.3 | CVE-2025-58063 · GHSA-93mf-426m-g6x9 · GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | **HIGH** | **1.12.4** |
| CoreDNS | 1.12.3 | CVE-2026-26017 · GHSA-c9v3-4pv7-87pr · GO-2026-4630 — ACL bypass | **HIGH** | 1.14.2 |
| CoreDNS | 1.12.3 | CVE-2026-26018 · GHSA-h75p-j8xm-m278 · GO-2026-4635 — loop-detection DoS | **HIGH** | 1.14.2 |
| CoreDNS | 1.12.3 | CVE-2026-32934 · GHSA-2wpx-qpw2-g5h5 · GO-2026-4969 — DoQ worker pool unbounded stream backlog | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-32936 · GHSA-63cw-r7xf-jmwr · GO-2026-5164 — DoH GET oversized `dns=` CPU/memory amplification | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-33489 · GHSA-h8mm-c463-wjq3 · GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-33190 · GHSA-qhmp-q7xh-99rh · GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-35579 · GHSA-vp29-5652-4fw9 · GO-2026-5667 — TSIG auth bypass on gRPC and QUIC | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-86003 · GHSA-9gm5-9rfh-m6vx · GO-2026-6506 — DoH/DoQ/gRPC bypass of UPDATE rejection | **HIGH** | 1.14.7 |
| CoreDNS | 1.12.3 | CVE-2026-82399 · GHSA-mrg3-qvqr-jw29 · GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | 1.14.7 |
| CoreDNS | 1.12.3 | CVE-2025-68151 · GHSA-527x-5wrf-22m2 · GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | 1.12.3 | CVE-2026-62994 · GHSA-74w3-63xv-x9mv · GO-2026-6805 — `k8s_external` headless AXFR panics `transfer` plugin | LOW | 1.14.5 |
| containerd (node runtime) | 2.2.7 | CVE-2026-53495 · GHSA-7jxh-36q5-gcqv · GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | MODERATE | **2.2.8** |
| containerd | 2.2.7 | CVE-2026-95838 · GHSA-rp3h-jf77-q9p4 · GO-2026-6796 — unpack DoS via repeated opaque whiteouts | MODERATE | **2.2.8** |
| containerd | 2.2.7 | CVE-2026-53493 · GHSA-pg57-6jwg-q645 · GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | MODERATE | **2.2.9** |
| Talos Linux (node OS) | 1.13.10 | CVE-2026-77564 · GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | LOW | 1.14.0 (dev build `1.14.0-alpha.0.0.20260513110206-76080416beee`) |
| Kubernetes (`k8s.io/kubernetes`) | 1.36.4 | CVE-2025-1767 · GHSA-3wgm-2gw2-vh5m · GO-2025-3521 — GitRepo volume inadvertent local repository access | MODERATE | none in OSV Go record (introduced 0, no `fixed`; GHSA `last_affected` 1.32.3) → verify with upstream |
| Kubernetes (`k8s.io/kubernetes`) | 1.36.4 | CVE-2024-7598 · GHSA-r56h-j38w-hrqq · GO-2025-3547 — kube-apiserver race condition | LOW | none in OSV Go record (introduced 1.3.0, no `fixed`; GHSA `last_affected` 1.32.3) → verify with upstream |
| kube-proxy (`registry.k8s.io/kube-proxy:v1.34.11`) | 1.34.11 | Same two Kubernetes advisories as above (CVE-2025-1767, CVE-2024-7598) | MODERATE / LOW | none given; **also note the 1.34.11 binary lags the v1.36.4 control plane by two minor versions (skew)** |
| external-secrets (`ghcr.io/external-secrets/external-secrets:v2.9.0`) | 2.9.0 | CVE-2026-42875 · GHSA-wv26-88m5-6h59 · GO-2026-5723 — namespace isolation bypass in CAProvider ConfigMap resolution for SecretStore | MODERATE | 2.4.0 in the ECOSYSTEM range, but the Go SEMVER range is `introduced: 0` with no upper bound — **flagged at 2.9.0; likely over-broad range, needs vendor confirmation** |
| dex (`ghcr.io/dexidp/dex:v2.45.1`, argocd-dex-server) | 2.45.1 | CVE-2024-23656 · GHSA-gr79-9v6v-gc9r · GO-2024-2476 — discards TLS config, always serves TLS 1.0/1.1 and insecure ciphers | not stated by OSV (`review_status: UNREVIEWED`) | ECOSYSTEM range 2.37.0 → fixed 2.38.0 (i.e. already fixed before 2.45.1) while the Go SEMVER range is `introduced: 0` — **contradictory; manual review required** |
| agentgateway OSS (`ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e` — atenet-router, atenet-egress) | commit-pinned `cd65181fd52e` (no semver) | CVE-2026-29791 · GHSA-v2x6-wwfw-r2rq — missing parameter sanitization in MCP→OpenAPI conversion | MODERATE | 0.12.0 — **unverifiable**: the image tag is a commit SHA, so applicability to this build cannot be confirmed from OSV |

### Clean (queried, no OSV hits)

| Component | Version | Result |
|---|---|---|
| cert-manager controller / cainjector / webhook (`quay.io/jetstack/cert-manager-*:v1.21.2`) | 1.21.2 | **No vulnerabilities returned.** Module name validated by control query (`1.14.4` → CVE-2024-12401 returned), so the empty result at 1.21.2 is a real "clean", not a name miss. |

## Not checked (could not be mapped to an OSV package)

Private / vendor-built images with no public OSV Go module (no upstream source to map):

- `solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` (kagent-controller)
- `solo-public/solo-enterprise/solo-enterprise-ui-frontend:1.0.0-alpha12`, `solo-enterprise-ui-frontend:0.5.9`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3`
- `solo-public/enterprise-agentgateway/agentgateway-enterprise:2026.10.2` (agentgateway-proxy, bedrock-proxy), `enterprise-agentgateway-controller:2026.10.2`, `ext-auth-service:2026.10.2`, `rate-limiter:2026.10.2`, `waf-server:2026.10.2`
- `substrate-enterprise/ateapi:v0.6.0-b15382d4`, `atecontroller:v0.6.0-b15382d4`, `ate-idp-server:v0.6.0-b15382d4`, `atelet:v0.6.0-b15382d4`, `ateworker-gvisor:v0.6.0-b15382d4`, `kubernetes-secrets:v0.6.0-b15382d4`

Public images whose Go module is not indexed in OSV (control queries at known-vulnerable versions also returned nothing, so the name — not the version — is the gap):

- OSS kagent tools `ghcr.io/kagent-dev/kagent/tools:0.3.0` (also digest-pinned agentgateway upstream — see caveat above)
- OpenTelemetry Collector Contrib `docker.io/otel/opentelemetry-collector-contrib:0.161.0` (×3 workloads; `github.com/open-telemetry/opentelemetry-collector-contrib` and its submodule both empty in OSV)

Base-image / non-Go stacks with no matching OSV ecosystem package name (need a container-image scanner instead — Trivy/Grype):

- `clickhouse/clickhouse-server:26.3.35-alpine` (agentgateway-system) and `:26.3.17-alpine` (kagent)
- `public.ecr.aws/docker/library/redis:8.2.3-alpine` (argocd-redis), `…/enterprise-agentgateway/redis:8.6.7-alpine` (ext-cache)
- `pgvector/pgvector:pg16` (postgres), `rustfs/rustfs:1.0.0-beta.3` (ate-system), `nginx:1.27-alpine` (kagent-ui-tls), `python:3.13-slim` (typesafe-jev-mcp), `127.0.0.1:5000/fortigate-mcp:dev` (unpinned `dev` tag — not scannable)

Also inventoried but out of the requested namespace scope, therefore not queried: `tailscale/homelab-router`, `local-path-provisioner`, `registry`, `podcertificate-controller`, `remarkable-diary/agent-desk`, `default/lab-*`.

## Recommended actions (highest first)

1. **Argo CD v3.5.3 → v3.5.4** — one patch release clears both the CRITICAL AppProject restriction bypass and the HIGH repo-server command injection. All five argocd workloads run the same image.
2. **flannel v0.27.2 → ≥ v0.28.2** — HIGH cross-node RCE on the only node; confirm the Talos-managed flannel CNI channel for the patched tag.
3. **CoreDNS v1.12.3** — the only fix covering all 13 advisories is the 1.14.x line (≥1.14.7); the single CVE that applies to the 1.12 branch alone bumps to 1.12.4. Plan a jump to the current supported CoreDNS for Talos.
4. **containerd 2.2.7 → ≥ 2.2.9** and **Talos 1.13.10 → patched 1.14.x** via a Talos machine-upgrade (both DoS-class, node-local).
5. **kube-proxy v1.34.11** is two minor versions behind the v1.36.4 control plane — align it even though its OSV hits are the same two low-impact Kubernetes advisories.
6. Ask vendors to confirm applicability of CVE-2026-42875 (external-secrets 2.9.0), CVE-2024-23656 (dex 2.45.1) and CVE-2026-29791 (agentgateway `cd65181fd52e`) — all three were returned on over-broad / unverifiable version ranges.
7. Add a container-image scanner (Trivy/Grype) to cover the ClickHouse, Redis, Postgres/pgvector, RustFS, nginx and private Solo/substrate images that OSV cannot map.
