# k8s-cve - 2026-11-22

**VERDICT: ACTION NEEDED** — 1 CRITICAL (Argo CD), multiple HIGH (flannel CNI RCE, containerd, CoreDNS x12, Redis, nginx TLS sidecar) affecting running workloads in `argocd`, `kube-system`, `kagent`, `agentgateway-system`.

- Cluster: **omni-viper** — single node `talos-cyv-ekb`, Kubernetes **v1.36.4**, Talos **v1.13.10**, containerd **2.2.7**, kernel 6.18.48-talos
- Report generated: 2026-11-22 (America/Toronto) by `k8s_cve_checker` (read-only)
- Data source: **OSV.dev** (`https://api.osv.dev/v1/query`) — 24 requests issued (21 answered, 3 rejected HTTP 400), within the 25-query budget. Every row below comes from an OSV response obtained in this session.
- `DEBIAN-CVE-*`, `ALSA/ALEA/AZL/CLSA/BELL/ALPINE/MGASA/RHSA` distro-package advisories were ignored unless the query was scoped to the running version; upstream ranges were verified against `ranges[].events` to avoid false positives from OSV's imprecise Go auto-mappings.

---

## 1. Action needed — prioritised

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Argo CD (`quay.io/argoproj/argocd`, all argocd deployments + `argocd-application-controller`) | v3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp / GO-2026-6784 — AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete resource hooks | **CRITICAL** | **3.5.4** |
| Argo CD (repo-server) | v3.5.3 | CVE-2026-55797 / GHSA-j6cw-g6p4-7hch / GO-2026-6701 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | **HIGH** | **3.5.4** |
| flannel CNI (`ghcr.io/siderolabs/flannel`, kube-system DaemonSet) | v0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2 / GO-2026-4894 — cross-node remote code execution via extension backend `BackendData` injection | **HIGH** | **0.28.2** |
| containerd (node runtime, Talos) | 2.2.7 | CVE-2026-95839 / GHSA-vwhp-8p66-mq4f — image-triggered daemon crash via malformed digest in subject-annotated descriptor | **HIGH** | **2.2.10** |
| containerd (node runtime) | 2.2.7 | CVE-2026-53495 / GHSA-7jxh-36q5-gcqv / GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | MODERATE | **2.2.8** |
| containerd (node runtime) | 2.2.7 | CVE-2026-53493 / GHSA-pg57-6jwg-q645 / GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | MODERATE | **2.2.9** |
| containerd (node runtime) | 2.2.7 | CVE-2026-95838 / GHSA-rp3h-jf77-q9p4 / GO-2026-6796 — unpack DoS via repeated opaque whiteouts | MODERATE | **2.2.8** |
| CoreDNS (`registry.k8s.io/coredns/coredns`, kube-system) | v1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9 / GO-2025-3942 — DNS cache pinning via etcd Lease ID confusion | **HIGH** | **1.12.4** |
| CoreDNS | v1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr / GO-2026-4630 — ACL bypass | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GHSA-h75p-j8xm-m278 / GO-2026-4635 — loop-detection DoS | **HIGH** | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-32934 / GHSA-2wpx-qpw2-g5h5 / GO-2026-4969 — DoQ worker pool does not bound stream backlog | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GHSA-63cw-r7xf-jmwr / GO-2026-5164 — DoH GET oversized `dns=` param CPU/memory amplification | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GHSA-h8mm-c463-wjq3 / GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh / GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GHSA-vp29-5652-4fw9 / GO-2026-5667 — TSIG auth bypass on gRPC and QUIC | **HIGH** | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx / GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29 / GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GHSA-527x-5wrf-22m2 / GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv / GO-2026-6805 — `k8s_external` headless AXFR panics the `transfer` plugin | LOW | 1.14.5 |
| Redis (`public.ecr.aws/docker/library/redis`, `argocd-redis`) | 8.2.3-alpine | CVE-2026-23479 / GHSA-93m2-935m-8rj3 — redis-server use-after-free in unblock-client flow, possible RCE | **HIGH** | **8.2.6** |
| Redis (`argocd-redis`) | 8.2.3-alpine | CVE-2026-23631 / GHSA-8ghh-qpmp-7826 — redis-server Lua use-after-free, possible RCE | **HIGH** | **8.2.6** |
| Redis (`argocd-redis`) | 8.2.3-alpine | CVE-2026-25243 / GHSA-c8h9-259x-jff4 — redis-server `RESTORE` invalid memory access, possible RCE | **HIGH** | **8.2.6** |
| Redis (`argocd-redis`) | 8.2.3-alpine | CVE-2026-25588 / GHSA-7jwr-g5qv-w3gw — RedisTimeSeries `RESTORE` invalid memory access, possible RCE | **HIGH** | **8.2.6** |
| Redis (`argocd-redis`) | 8.2.3-alpine | CVE-2026-25589 / GHSA-7862-34pw-44wv — RedisBloom `RESTORE` invalid memory access, possible RCE | **HIGH** | **8.2.6** |
| nginx TLS sidecar (`nginx:1.27-alpine`, `kagent-ui-tls`) — floating tag, queried at **1.27.4**; all 20 upstream advisories start below 1.27.x, so the whole 1.27 line is affected (verify the running patch level) | 1.27.x | CVE-2026-15176 — `ngx_http_core_module` vulnerability | **CRITICAL** | 1.30.6 / 1.31.7 |
| nginx TLS sidecar (`kagent-ui-tls`) | 1.27.x | CVE-2026-42533 — Map directive and regex matching vulnerability | **CRITICAL** | 1.30.4 / 1.31.3 |
| nginx TLS sidecar (`kagent-ui-tls`) | 1.27.x | CVE-2026-42945 — `ngx_http_rewrite_module` vulnerability | **CRITICAL** | 1.30.1 |
| nginx TLS sidecar (`kagent-ui-tls`) | 1.27.x | CVE-2026-1642, CVE-2026-27651, CVE-2026-27654, CVE-2026-27784, CVE-2026-32647, CVE-2026-42946, CVE-2026-56434, CVE-2026-60005, CVE-2026-97629 (9 advisories) | **HIGH** | 1.28.3 / 1.28.4 and 1.30.x (per advisory) |
| nginx TLS sidecar (`kagent-ui-tls`) | 1.27.x | CVE-2025-53859, CVE-2026-28753, CVE-2026-28755, CVE-2026-40460, CVE-2026-40701, CVE-2026-42934, CVE-2026-48142, CVE-2026-95101 (8 advisories) | MEDIUM | 1.28.x – 1.30.x (per advisory) |
| Talos Linux (node OS) | v1.13.10 | CVE-2026-77564 / GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | LOW | 1.14.0 (commit `76080416beee`) |
| Kubernetes (apiserver / controller-manager / scheduler) | v1.36.4 | CVE-2025-1767 / GHSA-3wgm-2gw2-vh5m / GO-2025-3521 — GitRepo volume inadvertent local repository access | not stated by OSV | *no fixed version in OSV (unbounded `introduced: 0`)* |
| Kubernetes (kube-apiserver) | v1.36.4 | CVE-2024-7598 / GHSA-r56h-j38w-hrqq / GO-2025-3547 — race condition in kube-apiserver | not stated by OSV | *no fixed version in OSV (unbounded from 1.3.0)* |
| kube-proxy (`registry.k8s.io/kube-proxy`) | **v1.34.11** | CVE-2025-1767 / GO-2025-3521 and CVE-2024-7598 / GO-2025-3547 (same two as above) | not stated by OSV | *none given* — **also: 2 minor versions behind the v1.36.4 control plane (version skew)** |

