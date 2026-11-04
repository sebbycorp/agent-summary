# k8s-cve - 2026-11-04

**Verdict: ACTION NEEDED** — Argo CD v3.5.3 is affected by a CRITICAL AppProject-restriction bypass (CVE-2026-77459, fixed in 3.5.4), CoreDNS v1.12.3 carries multiple HIGH advisories, and flannel v0.27.2 has a HIGH cross-node RCE (CVE-2026-32241).

- Cluster: `omni-viper` (single node `talos-cyv-ekb`, Talos v1.13.10, containerd 2.2.7)
- Kubernetes server (kubelet/API): **v1.36.4** — kube-proxy image is **v1.34.11** (version skew)
- Source: OSV.dev (`https://api.osv.dev/v1/query`), queried live in this session (~25 queries)
- Read-only check; no cluster mutations performed.

## CVE table

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Kubernetes (control plane) | 1.36.4 | CVE-2025-1767 / GO-2025-3521 (GHSA-3wgm-2gw2-vh5m) — GitRepo volume local-repo access | not stated by OSV | none published (open-ended range) |
| Kubernetes (control plane) | 1.36.4 | CVE-2024-7598 / GO-2025-3547 (GHSA-r56h-j38w-hrqq) — kube-apiserver race condition | not stated by OSV | none published (from 1.3.0) |
| kube-proxy (kube-system) | v1.34.11 | CVE-2025-1767 / GO-2025-3521 | not stated by OSV | none published |
| kube-proxy (kube-system) | v1.34.11 | CVE-2024-7598 / GO-2025-3547 | not stated by OSV | none published |
| Argo CD (server/repo-server/controller/applicationset/notifications) | v3.5.3 | **CVE-2026-77459 / GO-2026-6784 (GHSA-fmxq-cgp8-87wp)** — AppProject namespace/resource restrictions bypassed by PreDelete/PostDelete hooks | **CRITICAL** | **3.5.4** (3.4.10 / 3.3.15 / 3.6.0-rc2) |
| Argo CD (repo-server) | v3.5.3 | CVE-2026-55797 / GO-2026-6701 (GHSA-j6cw-g6p4-7hch) — repo-server command injection via crafted SSH repo SOCKS5 proxy URL | HIGH | 3.5.4 (3.4.10 / 3.3.15) |
| CoreDNS (kube-system) | v1.12.3 | CVE-2025-58063 / GO-2025-3942 (GHSA-93mf-426m-g6x9) — DNS cache pinning via etcd lease ID confusion | HIGH | 1.12.4 |
| CoreDNS | v1.12.3 | CVE-2026-26017 / GO-2026-4630 (GHSA-c9v3-4pv7-87pr) — ACL bypass | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GO-2026-4635 (GHSA-h75p-j8xm-m278) — loop-detection DoS | not stated by OSV | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-32934 / GO-2026-4969 (GHSA-2wpx-qpw2-g5h5) — DoQ worker pool unbounded stream backlog | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GO-2026-5583 (GHSA-qhmp-q7xh-99rh) — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GO-2026-5417 (GHSA-h8mm-c463-wjq3) — transfer stanza lexicographic compare (subzone ACL bypass) | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GO-2026-5667 (GHSA-vp29-5652-4fw9) — TSIG bypass on gRPC/QUIC | not stated by OSV | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GO-2026-5164 (GHSA-63cw-r7xf-jmwr) — DoH GET oversized `dns=` amplification | not stated by OSV | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GO-2026-4289 (GHSA-527x-5wrf-22m2) — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GO-2026-6805 (GHSA-74w3-63xv-x9mv) — `k8s_external` headless AXFR panics transfer plugin | not stated by OSV | 1.14.5 |
| CoreDNS | v1.12.3 | CVE-2026-86003 / GO-2026-6506 (GHSA-9gm5-9rfh-m6vx) — DoH/DoQ/gRPC bypass UPDATE rejection | not stated by OSV | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GO-2026-6507 (GHSA-mrg3-qvqr-jw29) — unauthenticated memory exhaustion in custom transports | not stated by OSV | 1.14.7 |
| flannel (kube-system, CNI) | v0.27.2 | CVE-2026-32241 / GO-2026-4894 (GHSA-vchx-5pr6-ffx2) — cross-node RCE via extension backend BackendData injection | HIGH | 0.28.2 |
| containerd (node runtime) | 2.2.7 | CVE-2026-53495 / GO-2026-6444 (GHSA-7jxh-36q5-gcqv) — CRI ExecSync goroutine leak → node DoS | MODERATE | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-95838 / GO-2026-6796 (GHSA-rp3h-jf77-q9p4) — unpack DoS via repeated opaque whiteouts | MODERATE | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-53493 / GO-2026-6597 (GHSA-pg57-6jwg-q645) — image-pull DoS via crafted OCI index graph | MODERATE | 2.2.9 |
| Talos Linux (node OS) | v1.13.10 | CVE-2026-77564 (GHSA-7cxh-94cf-mfp6) — support bundle does not redact registry credentials | LOW | 1.14.0-alpha.0.0.20260513110206-76080416beee (1.14 dev build) |
| agentgateway (substrate `atenet-egress` / `atenet-router`, image pinned `cd65181fd52e`) | queried as Go module 0.11.0 (proxy) | CVE-2026-29791 (GHSA-v2x6-wwfw-r2rq) — missing parameter sanitization in MCP→OpenAPI conversion | MODERATE | 0.12.0 |
| local-path-provisioner | v0.0.31 | **CVE-2025-62878 / GO-2026-4425 (GHSA-jr3w-9vfr-c746)** — path traversal via `parameters.pathPattern` | **CRITICAL** | **0.0.34** |
| local-path-provisioner | v0.0.31 | CVE-2026-44543 / GO-2026-5218 (GHSA-7fxv-8wr2-mfc4) — helper-pod template injection | HIGH | 0.0.36 |
| registry (`registry:2`, tag not version-pinned) | queried as `distribution/v3@2.8.3` (inconclusive) | CVE-2026-33540 / GO-2026-5094 (GHSA-3p65-76g6-3w7r) — pull-through cache credential exfiltration via bearer realm | HIGH | 3.1.0 |
| registry | (inconclusive mapping) | CVE-2026-41888 / GO-2026-5185 (GHSA-6pjf-3r9x-m592) — tag deletion bypasses `storage.delete.enabled` | MODERATE | 3.1.1 |
| registry | (inconclusive mapping) | CVE-2026-35172 / GO-2026-4942 (GHSA-f2g3-hh2r-cwgc) — stale blob descriptor cache invalidation | not stated by OSV | 3.1.0 |
| registry | (inconclusive mapping) | GHSA-685w-q87j-wqw3 — `manifests.Get()` does not verify content against requested digest | not stated by OSV | none published |

