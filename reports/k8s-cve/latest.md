# k8s-cve - 2026-11-16

**Verdict: ACTION NEEDED** — Argo CD v3.5.3 is affected by 2 unpatched advisories (one CRITICAL), kube-flannel v0.27.2 and CoreDNS v1.12.3 are several minor releases behind known fixes, and external-secrets v2.9.0 predates a namespace-isolation fix; all other checked platform components are clean.

- Cluster: `omni-viper` — single node `talos-cyv-ekb`, Kubernetes **v1.36.4**, Talos **v1.13.10**, containerd **2.2.7**, kernel **6.18.48-talos**
- Data source: OSV.dev API (`https://api.osv.dev/v1/query`), reached successfully from this session — 17 queries issued
- Every CVE/advisory ID below is taken verbatim from an OSV response in this session; nothing is inferred. Where OSV returns no `severity` field it is reported as *not provided*.

## Findings (OSV hits against the running versions)

| Component | Running version | CVE / advisory | Severity | Fixed version | Notes |
|---|---|---|---|---|---|
| Argo CD (`quay.io/argoproj/argo-cd` → `argocd:v3.5.3`; server, repo-server, application-controller, applicationset, notifications) | 3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp / GO-2026-6784 — AppProject destination-namespace and resource restrictions bypassed by PreDelete/PostDelete resource hooks | **CRITICAL** (CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H) | **3.5.4** (also 3.3.15 / 3.4.10 / 3.6.0-rc2) | In range `3.5.0 → 3.5.4`. Impact is on the managed destination clusters, not only Argo CD itself. |
| Argo CD (same image) | 3.5.3 | CVE-2026-55797 / GHSA-j6cw-g6p4-7hch / GO-2026-6701 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | **HIGH** (CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H) | **3.5.4** | In range `3.5.0 → 3.5.4`. Triggerable by a malicious/compromised Git repo URL. |
| kube-flannel (`ghcr.io/siderolabs/flannel`) | v0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2 / GO-2026-4894 — cross-node RCE via extension backend `BackendData` injection | **HIGH** | **0.28.2** | DaemonSet `kube-flannel`, single node. Patch via a Talos/Sidero flannel image bump. |
| CoreDNS (`registry.k8s.io/coredns/coredns`) | v1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx / GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | **HIGH** | 1.14.7 | Cluster DNS is in-cluster only and the exploits target DoH/DoQ/gRPC/DoH3 listeners, which the Talos CoreDNS deployment does not expose — practical risk reduced, but the version is many releases behind. |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29 / GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | 1.14.7 | Same custom-transport caveat. |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh / GO-2026-5583 and CVE-2026-35579 / GHSA-vp29-5652-4fw9 / GO-2026-5667 — TSIG authentication bypass (DoT/DoH/DoH3/DoQ/gRPC, and gRPC/QUIC) | **HIGH** (×2) | 1.14.3 | TSIG not used in the default Corefile — low practical exposure. |
| CoreDNS | v1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr / GO-2026-4630 (ACL bypass) and CVE-2026-26018 / GHSA-h75p-j8xm-m278 / GO-2026-4635 (loop-detection DoS) | **HIGH** (×2) | 1.14.2 | `acl`/`rewrite` plugins not in use; `loop` plugin is standard in the Talos Corefile. |
| CoreDNS | v1.12.3 | CVE-2026-32934, CVE-2026-32936, CVE-2026-33489 (GHSA-2wpx-qpw2-g5h5, GHSA-63cw-r7xf-jmwr, GHSA-h8mm-c463-wjq3 + GO-2026-4969/5164/5417) — DoQ worker-pool DoS, DoH GET amplification, transfer subzone ACL bypass | **HIGH** (×3) | 1.14.3 | DoH/DoQ/transfer plugins not enabled here. |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GHSA-527x-5wrf-22m2 / GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 | |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv / GO-2026-6805 — `k8s_external` headless AXFR panics `transfer` plugin | LOW | 1.14.5 | |
| CoreDNS | v1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9 / GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | **HIGH** | **1.12.4** | Only affects the `etcd` plugin, not used in this Corefile. |
| external-secrets (`ghcr.io/external-secrets/external-secrets`) | v2.9.0 | CVE-2026-42875 / GHSA-wv26-88m5-6h59 / GO-2026-5723 — namespace-isolation bypass in CAProvider ConfigMap resolution for `SecretStore` | not provided by OSV (Go report; GHSA unreviewed severity) | **2.4.0** per OSV custom range | **Likely false positive**: OSV's machine-readable range is `introduced: 0` with no `fixed`; the fixed bound (2.4.0) appears only in free text/custom ranges, so v2.9.0 is almost certainly already fixed. Verify against upstream GHSA-wv26-88m5-6h59. |
| Kubernetes API server / controller-manager / scheduler / kubelet (`k8s.io/kubernetes`) | v1.36.4 | CVE-2025-1767 / GHSA-3wgm-2gw2-vh5m / GO-2025-3521 — GitRepo volume inadvertent local repository access | not provided by OSV | none published in OSV (range `introduced: 0`, no fixed) | Requires the `gitRepo` volume type; disable/mitigate via Pod Security. |
| Kubernetes (same) | v1.36.4 | CVE-2024-7598 / GHSA-r56h-j38w-hrqq / GO-2025-3547 — kube-apiserver race condition | not provided by OSV | none published in OSV (range `introduced: 1.3.0`, no fixed) | Long-standing open Go advisory; upstream classifies as low-impact. |
| kube-proxy (`registry.k8s.io/kube-proxy`) | **v1.34.11** | same two `k8s.io/kubernetes` advisories as above (CVE-2025-1767, CVE-2024-7598) | not provided by OSV | none published in OSV | Note the **version skew**: kube-proxy is v1.34.11 while the API server is v1.36.4 (still inside the 3-version skew window, but worth aligning). |
| Talos Linux (`github.com/siderolabs/talos`) | v1.13.10 | CVE-2026-77564 / GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | LOW | 1.14.0-alpha.0.0.20260513110206-76080416beee (pre-release only) | Only matters when `talosctl support-bundle` output is shared off-cluster. |

