# k8s-cve - 2026-11-23

**VERDICT: ACTION NEEDED** — 1 CRITICAL (Argo CD 3.5.3) + 3 CRITICAL-class (nginx TLS sidecar), plus HIGH findings in flannel CNI (cross-node RCE), containerd, CoreDNS (x12), Redis (x5) affecting running workloads in `argocd`, `kube-system`, `kagent`, `agentgateway-system`, `ate-system`.

- Cluster: **omni-viper** — single node `talos-cyv-ekb`, Kubernetes **v1.36.4**, Talos **v1.13.10**, containerd **2.2.7**, kernel 6.18.48-talos
- Report generated: 2026-11-23 (America/Toronto) by `k8s_cve_checker` (read-only)
- Data source: **OSV.dev** (`https://api.osv.dev/v1/query`) — 23 queries issued, all reachable; **every row below comes from an OSV response obtained in this session**. Nothing is inferred from memory.
- Go advisory entries were cross-checked against `affected[].ranges[].events` (and `ecosystem_specific.custom_ranges`) so that OSV's imprecise Go auto-mappings do not create false positives; two such cases are called out below.

## 1. Inventory checked

| Layer | Version | Source |
|---|---|---|
| Kubernetes (apiserver / controller-manager / scheduler) | v1.36.4 | node `kubectl get nodes` VERSION |
| kube-proxy | `registry.k8s.io/kube-proxy:v1.34.11` | kube-system DaemonSet |
| CoreDNS | `registry.k8s.io/coredns/coredns:v1.12.3` | kube-system Deployment |
| flannel CNI | `ghcr.io/siderolabs/flannel:v0.27.2` | kube-system DaemonSet |
| containerd (node runtime) | 2.2.7 | node CONTAINER-RUNTIME |
| Talos Linux (node OS) | v1.13.10 | node OS-IMAGE |
| Argo CD | `quay.io/argoproj/argocd:v3.5.3` | argocd ns |
| Dex | `ghcr.io/dexidp/dex:v2.45.1` | argocd-dex-server |
| Argo CD Redis | `public.ecr.aws/docker/library/redis:8.2.3-alpine` | argocd-redis |
| cert-manager | controller/cainjector/webhook `v1.21.2` | cert-manager ns |
| External Secrets Operator | `ghcr.io/external-secrets/external-secrets:v2.9.0` | external-secrets ns |
| kagent tools | `ghcr.io/kagent-dev/kagent/tools:0.3.0` | kagent ns |
| Substrate/ate | `ateapi`, `atecontroller`, `atelet`, `ate-idp-server`, `kubernetes-secrets`, `ateworker-gvisor` `v0.6.0-b15382d4` | ate-system / kagent |
| agentgateway (substrate OSS) | `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` | atenet-router / atenet-egress |
| agentgateway enterprise | `…/enterprise-agentgateway/*:2026.11.1` | agentgateway-system |
| Solo enterprise UI/auth | `solo-enterprise-ui-frontend:1.0.0-alpha12`, `:0.5.9`, `ui-backend:0.5.9`, `autoauth:v0.2.2`/`v0.2.3` | kagent / agentgateway-system |
| kagent enterprise controller | `kagent-enterprise-controller:1.0.0-alpha12` | kagent |
| OTel Collector Contrib | `docker.io/otel/opentelemetry-collector-contrib:0.161.0` | kagent / agentgateway-system |
| nginx TLS sidecar | `nginx:1.27-alpine` (floating tag) | kagent-ui-tls |
| Redis (agentgateway ext-cache) | `…/enterprise-agentgateway/redis:8.6.8-alpine` | agentgateway-system |
| ClickHouse | `clickhouse/clickhouse-server:26.3.17-alpine`, `:26.3.35-alpine` | kagent / agentgateway-system |
| RustFS | `rustfs/rustfs:1.0.0-beta.3@sha256:378642b0…` | ate-system |
| Misc | `127.0.0.1:5000/fortigate-mcp:dev`, `python:3.13-slim` | kagent |

