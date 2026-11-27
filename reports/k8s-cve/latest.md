# k8s-cve - 2026-11-27

**Verdict: ACTION NEEDED** — Argo CD v3.5.3 is affected by a Critical (CVSS 3.1, scope-changing) project-restriction bypass and a High repo-server command injection (both fixed in v3.5.4); CoreDNS v1.12.3 matches 24 OSV records (multiple High-severity auth bypass/DoS, latest fix 1.14.7); Flannel v0.27.2 has a High cross-node RCE (fixed 0.28.2); the node container runtime containerd 2.2.7 has 4 unfixed-by-us image-handling DoS advisories (fixed across 2.2.8–2.2.10).

- Cluster: `omni-viper` — Kubernetes **v1.36.4**, single node `talos-cyv-ekb`, OS **Talos v1.13.10**, kernel 6.18.48-talos, runtime **containerd 2.2.7**
- Data source: OSV.dev (`POST https://api.osv.dev/v1/query`), reachable, 23 queries this run
- Every CVE/advisory ID below was returned by an OSV response in this session. Rows where OSV also returned a version range that excludes our running version are marked *likely false positive* rather than dropped, for auditability.

## Inventory (deduplicated, key namespaces)

| Namespace | Image (tag) | Mapped to OSV as |
|---|---|---|
| cluster-wide | Kubernetes `v1.36.4` | Go `k8s.io/kubernetes@1.36.4` |
| kube-system | `registry.k8s.io/kube-proxy:v1.34.11` | Go `k8s.io/kubernetes@1.34.11` |
| kube-system | `registry.k8s.io/coredns/coredns:v1.12.3` | Go `github.com/coredns/coredns@v1.12.3` |
| kube-system | `ghcr.io/siderolabs/flannel:v0.27.2` | Go `github.com/flannel-io/flannel@v0.27.2` |
| node | containerd `2.2.7` / Talos `v1.13.10` | Go `containerd/v2`, `github.com/siderolabs/talos` |
| argocd | `quay.io/argoproj/argocd:v3.5.3` | Go `github.com/argoproj/argo-cd/v3@v3.5.3` |
| argocd | `ghcr.io/dexidp/dex:v2.45.1` | Go `github.com/dexidp/dex@v2.45.1` |
| cert-manager | `quay.io/jetstack/cert-manager-{controller,cainjector,webhook}:v1.21.2` | Go `github.com/cert-manager/cert-manager@v1.21.2` |
| external-secrets | `ghcr.io/external-secrets/external-secrets:v2.9.0` | Go `github.com/external-secrets/external-secrets@v2.9.0` |
| agentgateway-system, ate-system | `cr.agentgateway.dev/agentgateway:v1.7.0` | Go `github.com/agentgateway/agentgateway@1.7.0` |
| kagent | `ghcr.io/kagent-dev/kagent/tools:0.3.1` | Go `github.com/kagent-dev/kagent@0.3.1` |

## CVE table

