# k8s-cve - 2026-11-14

**Verdict: ACTION NEEDED** - Argo CD v3.5.3 is affected by a CRITICAL AppProject restriction bypass (fixed in 3.5.4), CoreDNS v1.12.3 and flannel v0.27.2 carry unpatched HIGH-severity CVEs, and the container runtime (containerd 2.2.7 on Talos 1.13.10) is one patch release behind three MODERATE DoS advisories.

- Cluster: **omni-viper** (single-node Talos, node `talos-cyv-ekb`)
- Kubernetes server / kubelet: **v1.36.4** | Talos **v1.13.10** | containerd **2.2.7** | kernel 6.18.48-talos
- Date of check: 2026-11-14 (America/Toronto) | Data source: **OSV.dev** (`https://api.osv.dev/v1/query`), 19 queries
- Read-only check: no cluster resource was created, modified or deleted.

## Findings (every row below comes from an OSV.dev response in this session)

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Kubernetes (apiserver, controller-manager, scheduler) | v1.36.4 | CVE-2025-1767 / GO-2025-3521 (GHSA-3wgm-2gw2-vh5m) - GitRepo volume inadvertent local repository access | not given by OSV | none given (range `introduced: 0`) |
| Kubernetes (kube-apiserver) | v1.36.4 | CVE-2024-7598 / GO-2025-3547 (GHSA-r56h-j38w-hrqq) - kube-apiserver race condition | not given by OSV | none given (range `introduced: 1.3.0`) |
| kube-proxy (`registry.k8s.io/kube-proxy`) | v1.34.11 | CVE-2025-1767 / GO-2025-3521 | not given by OSV | none given |
| kube-proxy (`registry.k8s.io/kube-proxy`) | v1.34.11 | CVE-2024-7598 / GO-2025-3547 | not given by OSV | none given |
| Argo CD (`quay.io/argoproj/argocd`) | v3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp (GO-2026-6784) - AppProject destination-namespace & resource restrictions bypassed via PreDelete/PostDelete hooks | **CRITICAL** (CVSS:3.1/AV:N/.../S:C/C:H/I:H/A:H) | **3.5.4** (also 3.3.15, 3.4.10, 3.6.0-rc2) |
| Argo CD (`quay.io/argoproj/argocd`) | v3.5.3 | CVE-2026-55797 / GHSA-j6cw-g6p4-7hch (GO-2026-6701) - repo-server command injection via crafted SSH repo SOCKS5 proxy URL | **HIGH** (CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H) | **3.5.4** (also 3.3.15, 3.4.10, 3.6.0-rc2) |
| CoreDNS (`registry.k8s.io/coredns/coredns`) | v1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9 (GO-2025-3942) - DNS cache pinning via etcd lease ID confusion | HIGH | 1.12.4 |
| CoreDNS | v1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr (GO-2026-4630) - ACL bypass | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GHSA-h75p-j8xm-m278 (GO-2026-4635) - loop-detection DoS | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GHSA-527x-5wrf-22m2 (GO-2026-4289) - gRPC/HTTPS/HTTP3 servers lack resource limits | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-32934 / GHSA-2wpx-qpw2-g5h5 (GO-2026-4969) - DoQ worker pool unbounded stream backlog | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GHSA-63cw-r7xf-jmwr (GO-2026-5164) - DoH GET oversized `dns=` query amplification | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh (GO-2026-5583) - TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GHSA-h8mm-c463-wjq3 (GO-2026-5417) - transfer stanza lexicographic compare (subzone ACL bypass) | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GHSA-vp29-5652-4fw9 (GO-2026-5667) - TSIG auth bypass on gRPC and QUIC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29 (GO-2026-6507) - unauthenticated memory exhaustion in custom transports | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx (GO-2026-6506) - DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv (GO-2026-6805) - `k8s_external` headless AXFR panics the `transfer` plugin | LOW | 1.14.5 |
| flannel (`ghcr.io/siderolabs/flannel`, DaemonSet) | v0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2 (GO-2026-4894) - cross-node RCE via extension backend BackendData injection | **HIGH** (CVSS:3.1/AV:N/AC:H/PR:L/UI:N/S:U/C:H/I:H/A:H) | **0.28.2** |
| containerd (node runtime) | 2.2.7 | CVE-2026-53495 / GHSA-7jxh-36q5-gcqv (GO-2026-6444) - CRI ExecSync goroutine leak -> node-level DoS | MODERATE | 2.2.8 (also 1.7.35, 2.0.12, 2.3.5) |
| containerd (node runtime) | 2.2.7 | CVE-2026-95838 / GHSA-rp3h-jf77-q9p4 (GO-2026-6796) - unpack DoS via repeated opaque whiteouts | MODERATE | 2.2.8 (also 1.7.35, 2.0.12, 2.3.5) |
| containerd (node runtime) | 2.2.7 | CVE-2026-53493 / GHSA-pg57-6jwg-q645 (GO-2026-6597) - image-pull DoS via crafted OCI index graph amplification | MODERATE | 2.2.9 (also 1.7.36, 2.0.13, 2.3.6, 2.4.1) |
| Talos Linux (node OS) | v1.13.10 | CVE-2026-77564 / GHSA-7cxh-94cf-mfp6 - support bundle does not redact registry credentials | LOW | OSV: `1.14.0-alpha.0.0.20260513110206-76080416beee`; advisory refs v1.12.9 / v1.13.3 |
| External Secrets (`ghcr.io/external-secrets/external-secrets`) | v2.9.0 | CVE-2026-42875 / GHSA-wv26-88m5-6h59 (GO-2026-5723) - namespace isolation bypass in CAProvider ConfigMap resolution | MODERATE | 2.4.0 -> **running 2.9.0 is already newer; likely not applicable** |
| Dex (`ghcr.io/dexidp/dex`, argocd-dex-server) | v2.45.1 | CVE-2024-23656 / GO-2024-2476 (GHSA-gr79-9v6v-gc9r) - Dex discards TLS config, serves TLS 1.0/1.1 + weak ciphers | not given by OSV | 2.38.0 (ecosystem range 2.37.0-2.38.0) -> **running 2.45.1 is newer; likely not applicable** |

