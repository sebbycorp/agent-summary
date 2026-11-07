# k8s-cve - 2026-11-07

**Verdict: ACTION NEEDED** — 1 CRITICAL and several HIGH findings on in-use components (Argo CD 3.5.3, CoreDNS 1.12.3, Flannel 0.27.2), all with fixed versions available; no exploited/unfixed criticals on the control plane itself.

## Cluster

| Item | Value |
|---|---|
| Cluster | omni-viper (node `talos-cyv-ekb`, single control-plane) |
| Kubernetes | **v1.36.4** |
| Node OS | Talos **v1.13.10**, kernel 6.18.48-talos (amd64) |
| Container runtime | **containerd 2.2.7** |
| kube-proxy image | `registry.k8s.io/kube-proxy:v1.34.11` (version skew vs. 1.36.4) |
| CNI | `ghcr.io/siderolabs/flannel:v0.27.2` |
| DNS | `registry.k8s.io/coredns/coredns:v1.12.3` |

OSV.dev (`https://api.osv.dev/v1/query`) was reachable. The 25-query budget was fully consumed (21 distinct package/version lookups plus re-runs used to validate results); every row below is copied from an OSV response in this session.

## Findings

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Argo CD (`quay.io/argoproj/argocd`) | 3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp / GO-2026-6784 — AppProject destination-namespace & resource restrictions bypassed by PreDelete/PostDelete hooks | **CRITICAL** (CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H) | **3.5.4** (also 3.3.15, 3.4.10, 3.6.0-rc2) |
| Argo CD (`quay.io/argoproj/argocd`) | 3.5.3 | CVE-2026-55797 / GHSA-j6cw-g6p4-7hch / GO-2026-6701 — repo-server command injection via crafted SSH repo SOCKS5 proxy URL | **HIGH** (CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H) | **3.5.4** (also 3.3.15, 3.4.10, 3.6.0-rc2) |
| Flannel (`ghcr.io/siderolabs/flannel`) | 0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2 / GO-2026-4894 — cross-node RCE via extension backend BackendData injection | **HIGH** | **0.28.2** |
| CoreDNS (`registry.k8s.io/coredns/coredns`) | 1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9 / GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | **HIGH** | 1.12.4 |
| CoreDNS | 1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr / GO-2026-4630 — ACL bypass | **HIGH** | 1.14.2 |
| CoreDNS | 1.12.3 | CVE-2026-26018 / GHSA-h75p-j8xm-m278 / GO-2026-4635 — loop-detection DoS | **HIGH** | 1.14.2 |
| CoreDNS | 1.12.3 | CVE-2026-32934 / GHSA-2wpx-qpw2-g5h5 / GO-2026-4969 — DoQ worker pool does not bound stream backlog | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-32936 / GHSA-63cw-r7xf-jmwr / GO-2026-5164 — DoH GET oversized `dns=` param CPU/memory amplification | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh / GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-33489 / GHSA-h8mm-c463-wjq3 / GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-35579 / GHSA-vp29-5652-4fw9 / GO-2026-5667 — TSIG auth bypass on gRPC and QUIC | **HIGH** | 1.14.3 |
| CoreDNS | 1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29 / GO-2026-6507 — unauthenticated memory exhaustion in custom transports | **HIGH** | 1.14.7 |
| CoreDNS | 1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx / GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | **HIGH** | 1.14.7 |
| CoreDNS | 1.12.3 | CVE-2025-68151 / GHSA-527x-5wrf-22m2 / GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | 1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv / GO-2026-6805 — `k8s_external` headless AXFR empty batch panics `transfer` plugin | LOW | 1.14.5 |
| containerd (node runtime) | 2.2.7 | CVE-2026-53495 / GHSA-7jxh-36q5-gcqv / GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | MODERATE | **2.2.8** |
| containerd (node runtime) | 2.2.7 | CVE-2026-95838 / GHSA-rp3h-jf77-q9p4 / GO-2026-6796 — unpack DoS via repeated opaque whiteouts | MODERATE | **2.2.8** |
| containerd (node runtime) | 2.2.7 | CVE-2026-53493 / GHSA-pg57-6jwg-q645 / GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | MODERATE | **2.2.9** |
| Talos Linux (node OS) | 1.13.10 | CVE-2026-77564 / GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | LOW | post-1.14.0-alpha commit (76080416beee) — update Talos |
| Kubernetes / kube-proxy | 1.36.4 / 1.34.11 | GO-2025-3521 (CVE-2025-1767) — GitRepo volume inadvertent local repository access | not given by OSV | no fixed version in OSV |
| Kubernetes / kube-apiserver | 1.36.4 / 1.34.11 | GO-2025-3547 (CVE-2024-7598) — kube-apiserver race condition | not given by OSV | no fixed version in OSV |
| cert-manager (controller/cainjector/webhook) | 1.21.2 | none reported by OSV for `github.com/cert-manager/cert-manager` 1.21.2 | – | – |
| external-secrets | 2.9.0 | GO-2026-5723 (CVE-2026-42875) listed, but its range is "before v2.4.0" → **v2.9.0 not actually affected** (scanner false positive from the `introduced:0` Go range) | not given by OSV | 2.4.0 |
| dex (`ghcr.io/dexidp/dex`) | 2.45.1 | GO-2024-2476 (CVE-2024-23656) listed, but its range is 2.37.0 → <2.38.0 → **v2.45.1 not actually affected** (false positive) | not given by OSV | 2.38.0 |
| kagent tools (`ghcr.io/kagent-dev/kagent/tools`) | 0.3.0 | none reported by OSV for `github.com/kagent-dev/kagent` 0.3.0 | – | – |
| Substrate / ATE (ateapi, atecontroller, ate-idp-server, atelet, ateworker-gvisor) | 0.6.0-b15382d4 | none reported by OSV for `github.com/kagent-dev/substrate` 0.6.0 | – | – |
| otel opentelemetry-collector-contrib | 0.161.0 | none reported by OSV for `github.com/open-telemetry/opentelemetry-collector-contrib` 0.161.0 | – | – |