## 2. Action needed — prioritised

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Argo CD — all argocd deployments + application-controller | v3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp / GO-2026-6784 — AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete resource hooks | **CRITICAL** | **3.5.4** |
| Argo CD — repo-server | v3.5.3 | CVE-2026-55797 / GHSA-j6cw-g6p4-7hch / GO-2026-6701 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | **HIGH** | **3.5.4** |
| flannel CNI (`ghcr.io/siderolabs/flannel`, privileged node DaemonSet) | v0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2 / GO-2026-4894 — cross-node RCE via extension backend `BackendData` injection | **HIGH** | **0.28.2** |
| containerd (node runtime) | 2.2.7 | CVE-2026-95839 / GHSA-vwhp-8p66-mq4f — image-triggered daemon crash via malformed digest in subject-annotated descriptor | **HIGH** | **2.2.10** |
| containerd (node runtime) | 2.2.7 | CVE-2026-53495 / GHSA-7jxh-36q5-gcqv / GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | MODERATE | **2.2.8** |
| containerd (node runtime) | 2.2.7 | CVE-2026-53493 / GHSA-pg57-6jwg-q645 / GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | MODERATE | **2.2.9** |
| containerd (node runtime) | 2.2.7 | CVE-2026-95838 / GHSA-rp3h-jf77-q9p4 / GO-2026-6796 — unpack DoS via repeated opaque whiteouts | MODERATE | **2.2.8** |
| nginx TLS sidecar (`nginx:1.27-alpine`, `kagent-ui-tls`) — floating tag; queried at 1.27.4 (all 20 upstream advisories reach the whole 1.27 line) | 1.27.x | CVE-2026-15176 — `ngx_http_core_module` vulnerability | **CRITICAL** | 1.30.6 / 1.31.7 |
| nginx TLS sidecar | 1.27.x | CVE-2026-42533 — Map directive + regex matching vulnerability | **CRITICAL** | 1.30.4 / 1.31.3 |
| nginx TLS sidecar | 1.27.x | CVE-2026-42945 — `ngx_http_rewrite_module` vulnerability | **CRITICAL** | 1.30.1 |
| nginx TLS sidecar | 1.27.x | CVE-2026-1642, CVE-2026-27651, CVE-2026-27654, CVE-2026-27784, CVE-2026-32647, CVE-2026-42946, CVE-2026-56434, CVE-2026-60005, CVE-2026-97629 (9 advisories) | HIGH | 1.28.3 / 1.28.4 / 1.30.1 / 1.30.4 / 1.30.6 (per advisory) |
| nginx TLS sidecar | 1.27.x | CVE-2025-53859, CVE-2026-28753, CVE-2026-28755, CVE-2026-40460, CVE-2026-40701, CVE-2026-42934, CVE-2026-48142, CVE-2026-95101 (8 advisories) | MEDIUM | 1.28.3 – 1.30.6 (per advisory) |
| CoreDNS (`registry.k8s.io/coredns/coredns`, kube-system) | v1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9 / GO-2025-3942 — DNS cache pinning via etcd Lease ID confusion | **HIGH** | **1.12.4** |
| CoreDNS | v1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr / GO-2026-4630 — ACL bypass | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GHSA-h75p-j8xm-m278 / GO-2026-4635 — loop-detection DoS | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-32934 / GHSA-2wpx-qpw2-g5h5 / GO-2026-4969 — DoQ worker pool does not bound stream backlog | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GHSA-63cw-r7xf-jmwr / GO-2026-5164 — DoH GET oversized `dns=` param CPU/memory amplification | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GHSA-h8mm-c463-wjq3 / GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh / GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GHSA-vp29-5652-4fw9 / GO-2026-5667 — TSIG auth bypass on gRPC and QUIC transports | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx / GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29 / GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GHSA-527x-5wrf-22m2 / GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv / GO-2026-6805 — `k8s_external` headless AXFR panics the `transfer` plugin (introduced 1.9.4) | LOW | 1.14.5 |
| Redis (`public.ecr.aws/docker/library/redis:8.2.3-alpine`, `argocd-redis`) | 8.2.3 | CVE-2026-23479 / GHSA-93m2-935m-8rj3 — redis-server use-after-free in unblock-client flow, possible RCE | **HIGH** | **8.2.6** |
| Redis (`argocd-redis`) | 8.2.3 | CVE-2026-23631 / GHSA-8ghh-qpmp-7826 — redis-server Lua use-after-free, possible RCE | **HIGH** | **8.2.6** |
| Redis (`argocd-redis`) | 8.2.3 | CVE-2026-25243 / GHSA-c8h9-259x-jff4 — redis-server `RESTORE` invalid memory access, possible RCE | **HIGH** | **8.2.6** |
| Redis (`argocd-redis`) | 8.2.3 | CVE-2026-25588 / GHSA-7jwr-g5qv-w3gw — RedisTimeSeries `RESTORE` invalid memory access, possible RCE | **HIGH** | **8.2.6** |
| Redis (`argocd-redis`) | 8.2.3 | CVE-2026-25589 / GHSA-7862-34pw-44wv — RedisBloom `RESTORE` invalid memory access, possible RCE | **HIGH** | **8.2.6** |
| Talos Linux (node OS) | v1.13.10 | CVE-2026-77564 / GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | LOW | 1.14.0 (commit `76080416beee`) |
| Kubernetes (apiserver / controller-manager / scheduler) | v1.36.4 | CVE-2025-1767 / GHSA-3wgm-2gw2-vh5m / GO-2025-3521 — GitRepo volume inadvertent local repository access (`introduced: 0`, unbounded) | not stated by OSV | *no fixed version in OSV* |
| Kubernetes (kube-apiserver) | v1.36.4 | CVE-2024-7598 / GHSA-r56h-j38w-hrqq / GO-2025-3547 — race condition in kube-apiserver (`introduced: 1.3.0`, unbounded) | not stated by OSV | *no fixed version in OSV* |
| kube-proxy (`registry.k8s.io/kube-proxy`) | **v1.34.11** | Same two as above (queried at 1.34.11: GO-2025-3521, GO-2025-3547) | not stated by OSV | *none given* — **also 2 minor versions behind the v1.36.4 control plane (version skew)** |