## Clean (queried, no OSV findings at the running version)

| Component | Version | Query |
|---|---|---|
| cert-manager controller / cainjector / webhook | v1.21.2 | `github.com/cert-manager/cert-manager` @ 1.21.2, Go → 0 vulns |
| Argo CD | — | see findings above (image `quay.io/argoproj/argocd:v3.5.3`) |
| Dex (`ghcr.io/dexidp/dex`) | v2.45.1 | `github.com/dexidp/dex` @ 2.45.1 returned only GO-2024-2476 / CVE-2024-23656 (weak TLS 1.0/1.1), whose range is *from v2.37.0 before v2.38.0* — v2.45.1 is **outside** the affected range, so treat as not affected |

## Not checked (could not map image → OSV package/version)

- `us-docker.pkg.dev/solo-public/substrate-enterprise/*` — ateapi, atecontroller, ate-idp-server, ateworker-gvisor, kubernetes-secrets @ `v0.6.0-b15382d4` (private Solo/Layr build; no public Go module path to query)
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/*` — agentgateway-enterprise, enterprise-agentgateway-controller, ext-auth-service, rate-limiter, waf-server, redis `8.6.7-alpine` @ `2026.11.0` (private registry, CalVer tag not resolvable to an upstream version)
- `us-docker.pkg.dev/solo-public/solo-enterprise/*` — solo-enterprise-ui-frontend `1.0.0-alpha12`/`0.5.9`, ui-backend `0.5.9`, solo-enterprise-autoauth `v0.2.2`/`v0.2.3` (private)
- `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` (private)
- `ghcr.io/kagent-dev/substrate/agentgateway@sha256:ebebf5a7…` (atenet-egress / atenet-router) and `ghcr.io/kagent-dev/kagent/tools:0.3.0` — queried `agentgateway` (crates.io) and `github.com/kagent-dev/kagent` (Go): both returned **no data**; the running digests cannot be mapped to a release version
- `docker.io/otel/opentelemetry-collector-contrib:0.161.0` — queried `go.opentelemetry.io/collector` and `github.com/open-telemetry/opentelemetry-collector-contrib` @ 0.161.0: **no OSV data** for those versions, so no coverage claim either way
- `rustfs/rustfs:1.0.0-beta.3` (Rust S3-compatible store) — no crate name/version confirmed
- `clickhouse/clickhouse-server:26.3.35-alpine` and `:26.3.17-alpine`; `pgvector/pgvector:pg16` (PostgreSQL 16) — no OSV ecosystem coverage for these at those tags
- Base/OS layer images not image-inspectable from the API: `nginx:1.27-alpine` (kagent-ui-tls), `redis:8.2.3-alpine` (argocd-redis), `redis 8.6.7-alpine` (ext-cache), `python:3.13-slim` (typesafe-jev-mcp), `fortigate-mcp:dev` (locally built, `127.0.0.1:5000`), containerd **2.2.7**, Talos host packages — OSV Alpine/Debian lookups need an exact package release (`-rN`) that requires an image manifest scan; probe queries against Alpine returned no data
- Non-target namespaces left out of this run by design (time budget): `default`, `local-path-storage`, `podcertificate-controller-system`, `postgres`, `registry`, `remarkable-diary`, `tailscale`

## Recommended actions (priority order)

1. **Argo CD v3.5.3 → v3.5.4** — closes CVE-2026-77459 (CRITICAL) and CVE-2026-55797 (HIGH). One manifest tag change; all five argocd workloads share the image.
2. **kube-flannel → v0.28.2** (via Talos/Sidero image) — closes CVE-2026-32241 (HIGH).
3. **CoreDNS → 1.14.7** (or the version shipped by the next Talos patch) — clears 12 open advisories.
4. **Verify external-secrets v2.9.0 vs CVE-2026-42875** against GHSA-wv26-88m5-6h59 before spending an upgrade; OSV's structured range looks like a scanner false positive.
5. Keep `talosctl support-bundle` output on-cluster until Talos ships a stable fix for CVE-2026-77564; confirm `gitRepo` volumes are unusable (Pod Security / policy) for CVE-2025-1767.
6. Consider aligning `kube-proxy` (v1.34.11) with the v1.36 control plane, and add image-scanning (Trivy/Grype) for the private Solo/substrate/agentgateway/rustfs/clickhouse images that OSV cannot map.
