# k8s-cve - 2026-11-09

**Verdict: ACTION NEEDED** — CoreDNS 1.12.3 (12 unpatched CVEs, latest fix 1.14.7), Argo CD 3.5.3 (2 CVEs, fix 3.5.4), flannel 0.27.2 (cross-node RCE, fix 0.28.2) and containerd 2.2.7 (3 CVEs, fix 2.2.8/2.2.9) all need upgrades; cert-manager, external-secrets and the OTel collector core came back clean.

- Cluster: `omni-viper` — single control-plane node `talos-cyv-ekb`, Kubernetes **v1.36.4**, Talos **v1.13.10**, containerd **2.2.7**, kernel 6.18.48-talos, flannel CNI.
- Namespaces inventoried: `kagent`, `ate-system`, `agentgateway-system`, `argocd`, `external-secrets`, `cert-manager`, `kube-system`.
- Source: OSV.dev `/v1/query` — 23 queries used of the 25 budget. Every row below comes from an OSV response captured in this session; nothing is inferred from memory.
- Date: 2026-11-09 (America/Toronto). Read-only run; no cluster changes made.

## Findings

### 1. Control plane / node

| Component | Version | CVE / Advisory | Severity (from OSV) | Fixed version |
|---|---|---|---|---|
| Kubernetes (kube-apiserver et al.) | v1.36.4 | CVE-2025-1767 · GO-2025-3521 · GHSA-3wgm-2gw2-vh5m — GitRepo volume inadvertent local repository access | Not rated by OSV | None listed (OSV range `introduced:0`, unbounded) |
| Kubernetes (kube-apiserver) | v1.36.4 | CVE-2024-7598 · GO-2025-3547 · GHSA-r56h-j38w-hrqq — race condition in kube-apiserver | Not rated by OSV | None listed (OSV range `introduced:1.3.0`, unbounded) |
| kube-proxy | **v1.34.11** | Same two advisories apply to the kube-proxy image (registry.k8s.io/kube-proxy:v1.34.11) | Not rated by OSV | None listed |
| Talos Linux | v1.13.10 | CVE-2026-77564 · GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | Low — `CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:L/I:N/A:N` | `1.14.0-alpha.0.0.20260513110206-76080416beee` (no stable tag listed) |
| containerd | 2.2.7 | CVE-2026-53495 · GHSA-7jxh-36q5-gcqv · GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | High — `CVSS:4.0/…/VA:H` | **2.2.8** (1.7.35 / 2.0.12 / 2.3.5) |
| containerd | 2.2.7 | CVE-2026-53493 · GHSA-pg57-6jwg-q645 · GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | Medium — `CVSS:4.0/AV:N/…/VA:L` | **2.2.9** (1.7.36 / 2.0.13 / 2.3.6 / 2.4.1) |
| containerd | 2.2.7 | CVE-2026-95838 · GHSA-rp3h-jf77-q9p4 · GO-2026-6796 — unpack DoS via repeated opaque whiteouts | Medium — `CVSS:4.0/AV:L/…/UI:A/VA:L` | **2.2.8** |
| flannel (ghcr.io/siderolabs/flannel) | v0.27.2 | CVE-2026-32241 · GHSA-vchx-5pr6-ffx2 · GO-2026-4894 — cross-node RCE via extension-backend `BackendData` injection | High — `CVSS:3.1/AV:N/AC:H/PR:L/UI:N/S:U/C:H/I:H/A:H` | **0.28.2** |
| CoreDNS (registry.k8s.io/coredns/coredns) | **v1.12.3** | 12 unique CVEs — see section 3 | up to High | **1.14.7** |

> Note (not a CVE, but worth raising): the `kube-proxy` image is pinned at **v1.34.11** while the apiserver/scheduler/controller-manager are **v1.36.4** — a 2-minor-version skew well outside the supported skew window.

### 2. Platform control planes