## 3. Checked — clean (no OSV match at the running version)

| Component | Version | Result |
|---|---|---|
| cert-manager controller / cainjector / webhook | v1.21.2 | `github.com/cert-manager/cert-manager` @ 1.21.2 → **no advisories** |
| External Secrets Operator (+ cert-controller, webhook) | v2.9.0 | OSV returned 1 entry (GO-2026-5723 / CVE-2026-42875 / GHSA-wv26-88m5-6h59, namespace-isolation bypass in CAProvider ConfigMap resolution) **but** its upstream range closes at **v2.4.0** (`ecosystem_specific.custom_ranges`) while we run v2.9.0 → **likely NOT affected** (unreviewed Go auto-mapping). Verify before patching. |
| Dex (`argocd-dex-server`) | v2.45.1 | OSV returned GO-2024-2476 / CVE-2024-23656 (deprecated TLS 1.0/1.1), true range **v2.37.0 → before v2.38.0** → **NOT affected** at v2.45.1 (false positive) |
| kagent tools (`ghcr.io/kagent-dev/kagent/tools`) | 0.3.0 | `github.com/kagent-dev/kagent` @ 0.3.0 → **no advisories** |
| OpenTelemetry Collector Contrib | 0.161.0 | `github.com/open-telemetry/opentelemetry-collector-contrib` and `go.opentelemetry.io/collector` @ 0.161.0 → **no advisories** |
| Redis (`…/enterprise-agentgateway/redis:8.6.8-alpine`, ext-cache) | 8.6.8 | Unscoped `redis` query returned **no upstream advisories covering 8.6.8** (the 8.x findings above are all fixed by 8.2.6 / 8.4.3 / 8.6.3) |
| kube-proxy as a Go module | v1.34.11 | `k8s.io/kube-proxy` @ 1.34.11 → no advisories; the k8s findings are carried by the `k8s.io/kubernetes` row |

## 4. Not checked — could not map to an OSV package

