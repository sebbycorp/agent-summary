# k8s-cve - 2026-11-17

**Verdict: ACTION NEEDED** — Argo CD v3.5.3 carries 2 high-severity advisories fixed in v3.5.4, CoreDNS v1.12.3 has 12 unresolved advisories (latest fix 1.14.7), and node components flannel (cross-node RCE, fixed 0.28.2) and containerd 2.2.7 (3 DoS advisories, fixed 2.2.9) are behind. cert-manager and the OTel collector came back clean.

- Cluster: `omni-viper`, single node `talos-cyv-ekb`
- Kubernetes server: **v1.36.4** (node kubelet version), OS Talos **v1.13.10**, runtime **containerd 2.2.7**
- Namespaces inventoried: kagent, ate-system, agentgateway-system, argocd, external-secrets, cert-manager, kube-system (pods also observed in default, postgres, registry, tailscale, remarkable-diary, local-path-storage, podcertificate-controller-system)
- Source: OSV.dev `/v1/query` — 21 queries, all successful. Every row below comes from an OSV response returned in this session; nothing was added from memory.

## CVE table

| Component (image) | Version | CVE / advisory id | Severity (OSV) | Fixed version |
|---|---|---|---|---|
| Kubernetes (kube-apiserver / controller-manager / scheduler) | v1.36.4 | CVE-2025-1767 / GO-2025-3521 / GHSA-3wgm-2gw2-vh5m — GitRepo volume inadvertent local repository access | not stated by OSV | not stated (range `introduced: 0`, no `fixed` event) |
| Kubernetes (kube-apiserver) | v1.36.4 | CVE-2024-7598 / GO-2025-3547 / GHSA-r56h-j38w-hrqq — apiserver race condition | not stated by OSV | not stated (range `introduced: 1.3.0`, no `fixed` event) |
| kube-proxy | registry.k8s.io/kube-proxy:v1.34.11 | CVE-2025-1767 (GO-2025-3521), CVE-2024-7598 (GO-2025-3547) — same two, older build than the apiserver | not stated by OSV | not stated |
| CoreDNS | registry.k8s.io/coredns/coredns:v1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | High — CVSS:3.1 AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:H/A:N | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29 — unauthenticated memory exhaustion in custom transports | High — CVSS:3.1 AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh — TSIG authentication bypass (DoT/DoH/DoH3/DoQ/gRPC) | High — CVSS:3.1 AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GHSA-vp29-5652-4fw9 — TSIG authentication bypass (gRPC + QUIC) | High — CVSS:3.1 C:H | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr — ACL bypass | High — CVSS:3.1 AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:N/A:N | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GHSA-h8mm-c463-wjq3 — transfer stanza lexicographic compare (subzone ACL bypass) | High — CVSS:3.1 C:H | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GHSA-h75p-j8xm-m278 — loop-detection denial of service | High — CVSS:3.1 A:H | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GO-2026-4289 / GHSA-527x-5wrf-22m2 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | High — CVSS:4.0 AV:N/AC:L/PR:N/UI:N/VA:H | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-32934 / GHSA-2wpx-qpw2-g5h5 — DoQ worker pool does not bound stream backlog | High — CVSS:3.1 A:H | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GHSA-63cw-r7xf-jmwr — DoH GET oversized `dns=` query pre-validation CPU/memory amplification | High — CVSS:3.1 A:H | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9 — DNS cache pinning via etcd lease ID confusion | Medium — CVSS:3.1 AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:L/A:H | 1.12.4 |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv — `k8s_external` headless AXFR panics the `transfer` plugin | Low — CVSS:3.1 AV:N/AC:H/PR:N/UI:N/S:U/C:N/I:N/A:L | 1.14.5 |
| flannel (CNI) | ghcr.io/siderolabs/flannel:v0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2 / GO-2026-4894 — cross-node RCE via extension backend BackendData injection | High — CVSS:3.1 AV:N/AC:H/PR:L/UI:N/S:U/C:H/I:H/A:H | 0.28.2 |
| containerd (node runtime, shipped by Talos) | containerd://2.2.7 | CVE-2026-53495 / GHSA-7jxh-36q5-gcqv — CRI ExecSync goroutine leak → node-level DoS | Medium — CVSS:4.0 AV:L/AC:L/PR:L/UI:N/VA:H | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-95838 / GHSA-rp3h-jf77-q9p4 — unpack DoS via repeated opaque whiteouts | Medium — CVSS:4.0 AV:L/AC:L/VA:L | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-53493 / GHSA-pg57-6jwg-q645 — image-pull DoS via crafted OCI index graph amplification | Medium — CVSS:4.0 AV:N/AC:L/PR:N/UI:N/VA:L | 2.2.9 |
| Argo CD (server, repo-server, application-controller, notifications, applicationset) | quay.io/argoproj/argocd:v3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp / GO-2026-6784 — AppProject destination-namespace and resource restrictions bypassed by PreDelete/PostDelete resource hooks | **Critical-class — CVSS:3.1 AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H** | 3.5.4 (also 3.3.15 / 3.4.10 / 3.6.0-rc2) |
| Argo CD | v3.5.3 | CVE-2026-55797 / GHSA-j6cw-g6p4-7hch / GO-2026-6701 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | **High — CVSS:3.1 AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H** | 3.5.4 |
| dex (argocd-dex-server) | ghcr.io/dexidp/dex:v2.45.1 | CVE-2024-23656 / GO-2024-2476 / GHSA-gr79-9v6v-gc9r — TLS config discarded, serves deprecated TLS 1.0/1.1 and insecure ciphers | not stated by OSV | 2.38.0 |
| redis (argocd-redis) | public.ecr.aws/docker/library/redis:8.2.3-alpine | CVE-2026-23479 / GHSA-93m2-935m-8rj3 — use-after-free in unblock-client flow, may allow RCE | High — CVSS:4.0 AV:N/PR:L/VC:H/VI:H/VA:H | 8.2.6 |
| redis (argocd-redis) | 8.2.3-alpine | CVE-2026-23631 / GHSA-8ghh-qpmp-7826 — Lua use-after-free, may allow RCE | High — CVSS:3.1 AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H | 8.2.6 |
| redis (argocd-redis) | 8.2.3-alpine | CVE-2026-25243 / GHSA-c8h9-259x-jff4 — RESTORE invalid memory access, may allow RCE | High — CVSS:4.0 VC:H/VI:H/VA:H | 8.2.6 |
| redis (argocd-redis) | 8.2.3-alpine | CVE-2026-25588 (RedisTimeSeries RESTORE) / CVE-2026-25589 (RedisBloom RESTORE) | High — CVSS:4.0 VC:H/VI:H/VA:H | 8.2.6 |
| nginx (kagent-ui-tls front-proxy) | nginx:1.27-alpine | 21 Bitnami-ecosystem advisories incl. CVE-2026-15176 (ngx_http_core), CVE-2026-40460 (ngx_quic), CVE-2026-95101 (ngx_http_v3), CVE-2026-40701 / CVE-2026-97629 (ngx_http_ssl), CVE-2026-42533 (map/regex), CVE-2025-23419 (TLS session resumption) | not stated by OSV | 1.30.6 / 1.31.7 line; the 1.27 branch ends at 1.27.4 |
| external-secrets (operator, cert-controller, webhook) | ghcr.io/external-secrets/external-secrets:v2.9.0 | CVE-2026-42875 / GO-2026-5723 / GHSA-wv26-88m5-6h59 — namespace isolation bypass in CAProvider ConfigMap resolution for SecretStore | not stated by OSV (record `review_status: UNREVIEWED`) | main range has no `fixed` event; the ecosystem-specific range says `fixed: 2.4.0` — verify against the upstream GHSA before concluding v2.9.0 is vulnerable |
| cert-manager (controller, cainjector, webhook) | quay.io/jetstack/cert-manager-{controller,cainjector,webhook}:v1.21.2 | **no OSV findings** | — | — |
| OTel collector (solo-enterprise-telemetry-collector, k8sobjects-collector) | docker.io/otel/opentelemetry-collector-contrib:0.161.0 | **no OSV findings** (`go.opentelemetry.io/collector` and `github.com/open-telemetry/opentelemetry-collector-contrib` @ v0.161.0) | — | — |