| Component | Version | CVE / advisory | Severity | Fixed version |
|---|---|---|---|---|
| **Argo CD** (argo-cd, v3.5.3 image) | v3.5.3 | CVE-2026-77459 / GHSA-fmxq-cgp8-87wp, GO-2026-6784 — AppProject destination-namespace & resource restrictions bypass | **Critical** (CVSS3.1 AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H) | **3.5.4** (also 3.4.10 / 3.3.15) |
| **Argo CD** (repo-server) | v3.5.3 | CVE-2026-55797, CVE-2026-113708, CVE-2026-113709 / GHSA-j6cw-g6p4-7hch, GO-2026-6701 — repo-server command injection via crafted SSH repo SOCKS5 proxy | **High** (CVSS3.1 AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H) | **3.5.4** (also 3.4.10 / 3.3.15) |
| **CoreDNS** | v1.12.3 | CVE-2026-26017 / GHSA-c9v3-4pv7-87pr, GO-2026-4630 — ACL bypass | **High** (AV:N/AC:L/PR:L/UI:N/S:C/C:H) | **1.14.2** |
| CoreDNS | v1.12.3 | CVE-2026-33190 / GHSA-qhmp-q7xh-99rh, GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | **High** (AV:N/AC:L/PR:N/UI:N/C:H) | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 / GHSA-vp29-5652-4fw9, GO-2026-5667 — TSIG auth bypass on gRPC/QUIC | **High** (C:H) | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 / GHSA-h8mm-c463-wjq3, GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | **High** (C:H) | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-86003 / GHSA-9gm5-9rfh-m6vx, GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection | **High** (I:H) | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-82399 / GHSA-mrg3-qvqr-jw29, GO-2026-6507 — unauthenticated memory exhaustion | **High** (A:H) | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2025-68151 / GHSA-527x-5wrf-22m2, GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | High (CVSS4.0, availability H, E:U) | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-26018 / GHSA-h75p-j8xm-m278, GO-2026-4635 — loop detection DoS | **High** (A:H) | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2025-47950, CVE-2026-32934 / GHSA-2wpx-qpw2-g5h5, GO-2026-4969 — DoQ worker pool unbounded stream backlog | **High** (A:H) | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 / GHSA-63cw-r7xf-jmwr, GO-2026-5164 — DoH GET oversized `dns=` param CPU/mem | **High** (A:H) | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2025-58063 / GHSA-93mf-426m-g6x9, GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | **High** (I:L/A:H, needs `etcd` plugin) | **1.12.4** |
| CoreDNS | v1.12.3 | CVE-2026-62994 / GHSA-74w3-63xv-x9mv, GO-2026-6805 — `k8s_external` headless AXFR panic | Low (needs `k8s_external`) | 1.14.5 |
| **Flannel** (kube-flannel) | v0.27.2 | CVE-2026-32241 / GHSA-vchx-5pr6-ffx2, GO-2026-4894 — cross-node RCE via extension backend BackendData | **High** (AV:N/AC:H/PR:L/UI:N/C:H/I:H/A:H) | **0.28.2** |
| **containerd** (node runtime) | 2.2.7 | CVE-2026-53495 / GHSA-7jxh-36q5-gcqv, GO-2026-6444 — CRI ExecSync goroutine leak → node-level DoS | High (CVSS4.0, local, A:H) | **2.2.8** |
| containerd | 2.2.7 | CVE-2026-95838 / GHSA-rp3h-jf77-q9p4, GO-2026-6796 — unpack DoS via repeated opaque whiteouts | Medium/High (CVSS4.0, local, A:L) | 2.2.8 |
| containerd | 2.2.7 | CVE-2026-53493 / GHSA-pg57-6jwg-q645, GO-2026-6597 — image-pull DoS via crafted OCI index graph amplification | Medium (CVSS4.0, **network**, A:L) | 2.2.9 |
| containerd | 2.2.7 | CVE-2026-95839 / GHSA-vwhp-8p66-mq4f, GO-2026-7007 — daemon crash via malformed digest in subject-anchor | High (CVSS4.0, network, A:H) | **2.2.10** |
| **Talos Linux** (node OS) | v1.13.10 | CVE-2026-77564 / GHSA-7cxh-94cf-mfp6 — support bundle does not redact registry credentials | Low (AV:L/UI:R/C:L) | 1.14.0-alpha.0.0.20260513110206-76080416beee (no stable fix tag in OSV) |
| **Kubernetes** (apiserver/control plane) | v1.36.4 | CVE-2025-1767 / GO-2025-3521 (GHSA-3wgm-2gw2-vh5m) — GitRepo volume inadvertent local repository access | Not given in OSV (Go DB, no severity) | none published in OSV record (returned as affected at 1.36.4) |
| **Kubernetes** (kube-apiserver) | v1.36.4 | CVE-2024-7598 / GO-2025-3547 (GHSA-r56h-j38w-hrqq) — race condition in kube-apiserver | Not given in OSV | none published in OSV record (introduced 1.3.0, no fix event) |
| Kubernetes (kube-proxy image) | v1.34.11 | Same two records: CVE-2025-1767 (GO-2025-3521), CVE-2024-7598 (GO-2025-3547) | Not given in OSV | none published — plus a **version skew**: kube-proxy 1.34.11 vs control plane 1.36.4 |
| External Secrets Operator | v2.9.0 | CVE-2026-42875 / GO-2026-5723 (GHSA-wv26-88m5-6h59) — namespace isolation bypass in CAProvider ConfigMap resolution | Not given in OSV | 2.4.0 per advisory range → **likely FALSE POSITIVE at v2.9.0** |
| Dex (Argo CD sidecar) | v2.45.1 | CVE-2024-23656 / GO-2024-2476 — TLS config discarded, serves TLS 1.0/1.1 + weak ciphers | Not given in OSV | 2.38.0 per advisory range → **likely FALSE POSITIVE at v2.45.1** |

Clean in OSV (queried, zero results): **cert-manager v1.21.2** (controller/cainjector/webhook — queried twice, confirmed empty), **agentgateway v1.7.0** (Go module and Alpine distro-level queries both empty), **kagent tools 0.3.1**.