| Component | Version | CVE / Advisory | Severity (from OSV) | Fixed version |
|---|---|---|---|---|
| Argo CD (quay.io/argoproj/argocd) | v3.5.3 | CVE-2026-77459 · GHSA-fmxq-cgp8-87wp · GO-2026-6784 — AppProject destination-namespace/resource restrictions bypassed by PreDelete/PostDelete hooks | Critical — `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H` | **3.5.4** (3.3.15 / 3.4.10 / 3.6.0-rc2) |
| Argo CD repo-server | v3.5.3 | CVE-2026-55797 · GHSA-j6cw-g6p4-7hch · GO-2026-6701 — command injection via crafted SSH repository SOCKS5 proxy URL | High — `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H` | **3.5.4** |
| cert-manager (controller / cainjector / webhook) | v1.21.2 | No OSV results for `github.com/cert-manager/cert-manager@1.21.2` | — | — |
| external-secrets | v2.9.0 | CVE-2026-42875 · GO-2026-5723 · GHSA-wv26-88m5-6h59 — namespace-isolation bypass in CAProvider ConfigMap resolution for SecretStore. **Returned by OSV but likely NOT applicable**: the advisory's own ecosystem range is fixed at **2.4.0**, below the deployed 2.9.0 (OSV matched on an unbounded SemVer range) | Not rated by OSV | 2.4.0 (per advisory) |
| dex (argocd-dex-server) | v2.45.1 | CVE-2024-23656 · GO-2024-2476 · GHSA-gr79-9v6v-gc9r — discards TLS config, serves TLS 1.0/1.1 + weak ciphers. **Returned by OSV but likely NOT applicable**: fixed in **2.38.0**, deployed is 2.45.1 | Not rated by OSV | 2.38.0 (per advisory) |
| OTel collector core (`go.opentelemetry.io/collector`) | 0.161.0 | No OSV results (also checked `…/collector/contrib`: none) | — | — |
| agentgateway (ghcr.io/kagent-dev/substrate/agentgateway) | `cd65181fd52e@sha256:ebebf5a7…` | CVE-2026-29791 · GHSA-v2x6-wwfw-r2rq — missing parameter sanitization in MCP→OpenAPI conversion (**unconfirmed** — image is commit-pinned and OSV could not resolve the version, so it returns all unfixed advisories; identical response for a deliberately bogus version) | Medium — `CVSS:3.1/AV:N/AC:H/PR:L/UI:N/S:C/C:L/I:L/A:N` | 0.12.0 (per advisory) |
| kagent tools (ghcr.io/kagent-dev/kagent/tools) | 0.3.0 | No OSV results for `github.com/kagent-dev/kagent@0.3.0` (module path for the `tools` sub-image not confirmed) | — | — |

### 3. CoreDNS v1.12.3 — detail (12 unique CVEs, all unpatched)

| CVE / Advisory | Summary | Severity (OSV CVSS) | Fixed |
|---|---|---|---|
| CVE-2026-82399 · GHSA-mrg3-qvqr-jw29 · GO-2026-6507 | Unauthenticated memory exhaustion in custom transports | High — `3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H` | 1.14.7 |
| CVE-2026-86003 · GHSA-9gm5-9rfh-m6vx · GO-2026-6506 | DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | High — `3.1/…/S:U/C:N/I:H/A:N` | 1.14.7 |
| CVE-2026-62994 · GHSA-74w3-63xv-x9mv · GO-2026-6805 | `k8s_external` headless AXFR empty batch panics `transfer` plugin | Low — `3.1/AV:N/AC:H/…/A:L` | 1.14.5 |
| CVE-2026-33190 · GHSA-qhmp-q7xh-99rh · GO-2026-5583 | TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | High — `3.1/…/S:U/C:H/I:N/A:N` | 1.14.3 |
| CVE-2026-35579 · GHSA-vp29-5652-4fw9 · GO-2026-5667 | TSIG auth bypass on gRPC + QUIC | High — `3.1/…/C:H` | 1.14.3 |
| CVE-2026-33489 · GHSA-h8mm-c463-wjq3 · GO-2026-5417 | Transfer stanza selection uses lexicographic compare (subzone ACL bypass) | High — `3.1/…/C:H` | 1.14.3 |
| CVE-2026-32936 · GHSA-63cw-r7xf-jmwr · GO-2026-5164 | DoH GET oversized `dns=` param CPU/memory amplification | High — `3.1/…/A:H` | 1.14.3 |
| CVE-2026-32934 · GHSA-2wpx-qpw2-g5h5 · GO-2026-4969 | DoQ worker pool does not bound stream backlog | High — `3.1/…/A:H` | 1.14.3 |
| CVE-2026-26017 · GHSA-c9v3-4pv7-87pr · GO-2026-4630 | ACL bypass | Critical/High — `3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:N/A:N` | 1.14.2 |
| CVE-2026-26018 · GHSA-h75p-j8xm-m278 · GO-2026-4635 | Loop-detection DoS | High — `3.1/…/A:H` | 1.14.2 |
| CVE-2025-68151 · GHSA-527x-5wrf-22m2 · GO-2026-4289 | gRPC/HTTPS/HTTP3 servers lack resource limits → DoS | High — `CVSS:4.0/…/VA:H/E:U` | 1.14.0 |
| CVE-2025-58063 · GHSA-93mf-426m-g6x9 · GO-2025-3942 | DNS cache pinning via etcd lease ID confusion | High — `3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:L/A:H` (introduced 1.2.0) | **1.12.4** (nearest patch) |