| Image / component | Namespace | Why not checked |
|---|---|---|
| `…/enterprise-agentgateway/agentgateway-enterprise:2026.11.1` (agentgateway-proxy, bedrock-proxy) | agentgateway-system | Private Solo.io image; tag is not a published release; no OSV Go/Rust module mapping |
| `…/enterprise-agentgateway/enterprise-agentgateway-controller:2026.11.1` | agentgateway-system | Private image, unmappable |
| `…/enterprise-agentgateway/ext-auth-service:2026.11.1`, `rate-limiter:2026.11.1`, `waf-server:2026.11.1` | agentgateway-system | Private images, unmappable |
| `…/solo-public/solo-enterprise/solo-enterprise-ui-frontend:1.0.0-alpha12` / `:0.5.9`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3` | kagent, agentgateway-system | Private Solo enterprise images, unmappable |
| `…/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` | kagent | Private image, unmappable |
| `…/substrate-enterprise/ateapi`, `atecontroller`, `atelet`, `ate-idp-server`, `kubernetes-secrets`, `ateworker-gvisor` `:v0.6.0-b15382d4` | ate-system, kagent | Enterprise builds with a non-release tag; the open-source `github.com/kagent-dev/substrate` module was **not** queryable within this run's budget — contents unverified |
| `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` (atenet-router, atenet-egress) | ate-system | Digest-pinned dev build; tag is not a release version, no OSV mapping |
| `clickhouse/clickhouse-server:26.3.17-alpine` (kagent-clickhouse) and `:26.3.35-alpine` (management-clickhouse) | kagent, agentgateway-system | C++ project; no usable OSV package match at these tags |
| `rustfs/rustfs:1.0.0-beta.3@sha256:378642b0…` | ate-system | crates.io query for `rustfs` @ 1.0.0-beta.3 returned no data — not indexed under that name |
| `nginx:1.27-alpine` **Alpine base packages** (as opposed to nginx upstream, checked in §2) | kagent | `Alpine` / `Alpine:v3.21` package queries for nginx/redis returned no OSV data; base-image OS CVEs need a distro SBOM scanner |
| `python:3.13-slim` (typesafe-jev-mcp) | kagent | Debian-based image; needs a distro scanner (Debian security tracker), not an OSV Go/Rust module |
| `127.0.0.1:5000/fortigate-mcp:dev` | kagent | Local, unversioned `dev` tag — no version to query |
| Talos host userland + kernel `6.18.48-talos` | node | Requires an OS-level SBOM scanner; the OSV Go query covers Talos Go code only |
| gVisor sandbox runtime (used by `ateworker-gvisor` pools) | kagent | Runtime version not exposed in pod/deployment specs |

## 5. Recommended actions (priority order)

1. **Argo CD v3.5.3 → v3.5.4** — CRITICAL AppProject restriction bypass + HIGH repo-server command injection, both fixed in 3.5.4. Single image change across `argocd-applicationset-controller`, `argocd-server`, `argocd-repo-server`, `argocd-notifications-controller`, `argocd-application-controller`.
2. **flannel v0.27.2 → ≥ v0.28.2** (or the Sidero build carrying it) — HIGH cross-node RCE in a privileged node DaemonSet.
3. **Update Talos to a build with containerd ≥ 2.2.10** — covers all four containerd findings (the HIGH digest-crash one needs 2.2.10) and picks up the CVE-2026-77564 fix once at 1.14.x.
4. **CoreDNS v1.12.3 → ≥ 1.14.7** — clears all 12 advisories (minimum interim 1.12.4 for CVE-2025-58063).
5. **`argocd-redis` 8.2.3-alpine → ≥ 8.2.6-alpine** — five HIGH RCE-class advisories.
6. **Replace the floating `nginx:1.27-alpine` tag on `kagent-ui-tls`** with a pinned digest ≥ 1.30.6-alpine — 3 CRITICAL + 9 HIGH; the floating tag also makes the running patch level unauditable.
7. **Close the version skew**: kube-proxy is v1.34.11 vs control plane v1.36.4 — align it with the control plane.
8. **Confirm, don't patch blindly**: External Secrets v2.9.0 (advisory closes at v2.4.0) and Dex v2.45.1 (closes at v2.38.0) are returned only by unreviewed Go auto-mappings — both appear **not** affected.
9. The two Kubernetes findings (CVE-2025-1767, CVE-2024-7598) carry **no fixed version in OSV** — cross-check the Kubernetes security-announce threads referenced in the advisories before treating v1.36.4 as patched.

## 6. Query log (auditability)

Go ecosystem: `k8s.io/kubernetes` 1.36.4, 1.34.11 · `k8s.io/kube-proxy` 1.34.11 · `github.com/coredns/coredns` 1.12.3 · `github.com/flannel-io/flannel` 0.27.2 · `github.com/argoproj/argo-cd/v3` 3.5.3 · `github.com/cert-manager/cert-manager` 1.21.2 · `github.com/external-secrets/external-secrets` 2.9.0 · `github.com/dexidp/dex` 2.45.1 · `github.com/containerd/containerd/v2` 2.2.7 · `github.com/siderolabs/talos` 1.13.10 · `github.com/kagent-dev/kagent` 0.3.0 · `go.opentelemetry.io/collector` 0.161.0 · `github.com/open-telemetry/opentelemetry-collector-contrib` 0.161.0.
Other ecosystems: `redis` (unscoped) 8.2.3, 8.6.8 · `nginx` (unscoped) 1.27.4 · `rustfs` (crates.io) 1.0.0-beta.3 · `nginx`/`redis` (Alpine, incl. `Alpine:v3.21`) — HTTP 400 or empty, see §4.
Total: 23 POSTs to `https://api.osv.dev/v1/query`, all from this session; distro-package records (UBUNTU-\*, BELL-\*, CLSA-\*, ALSA/ALEA/AZL/COSMIC/DEBIAN-CVE-\*, ECHO-\*) were ignored unless the range covered the running version, to keep the table to genuine upstream exposure.