## 2. Checked — clean (no OSV match at the running version)

| Component | Version | Result |
|---|---|---|
| cert-manager controller / cainjector / webhook | v1.21.2 | OSV returned no advisories for `github.com/cert-manager/cert-manager` @ 1.21.2 |
| External Secrets Operator (+ cert-controller, webhook) | v2.9.0 | 1 advisory returned (CVE-2026-42875 / GHSA-wv26-88m5-6h59, namespace-isolation bypass in CAProvider ConfigMap resolution) **but** its upstream range is "before v2.4.0" (`ecosystem_specific.custom_ranges`) while the running version is v2.9.0 → **likely NOT affected**; OSV entry is `review_status: UNREVIEWED`. Verify manually before patching. |
| kagent tools (`ghcr.io/kagent-dev/kagent/tools`) | 0.3.0 | No advisories for `github.com/kagent-dev/kagent` |
| Substrate / ate (`ateapi`, `atecontroller`, `atelet`, `ate-idp-server`, `kubernetes-secrets`, `ateworker-gvisor`) | v0.6.0-b15382d4 | No advisories for `github.com/kagent-dev/substrate` |
| OpenTelemetry Collector Contrib (`otel/opentelemetry-collector-contrib`) | 0.161.0 | No advisories for `github.com/open-telemetry/opentelemetry-collector-contrib` |
| Dex (`ghcr.io/dexidp/dex`, `argocd-dex-server`) | v2.45.1 | OSV returned GO-2024-2476 / CVE-2024-23656 (weak TLS 1.0/1.1), but its true range is **v2.37.0 → before v2.38.0** → **NOT affected** at v2.45.1 (Go-DB auto-mapping false positive; `UNREVIEWED`) |
| CoreDNS (control comparison) | 1.31.11 | No advisories — confirms the 12 CoreDNS advisories above are genuine 1.12.3 exposure, not DB-wide noise |

## 3. Not checked — could not map to an OSV package