## Clean / no OSV records

| Component | Version | OSV result |
|---|---|---|
| cert-manager controller / cainjector / webhook (`quay.io/jetstack/*`) | v1.21.2 | no records (`{}`); module name validated with a control query at 1.12.0, which did return CVE-2024-12401 |
| External Secrets Operator (module `github.com/external-secrets/external-secrets/v2`) | v2.9.0 | no records for the `/v2` module path; the un-versioned module returned only CVE-2026-42875 (see above). Control query at 2.0.0 also showed CVE-2026-34984 (HIGH, fixed 2.2.0) - already fixed in 2.9.0 |
| Tailscale (`ghcr.io/tailscale/tailscale`, homelab-router) | v1.102.4 | no records (`tailscale.com` @ Go) |
| OpenTelemetry Collector contrib (`otel/opentelemetry-collector-contrib`) | 0.161.0 | no records (`github.com/open-telemetry/opentelemetry-collector-contrib` and core `go.opentelemetry.io/collector`) |
| kagent tools (`ghcr.io/kagent-dev/kagent/tools`) | 0.3.0 | no records (`github.com/kagent-dev/kagent` @ Go) |
| rustfs (`rustfs/rustfs`) | 1.0.0-beta.3 | no records for `rustfs` in crates.io ecosystem |

## Not checked (could not be mapped to an OSV package)

Proprietary / closed-source images with no public Go module or distro package identity:

- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/agentgateway-enterprise:2026.10.2` (agentgateway-proxy, bedrock-proxy)
- `.../enterprise-agentgateway/enterprise-agentgateway-controller:2026.10.2`
- `.../enterprise-agentgateway/ext-auth-service:2026.10.2`, `rate-limiter:2026.10.2`, `waf-server:2026.10.2`
- `.../solo-enterprise/solo-enterprise-ui-frontend:0.5.9` / `:1.0.0-alpha12`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3`
- `.../kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` (kagent-controller)
- `.../substrate-enterprise/ateapi:v0.6.0-b15382d4`, `atecontroller:v0.6.0-b15382d4`, `ate-idp-server:v0.6.0-b15382d4`, `atelet:v0.6.0-b15382d4`, `ateworker-gvisor:v0.6.0-b15382d4`, `kubernetes-secrets:v0.6.0-b15382d4`, `podcertcontroller:v0.6.0-b15382d4`
- `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7...` (atenet-egress / atenet-router) - digest-pinned, no upstream release tag to query
- `127.0.0.1:5000/fortigate-mcp:dev` (local build, untagged dev image)

Base-image / OS-layer images that would need a distro-ecosystem scan (Alpine/Debian package DBs are not exposed by api.osv.dev for these tags):

- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/redis:8.6.7-alpine` (ext-cache)
- `public.ecr.aws/docker/library/redis:8.2.3-alpine` (argocd-redis)
- `clickhouse/clickhouse-server:26.3.35-alpine` (agentgateway-system) and `:26.3.17-alpine` (kagent)
- `nginx:1.27-alpine` (kagent-ui-tls)
- `python:3.13-slim` (typesafe-jev-mcp), `python:3.11-slim` (agent-desk)
- `pgvector/pgvector:pg16` (postgres)
- `registry:2` (registry)
- `rancher/local-path-provisioner:v0.0.31` (local-path-storage)
- `python`-based MCP images above are also unpinned by patch version, so no exact Go/PyPI coordinate could be resolved.

## Notes / caveats

- OSV.dev was reachable for all queries; no query failed.
- Severity is taken verbatim from the OSV record (`database_specific.severity` and/or the CVSS vector). "not given by OSV" means the record carries no severity - it is not a judgement of risk.
- The two Kubernetes rows and the Dex / External Secrets rows are OSV Go-database records whose ranges are uncapped (`introduced` with no `fixed` in the SEMVER range); OSV therefore reports the installed version as affected. Where an explicit fixed version is older than what is deployed, it is flagged as likely not applicable rather than silently dropped.
- Version skew observed: the control-plane binaries run v1.36.4 while the `kube-proxy` image is `v1.34.11`; worth reconciling as part of the remediation pass.
- Priority remediation order: 1) Argo CD -> v3.5.4+ (CRITICAL, network-reachable repo-server/AppProject bypass), 2) flannel -> v0.28.2 (HIGH, cross-node RCE), 3) CoreDNS -> 1.14.7+ (many HIGH DoS/auth-bypass), 4) Talos patch carrying containerd 2.2.9+.