### Queried, no OSV advisories returned (clean at the queried version)

| Component | Version queried | Result |
|---|---|---|
| cert-manager controller / cainjector / webhook | v1.21.2 (`github.com/cert-manager/cert-manager`) | no advisories |
| external-secrets (+ cert-controller, webhook) | v2.9.0 (`github.com/external-secrets/external-secrets`) | OSV returned only CVE-2026-42875 / GO-2026-5723 (namespace-isolation bypass, CAProvider ConfigMap/SecretStore) whose Go `custom_ranges` cover `< 2.4.0` → **likely NOT affected at 2.9.0; verify manually** |
| kagent tools image | `ghcr.io/kagent-dev/kagent/tools:0.3.0` (`github.com/kagent-dev/kagent@0.3.0`) | no advisories indexed |
| substrate ate components | `github.com/kagent-dev/substrate@0.6.0` | no advisories indexed |
| opentelemetry-collector-contrib | 0.161.0 | no advisories indexed |
| tailscale (homelab-router) | v1.102.4 (`tailscale.com@1.102.4`) | no advisories indexed |
| dex (argocd-dex-server) | v2.45.1 | OSV returned only CVE-2024-23656 / GO-2024-2476 (weak TLS), whose Go `custom_ranges` cover 2.37.0–2.38.0 → **likely NOT affected at 2.45.1** |

## Not checked / could not map

- **Solo Enterprise agentgateway stack** (proprietary, no public module mapping): `agentgateway-enterprise:2026.10.2`, `enterprise-agentgateway-controller:2026.10.2`, `ext-auth-service:2026.10.2`, `rate-limiter:2026.10.2`, `waf-server:2026.10.2`
- **Substrate enterprise images** (build-hash tags `v0.6.0-b15382d4`): `ateapi`, `atecontroller`, `ate-idp-server`, `atelet`, `ateworker-gvisor`, `kubernetes-secrets`
- **kagent enterprise**: `kagent-enterprise-controller:1.0.0-alpha12`
- **Solo enterprise UI/auth**: `solo-enterprise-ui-frontend:0.5.9` / `:1.0.0-alpha12`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3`
- **substrate agentgateway pinned image** `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` — commit query (`/v1/query` with commit) returned no data; the row above is a version-proxy lookup, not authoritative for that commit
- **Distribution/base-OS content of images** (OS packages not resolvable from image refs): `redis:8.6.7-alpine`, `public.ecr.aws/docker/library/redis:8.2.3-alpine`, `nginx:1.27-alpine`, `python:3.13-slim`, `python:3.11-slim`
- **Databases / storage engines**: `clickhouse/clickhouse-server:26.3.35-alpine`, `clickhouse/clickhouse-server:26.3.17-alpine`, `pgvector/pgvector:pg16`, `rustfs/rustfs:1.0.0-beta.3`
- **Local/unversioned builds**: `127.0.0.1:5000/fortigate-mcp:dev` (no upstream package identity)
- `registry:2` — tag is not version-pinned; distribution results above are indicative only (see caveat)

## Notes

- Every CVE/ID above is copied from an OSV.dev response retrieved in this session; nothing was inferred from memory. Blank/"not stated by OSV" severities mean OSV returned no severity for that advisory.
- "Fixed version" is taken from OSV `fixed` events; where OSV published no fixed event (open-ended `introduced` ranges) the upstream may still have a patch — verify against the vendor advisory.
- Priority order for remediation: (1) Argo CD 3.5.3 → ≥3.5.4 (CRITICAL, reachable by anyone with Git write to a synced repo), (2) local-path-provisioner 0.0.31 → ≥0.0.36, (3) CoreDNS 1.12.3 → ≥1.14.7, (4) flannel v0.27.2 → ≥0.28.2, (5) Talos 1.13.10 update (bundles containerd 2.2.7 → 2.2.9 fixes).
