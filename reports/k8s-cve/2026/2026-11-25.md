# k8s-cve - 2026-11-25

**Verdict: ACTION NEEDED** — CoreDNS 1.12.3 (10 HIGH advisories), Flannel 0.27.2 (HIGH cross-node RCE, fixed 0.28.2) and Argo CD v3.5.3 (1 CRITICAL + 1 HIGH, fixed 3.5.4) all have published fixes; patch these three first.

- Cluster: `omni-viper`, single node `talos-cyv-ekb`, Kubernetes **v1.36.4** on **Talos v1.13.10**, containerd 2.2.7, kernel 6.18.48-talos.
- Source of every CVE below: **OSV.dev** (`https://api.osv.dev/v1/query`), 25 queries this run, all successful. Nothing below is inferred from memory.
- Namespaces inventoried: `kagent`, `ate-system`, `agentgateway-system`, `argocd`, `external-secrets`, `cert-manager`, `kube-system`.

## CVE table (OSV hits)

| Component | Version | CVE / advisory | Severity | Fixed version |
|---|---|---|---|---|
| CoreDNS (`registry.k8s.io/coredns/coredns`) | v1.12.3 | CVE-2025-58063 (GHSA-93mf-426m-g6x9 / GO-2025-3942) — DNS cache pinning via etcd lease ID confusion | HIGH | 1.12.4 |
| CoreDNS | v1.12.3 | CVE-2026-26017 (GHSA-c9v3-4pv7-87pr / GO-2026-4630) — ACL bypass | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 (GHSA-h75p-j8xm-m278 / GO-2026-4635) — loop-detection DoS | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-32934 (GHSA-2wpx-qpw2-g5h5 / GO-2026-4969) — DoQ worker pool unbounded stream backlog | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 (GHSA-63cw-r7xf-jmwr / GO-2026-5164) — DoH GET oversized `dns=` pre-validation CPU/memory burn | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 (GHSA-qhmp-q7xh-99rh / GO-2026-5583) — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 (GHSA-h8mm-c463-wjq3 / GO-2026-5417) — transfer stanza lexicographic compare → subzone ACL bypass | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 (GHSA-vp29-5652-4fw9 / GO-2026-5667) — TSIG auth bypass on gRPC/QUIC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-82399 (GHSA-mrg3-qvqr-jw29 / GO-2026-6507) — unauthenticated memory exhaustion in custom transports | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-86003 (GHSA-9gm5-9rfh-m6vx / GO-2026-6506) — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2025-68151 (GHSA-527x-5wrf-22m2 / GO-2026-4289) — gRPC/HTTPS/HTTP3 servers lack resource limits | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-62994 (GHSA-74w3-63xv-x9mv / GO-2026-6805) — `k8s_external` headless AXFR empty transfer batch panics | LOW | 1.14.5 |
| Flannel (`ghcr.io/siderolabs/flannel`) | v0.27.2 | CVE-2026-32241 (GHSA-vchx-5pr6-ffx2 / GO-2026-4894) — cross-node RCE via extension-backend `BackendData` injection (affected ≤ 0.28.1) | HIGH | 0.28.2 |
| Argo CD (`quay.io/argoproj/argocd`) | v3.5.3 | CVE-2026-77459 (GHSA-fmxq-cgp8-87wp / GO-2026-6784) — AppProject destination-namespace / resource restrictions bypassed | CRITICAL | 3.5.4 (also 3.4.10, 3.3.15) |
| Argo CD | v3.5.3 | CVE-2026-55797 (GHSA-j6cw-g6p4-7hch / GO-2026-6701) — repo-server command injection via crafted SSH repo SOCKS5 proxy URL | HIGH | 3.5.4 (also 3.4.10, 3.3.15) |
| Talos Linux (node OS) | v1.13.10 | CVE-2026-77564 (GHSA-7cxh-94cf-mfp6) — support bundle does not redact registry credentials | LOW | 1.14.0 (fix commit 2026-05-13) |
| Kubernetes (`k8s.io/kubernetes`) | v1.36.4 | CVE-2025-1767 (GO-2025-3521, GHSA-3wgm-2gw2-vh5m) — GitRepo volume inadvertent local repository access | not given by OSV | not given (no `fixed` event in OSV's Go range) |
| Kubernetes | v1.36.4 | CVE-2024-7598 (GO-2025-3547, GHSA-r56h-j38w-hrqq) — kube-apiserver race condition | not given by OSV | not given (no `fixed` event in OSV's Go range) |
| kube-proxy (`registry.k8s.io/kube-proxy`) | v1.34.11 | same two Go advisories as above (same `k8s.io/kubernetes` data) | not given by OSV | not given |
| External Secrets (`ghcr.io/external-secrets/external-secrets`) | v2.9.0 | CVE-2026-42875 (GO-2026-5723) — namespace isolation bypass in CAProvider ConfigMap resolver | not given by OSV | 2.4.0 per OSV range → **fix already present in 2.9.0; treat as stale hit** |
| Dex (`ghcr.io/dexidp/dex`, argocd-dex-server) | v2.45.1 | CVE-2024-23656 (GO-2024-2476) — TLS config discarded, serves TLS 1.0/1.1 + weak ciphers | not given by OSV | 2.38.0 (OSV custom range 2.37.0→2.38.0) → **2.45.1 above the fix; likely not affected** |

### Checked and clean (OSV returned zero advisories)

- cert-manager `quay.io/jetstack/cert-manager-controller` / `-cainjector` / `-webhook` **v1.21.2** → `github.com/cert-manager/cert-manager` (Go): no vulns.
- OpenTelemetry Collector Contrib `docker.io/otel/opentelemetry-collector-contrib` **0.163.2** (agentgateway-system) and **0.161.0** (kagent) → `github.com/open-telemetry/opentelemetry-collector-contrib` and `go.opentelemetry.io/collector` (Go): no vulns at either version.

### Notes / caveats

- The two Kubernetes hits are the only OSV entries for `k8s.io/kubernetes`; OSV's Go ranges carry no `fixed` event, so they cannot be version-refuted here. Both are historical advisory entries (2024/2025) — no fixed-version claim is available, so they are listed for completeness, not as confirmed exposures on 1.36.4.
- `nginx:1.27-alpine` (kagent-ui-tls): OSV Alpine data for `nginx` returns a large set of advisories (e.g. ALPINE-CVE-2026-113425, ALPINE-CVE-2026-97629) with fixes in the 1.28.3-r8 / 1.30.6-r0 lines, but the tag alone does not reveal the installed apk release, so applicability is unconfirmed → move to a currently-supported nginx alpine tag rather than treating it as a specific CVE.
- Operational (non-CVE): `kube-proxy` is pinned to **v1.34.11** while the control plane is **v1.36.4** — a 2-minor skew; also 5 `agentgateway-system`/`argocd` pods are stuck in `Completed`/`Error` from older ReplicaSets.

## Not checked (could not map to an OSV package)

Solo.io / private-registry and unmappable images — OSV returned no data or the project is not published as an OSV-covered package:

1. `cr.agentgateway.dev/agentgateway:v1.7.0` (atenet-router, atenet-egress) — no `agentgateway` entry in Go or crates.io.
2. `us-docker.pkg.dev/solo-public/enterprise-agentgateway/agentgateway-enterprise:2026.11.1-patch.0` (agentgateway-proxy, bedrock-proxy) — private enterprise build.
3. `.../enterprise-agentgateway/enterprise-agentgateway-controller:2026.11.1-patch.0` — private.
4. `.../enterprise-agentgateway/ext-auth-service:2026.11.1-patch.0` — private.
5. `.../enterprise-agentgateway/rate-limiter:2026.11.1-patch.0` — private.
6. `.../enterprise-agentgateway/waf-server:2026.11.1-patch.0` — private.
7. `.../enterprise-agentgateway/redis:8.6.8-alpine` (ext-cache) — distroless/enterprise redis fork, apk release unknown.
8. `.../solo-enterprise/solo-enterprise-ui:2026.11.2` (frontend), `solo-enterprise-ui-backend:0.5.18`, `solo-enterprise-ui-frontend:0.5.18` — private.
9. `.../solo-enterprise/solo-enterprise-autoauth:v0.2.2` / `v0.2.4` — private.
10. Substrate / ATE suite at `v1.1.0-alpha1-7354c972`: `ateapi`, `atecontroller`, `ate-idp-server`, `atelet`, `kubernetes-secrets` (k8s-credential-provider), `ateworker-gvisor` — no public OSV module mapping.
11. `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:2026.11.2` — private enterprise build.
12. `ghcr.io/kagent-dev/kagent/tools:0.3.1` — queried as `github.com/kagent-dev/kagent` Go module: no OSV coverage returned for that path.
13. `rustfs/rustfs:1.0.3` (ate-system object store) — no crates.io package at that version.
14. `clickhouse/clickhouse-server:26.8.36-alpine` (kagent + agentgateway clickhouse) — no OSV coverage returned.
15. `public.ecr.aws/docker/library/redis:8.2.3-alpine` (argocd-redis) — OSS-Fuzz/Debian/Alpine queries returned no data; needs a scanner with the image manifest (trivy/grype).
16. `nginx:1.27-alpine` (kagent-ui-tls) — see caveat above; apk release not determinable from the tag.
17. `127.0.0.1:5000/fortigate-mcp:dev` and `python:3.13-slim` (typesafe-jev-mcp) — private image with a mutable `dev` tag, and a language base image with no package mapping; needs an SBOM-based scanner.
18. Not inventoried at all (outside the 7 key namespaces, no time budget): `postgres`, `registry`, `tailscale`, `local-path-storage`, `podcertificate-controller-system`, `remarkable-diary`, `default` lab pods.

## Recommended actions (priority order)

1. **Argo CD v3.5.3 → v3.5.4** (CRITICAL AppProject restriction bypass + HIGH repo-server command injection).
2. **CoreDNS v1.12.3 → v1.14.7+** (10 HIGH; Talos controls this image — set `cluster.coreDNS.image.tag` in the Talos machine config).
3. **Flannel v0.27.2 → ≥ v0.28.2** (HIGH cross-node RCE; consume via a Talos patch that ships the newer CNI).
4. Consider **Talos v1.13.10 → v1.14.x** for CVE-2026-77564 (LOW) and the Flannel bump.
5. Review the **kube-proxy v1.34.11 vs v1.36.4** skew.
6. For items 1–17 of "not checked", run an image scanner that resolves digests + apk/dpkg SBOMs (trivy/grype) — OSV.dev package queries cannot cover these closed registries.