Mitigating context: CoreDNS here is cluster-internal (`10.244.0.0/24`, ClusterIP, `readOnlyRootFilesystem`, all capabilities dropped except `NET_BIND_SERVICE`) and the cluster is single-node — so the network-exposed DoS/TSIG items are lower exploitable risk than the vectors imply, but the ACL/TSIG-bypass items still matter once any workload can reach :53.

## Recommended actions (priority order)

1. **Argo CD → v3.5.4** (CVE-2026-77459 hook bypass + CVE-2026-55797 repo-server RCE; both are authz-boundary escapes in a component that holds cluster-wide write power).
2. **CoreDNS → v1.14.7** via the Talos/kubeadm CoreDNS image override (`registry.k8s.io/coredns/coredns:v1.14.7`); 1.12.3 is 12 CVEs behind.
3. **Talos → a release carrying containerd ≥ 2.2.9** (fixes all three containerd CVEs) and flannel ≥ 0.28.2 (`ghcr.io/siderolabs/flannel:v0.28.2`).
4. **Re-align kube-proxy v1.34.11 with the v1.36.4 control plane** (skew fix, not a CVE).
5. Triaged as no-action: cert-manager v1.21.2 (clean), external-secrets v2.9.0 and dex v2.45.1 (OSV hits whose own fixed versions 2.4.0 / 2.38.0 predate what is deployed), OTel collector core 0.161.0 (clean), kagent 0.3.0 (no OSV data), Talos CVE-2026-77564 (Low, local, support-bundle only — restrict `support bundle` usage).
6. Ask the vendor for SBOMs / a CVE report for the private `solo-public/*` enterprise images listed below — they are the largest blind spot in this scan.

## Not checked (could not map to an OSV package, or skipped to stay in query budget)

Private/unmapped vendor images — no OSV-detectable upstream package:
- `substrate-enterprise/ateapi:v0.6.0-b15382d4`, `atecontroller:v0.6.0-b15382d4`, `ate-idp-server:v0.6.0-b15382d4`, `ateworker-gvisor:v0.6.0-b15382d4`, `kubernetes-secrets:v0.6.0-b15382d4` (a `github.com/agentgateway/substrate@0.6.0` probe returned nothing)
- `kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12`
- `solo-enterprise/solo-enterprise-ui-frontend:1.0.0-alpha12` and `:0.5.9`, `solo-enterprise-ui-backend:0.5.9`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3`
- `enterprise-agentgateway/agentgateway-enterprise:2026.10.2`, `enterprise-agentgateway-controller:2026.10.2`, `ext-auth-service:2026.10.2`, `rate-limiter:2026.10.2`, `waf-server:2026.10.2`
- `127.0.0.1:5000/fortigate-mcp:dev` (internal dev tag, no upstream)

Base-OS / language images needing distro-release mapping (deferred — would exceed the OSV budget):
- `nginx:1.27-alpine` (kagent-ui-tls) — no OSV package for nginx outside a distro ecosystem
- `rustfs/rustfs:1.0.0-beta.3` — crates.io probe returned no data for that version
- `us-docker.pkg.dev/…/enterprise-agentgateway/redis:8.6.7-alpine` and `public.ecr.aws/docker/library/redis:8.2.3-alpine` — would require an Alpine release mapping
- `clickhouse/clickhouse-server:26.3.17-alpine` — not mapped
- `python:3.13-slim` (typesafe-jev-mcp) — needs Debian mapping plus the app's own requirements
- `otel/opentelemetry-collector-contrib:0.161.0` — core module checked and clean, but the several hundred contrib sub-modules (`github.com/open-telemetry/opentelemetry-collector-contrib/…`) were not enumerated individually

Caveat: the agentgateway row is commit-pinned (`cd65181fd52e`) and OSV could not resolve that version, so its single hit is unconfirmed; treat it as "review", not "confirmed vulnerable".