## Recommended actions (priority order)

1. **Argo CD → v3.5.4** — both findings are network-reachable and sit on the repo-server and AppProject enforcement path (CVSS 9.x-class vectors).
2. **CoreDNS → ≥1.14.7** — 12 advisories apply to v1.12.3, several with cluster-wide confidentiality or availability impact.
3. **flannel → v0.28.2** — CVE-2026-32241 is a cross-node RCE; single-node topology limits but does not remove the risk.
4. **containerd 2.2.7 → ≥2.2.9 via a Talos patch bump** — 2.2.9 fixes the network-facing image-pull DoS.
5. **redis 8.2.3-alpine → 8.2.6-alpine** (three RCE-class fixes on the same minor line) and refresh the pinned **nginx:1.27-alpine**, whose branch is EOL in the advisory data.
6. **dex v2.45.1** — only the 2024 TLS-hardening advisory applies; upgrade alongside the next Argo CD bump.
7. **external-secrets v2.9.0** — confirm CVE-2026-42875 against the upstream GHSA; the OSV Go record's fixed-version metadata is internally inconsistent.

## Not checked / not mappable

Queried OSV with **zero entries returned** — treat as "unknown", not "clean":

- `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` (atenet-router, atenet-egress) — commit/digest-pinned tag, no release version to query.
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/*:2026.11.0` (agentgateway-enterprise, ext-auth-service, rate-limiter, waf-server, controller) — closed-source builds; queries on `github.com/agentgateway/agentgateway` (Go) and `agentgateway` (crates.io) returned nothing.
- `us-docker.pkg.dev/solo-public/substrate-enterprise/*:v0.6.0-b15382d4` (ateapi, atecontroller, atelet, ate-idp-server, kubernetes-secrets, objectstore-plugin, ategworker-gvisor) — `github.com/kagent-dev/substrate@v0.6.0` returned no OSV entries; enterprise build tags unverifiable.
- `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` and `solo-enterprise/*` (ui-frontend 1.0.0-alpha12, ui-backend 0.5.9, autoauth v0.2.2/v0.2.3) — no OSV module for these Solo enterprise images. `ghcr.io/kagent-dev/kagent/tools:0.3.0` queried as `github.com/kagent-dev/kagent@v0.3.0` returned no entries.
- `rustfs/rustfs:1.0.0-beta.3` — crates.io `rustfs` query returned no package.
- Talos node OS v1.13.10 — `github.com/siderolabs/talos/pkg/machinery@v1.13.10` returned no entries; Sidero publishes TAL OS advisories outside OSV.

Not queried (25-query budget — **no OSV query issued**, so no conclusion either way):

- `clickhouse/clickhouse-server:26.3.35-alpine` (agentgateway-system) and `:26.3.17-alpine` (kagent).
- `pgvector/pgvector:pg16` (postgres namespace).
- `registry:2` (registry namespace), `rancher/local-path-provisioner`, the Tailscale sidecar in tailscale/homelab-router.
- `127.0.0.1:5000/fortigate-mcp:dev`, `python:3.13-slim` (typesafe-jev-mcp), and the `lab-*` / `agent-desk` workload images — cluster-local or unmapped upstream projects.
- `registry.k8s.io/kubectl:v1.36.2` (ate-system CronJob substrate-cert-restart) — expected to carry the same two k8s.io/kubernetes advisories listed above; not re-queried.

## Method notes

- Kubernetes queried as ecosystem `Go`, package `k8s.io/kubernetes`, version `1.36.4` (plus `1.34.11` for the lagging kube-proxy image).
- The Severity column reproduces the CVSS vector OSV returned. Where OSV supplied no `severity` array it is marked "not stated by OSV" rather than inferred. Qualitative labels (High/Medium/Low) are my reading of those vectors, not an OSV-provided rating.
- The nginx and redis results come from the **Bitnami** ecosystem (the Docker Library nginx/redis images are Bitnami-maintained), so they may not match the Alpine variants running here exactly.
- OSV.dev was reachable for all 21 queries; none failed.
- Read-only run: no cluster resources were created, modified, or deleted.