## Recommended actions (priority order)

1. **Argo CD 3.5.3 → 3.5.4** (argocd namespace: applicationset-controller, notifications-controller, repo-server, server). CRITICAL policy-bypass + HIGH repo-server RCE, single patch release.
2. **Flannel 0.27.2 → ≥0.28.2** (`ghcr.io/siderolabs/flannel`) — HIGH cross-node RCE on the CNI; on Talos this is a CNI asset bump.
3. **CoreDNS 1.12.3 → 1.14.7** — 12 advisories (11 HIGH+); DNS is cluster-critical and reachable from all pod networks.
4. **containerd 2.2.7 → ≥2.2.9** via a Talos patch update (also picks up the Talos credential-redaction fix).
5. Investigate the **kube-proxy v1.34.11 vs. apiserver v1.36.4 skew** (two versions behind; outside supported skew).
6. Ignore/close the two OSV **false positives**: external-secrets v2.9.0 (CVE-2026-42875 fixed in 2.4.0) and dex v2.45.1 (CVE-2024-23656 fixed in 2.38.0) — both flagged only because OSV's Go entry carries an `introduced: 0` range.
7. Note: the two Kubernetes entries (CVE-2025-1767, CVE-2024-7598) are published with `introduced`-only ranges and no fixed version in OSV; they are known long-standing advisories, not new 1.36 regressions — verify against the k8s 1.36 changelog rather than treating as urgent.

## Not checked (could not map image → OSV package)

- `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` — private Solo Enterprise image, no public module mapping.
- `us-docker.pkg.dev/solo-public/solo-enterprise/solo-enterprise-autoauth:v0.2.2` and `:v0.2.3` — private.
- `us-docker.pkg.dev/solo-public/solo-enterprise/solo-enterprise-ui-frontend:1.0.0-alpha12` / `:0.5.9`, `solo-enterprise-ui-backend:0.5.9` — private, frontend is a web bundle (no Go module).
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/agentgateway-enterprise:2026.10.2` (agentgateway-proxy, bedrock-proxy) — enterprise build, no public version mapping.
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/enterprise-agentgateway-controller:2026.10.2`, `ext-auth-service:2026.10.2`, `rate-limiter:2026.10.2`, `waf-server:2026.10.2` — private.
- `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` (atenet-egress, atenet-router) — commit/digest-pinned, upstream version not determinable. (OSV does carry an `agentgateway` advisory, CVE-2026-29791 / GHSA-v2x6-wwfw-r2rq, fixed 0.12.0, but it **cannot be attributed** to this untagged build — treat as unknown, not as a confirmed finding.)
- `us-docker.pkg.dev/solo-public/substrate-enterprise/kubernetes-secrets:v0.6.0-b15382d4` (k8s-credential-provider) — private component name, unmapped.
- `rustfs/rustfs:1.0.0-beta.3@sha256:378642b0…` — Rust binary image, no matching OSV package found.
- `clickhouse/clickhouse-server:26.3.17-alpine` — C++ server, no Go/OSV package mapping attempted.
- `nginx:1.27-alpine` (kagent-ui-tls) — needs an OS (Alpine) advisory source, not OSV Go/PyPI ecosystems.
- `us-docker.pkg.dev/solo-public/substrate-enterprise/ateworker-gvisor:v0.6.0-b15382d4` — also embeds **gVisor**, whose runtime version is not exposed on the image tag.
- `127.0.0.1:5000/fortigate-mcp:dev`, `python:3.13-slim` (typesafe-jev-mcp) — mutable `:dev` tag / base-image OS layer; not mappable.
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/redis:8.6.7-alpine`, `public.ecr.aws/docker/library/redis:8.2.3-alpine` (argocd-redis) — OS/Alpine ecosystem advisories, outside this run's scope.

*Generated read-only from the live cluster via kubectl-equivalent tooling; no cluster mutations performed. Source: api.osv.dev, queried 2026-11-07 (America/Toronto).*