## Recommended actions (priority order)

1. **Bump Argo CD v3.5.3 → v3.5.4.** Highest-priority item in the cluster: a Critical project-restriction bypass plus a High repo-server command injection are both closed by the patch release in the same minor line.
2. **Bump CoreDNS v1.12.3 → v1.14.7** (covers all 24 matched records). Risk-reducing caveat: several records only apply if the corresponding plugin/transport is enabled (`etcd`, `k8s_external`, DoH/DoQ/gRPC servers) — verify the Corefile before treating the auth-bypass/DoS rows as live exposure. Minimum if a jump to 1.14.x is undesirable: 1.12.4 closes CVE-2025-58063.
3. **Bump Flannel v0.27.2 → ≥0.28.2** (Siderolabs image) for CVE-2026-32241. Confirm whether the `extension` backend is actually in use; the advisory is conditioned on that backend handling `BackendData`.
4. **Patch containerd to ≥2.2.10 via a Talos update** — this is a node-OS-managed component, so it is fixed by a Talos patch release, not by editing a manifest. Single-node cluster, so schedule the reboot.
5. **Plan the Talos v1.13.10 → v1.14.x upgrade** for CVE-2026-77564; meanwhile avoid shipping `support-bundle` output externally, since it can contain registry credentials.
6. **Resolve the kube-proxy version skew** (image `v1.34.11` against a `v1.36.4` control plane). Both Kubernetes records above have no fix version published in OSV, so treat them as accepted risk unless/until upstream states otherwise; the skew itself is the actionable finding.
7. Verify the two likely false positives (External Secrets v2.9.0 vs fix 2.4.0; Dex v2.45.1 vs fix 2.38.0) — both appear to be Go-vulnDB `introduced:0`-only mapping artifacts, and no action is expected.

## Not checked (could not map to an OSV package/ecosystem entry)

Solo.io enterprise / substrate images (proprietary build tags, no public Go module equivalent; OSV queries would be guesswork):

- `us-docker.pkg.dev/solo-public/substrate-enterprise/`: `ateapi`, `atecontroller`, `ate-idp-server`, `atelet`, `kubernetes-secrets`, `ateworker-gvisor` — all `v1.1.0-alpha1-7354c972` (ate-system, kagent worker pools)
- `us-docker.pkg.dev/solo-public/enterprise-agentgateway/`: `agentgateway-enterprise:2026.11.1-patch.0` (agentgateway-proxy, bedrock-proxy), `enterprise-agentgateway-controller:2026.11.1-patch.0`, `ext-auth-service`, `rate-limiter`, `waf-server` (all `2026.11.1-patch.0`), `redis:8.6.8-alpine`
- `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:2026.11.2`
- `us-docker.pkg.dev/solo-public/solo-enterprise/`: `solo-enterprise-autoauth:v0.2.2`, `solo-enterprise-autoauth:v0.2.4`, `solo-enterprise-ui-frontend:2026.11.2` and `:0.5.18`, `solo-enterprise-ui-backend:0.5.18`

Other images with no reliable OSV mapping this run:

- `docker.io/otel/opentelemetry-collector-contrib:0.163.2` and `:0.161.0` — the Go core module (`go.opentelemetry.io/collector`) and contrib module queries both returned an empty result set (`{}`), which is *no data available*, **not** a clean bill of health
- `nginx:1.27-alpine` (kagent-ui-tls) — Alpine distro query for `nginx` 1.27.x returned empty; tag is not pinned to an exact patch/build
- `redis:8.2.3-alpine` (argocd-redis), `redis:8.6.8-alpine` (mirrored) — distro-level query not attempted within budget
- `clickhouse/clickhouse-server:26.8.36-alpine`, `rustfs/rustfs:1.0.3`
- `ghcr.io/agentevals-dev/agentevals:0.15.1`
- `python:3.13-slim` (typesafe-jev-mcp) — floating `slim` tag, no pinned patch version to query
- `127.0.0.1:5000/fortigate-mcp:dev` — cluster-local build on a `dev` tag, no version to map

Also out of scope this run: non-key namespaces (default, postgres, registry, tailscale, remarkable-diary, local-path-storage, podcertificate-controller-system) beyond what was captured incidentally; and OS/base-layer CVEs for the Solo.io and ClickHouse/rustfs/Redis images, which need an image-scan tool rather than OSV source-module lookups.

---
Report generated read-only by the `k8s_cve_checker` agent. No cluster resources were modified; no Secret data was read or printed. OSV.dev was reachable for all 23 queries.