| Image / component | Namespace | Why not checked |
|---|---|---|
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/agentgateway-enterprise:2026.11.1` (agentgateway-proxy, bedrock-proxy) | agentgateway-system | Private Solo.io image; no published OSV module/package; upstream is Rust but tag is not a crates.io release |
| `.../enterprise-agentgateway/enterprise-agentgateway-controller:2026.11.1` | agentgateway-system | Private image, unmappable |
| `.../enterprise-agentgateway/ext-auth-service:2026.11.1`, `rate-limiter:2026.11.1`, `waf-server:2026.11.1` | agentgateway-system | Private images, unmappable |
| `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` (atenet-router, atenet-egress) | ate-system | Digest-pinned dev build; crates.io probe (`agentgateway`) returned no data, tag is not a release version |
| `.../solo-public/solo-enterprise/solo-enterprise-ui-frontend:1.0.0-alpha12` / `:0.5.9`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3` | kagent, agentgateway-system | Private Solo enterprise images, unmappable |
| `.../kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` | kagent | Private image, unmappable |
| `.../substrate-enterprise/*:v0.6.0-b15382d4` — enterprise builds | ate-system | Only the open-source `github.com/kagent-dev/substrate` was queryable (clean); enterprise build contents unverified |
| `clickhouse/clickhouse-server:26.3.35-alpine` and `:26.3.17-alpine` | agentgateway-system, kagent | C++ project; OSV has no usable ClickHouse package match at these tags |
| `rustfs/rustfs:1.0.0-beta.3@sha256:378642b0…` | ate-system | Not indexed in OSV under that name |
| `pgvector/pgvector:pg16` | postgres | PostgreSQL + pgvector extension; no image→package mapping resolved (out of query budget) |
| `127.0.0.1:5000/fortigate-mcp:dev` | kagent | Local, unversioned `dev` tag — cannot identify a version to query |
| `python:3.13-slim` (typesafe-jev-mcp) | kagent | OSV has no upstream Go/Rust module for the CPython slim image; needs a distro/OS scanner (Debian security tracker) |
| gVisor sandbox (`ateworker-gvisor` runs sandboxes) | kagent | Runtime version not exposed in pod/deployment specs; not mapped |
| Talos host OS userland + kernel 6.18.48-talos | node | Requires OS-level SBOM scanner; OSV Go module query covers Talos Go code only |
| `us-docker.pkg.dev/solo-public/substrate-enterprise/kubernetes-secrets:v0.6.0-b15382d4` | ate-system | Private image; checked only via the open-source substrate module |

## 4. Recommended actions (in priority order)

1. **Bump Argo CD v3.5.3 → v3.5.4** — CRITICAL AppProject restriction bypass + HIGH repo-server command injection; both fixed in 3.5.4. Single-image change across `argocd-applicationset-controller`, `argocd-server`, `argocd-repo-server`, `argocd-notifications-controller`, `argocd-application-controller`.
2. **Bump flannel v0.27.2 → ≥ v0.28.2** (or the Sidero build that carries it) — HIGH cross-node RCE in a privileged node DaemonSet.
3. **Update Talos to a build carrying containerd ≥ 2.2.10** — covers all four containerd findings (the HIGH digest-crash one needs 2.2.10 specifically) and will also pick up the CVE-2026-77564 support-bundle fix once at 1.14.x.
4. **Bump CoreDNS v1.12.3 → ≥ 1.14.7** — clears all 12 advisories. (Minimum interim: 1.12.4 for CVE-2025-58063.)
5. **Bump `argocd-redis` 8.2.3-alpine → ≥ 8.2.6-alpine** — five HIGH RCE-class advisories.
6. **Replace the floating `nginx:1.27-alpine` tag on `kagent-ui-tls` with a pinned ≥ 1.30.6-alpine digest** — 3 CRITICAL + 9 HIGH; the floating tag also means the running patch level is not auditable.
7. **Close the version skew**: `kube-proxy` is v1.34.11 while the control plane is v1.36.4 — align it with the control-plane version.
8. **Confirm, don't patch blindly**: External Secrets v2.9.0 (advisory closes at v2.4.0) and Dex v2.45.1 (closes at v2.38.0) are returned by OSV only because of imprecise/unreviewed Go auto-mappings — both appear **not** affected.
9. The two Kubernetes findings (CVE-2025-1767, CVE-2024-7598) carry **no fixed version in OSV** — cross-check the Kubernetes security-announce threads referenced in the advisories before treating v1.36.4 as patched.

## 5. Query log (auditability)

Ecosystem / package → version queried: `k8s.io/kubernetes`(Go) 1.36.4 [Go + unscoped], 1.34.11 · `github.com/argoproj/argo-cd/v3`(Go) 3.5.3 · `github.com/cert-manager/cert-manager`(Go) 1.21.2 · `github.com/external-secrets/external-secrets`(Go) 2.9.0 · `github.com/coredns/coredns`(Go) 1.12.3, 1.31.11 · `github.com/siderolabs/talos`(Go) 1.13.10 · `github.com/containerd/containerd/v2`(Go) 2.2.7 · `github.com/flannel-io/flannel`(Go) 0.27.2 · `github.com/dexidp/dex`(Go) 2.45.1 · `github.com/kagent-dev/kagent`(Go) 0.3.0 [+ unscoped] · `github.com/kagent-dev/substrate`(Go) 0.6.0 [+ unscoped] · `github.com/open-telemetry/opentelemetry-collector-contrib`(Go) 0.161.0 · `redis`(Redis) 8.2.3 [8.6.8 rejected HTTP 400] · `nginx`(unscoped) 1.27.4 · `nginx`(Alpine:v3.21) 1.27.4-r0 [no match] · `agentgateway`(crates.io) 1.0.0 · `nginx`(nginx ecosystem) rejected HTTP 400.
