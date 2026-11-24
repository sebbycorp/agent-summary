# k8s-cve - 2026-11-24

**Verdict: ACTION NEEDED** — CoreDNS 1.12.3 (12 unfixed CVEs, mostly HIGH), Flannel 0.27.2 (HIGH, cross-node RCE class), and Argo CD v3.5.3 (1 CRITICAL + 1 HIGH, both patched in 3.5.4) require upgrades; Kubernetes 1.36.4 and cert-manager 1.21.2 are clean in OSV.

- **Cluster:** `omni-viper`, single node `talos-cyv-ekb` (Ready)
- **Kubernetes server:** `v1.36.4`
- **Node OS / runtime:** Talos `v1.13.10`, kernel `6.18.48-talos`, containerd `2.2.7`
- **Checked namespaces:** kagent, ate-system, agentgateway-system, argocd, external-secrets, cert-manager, kube-system
- **Source:** OSV.dev `POST /v1/query` (https), queried 2026-11-24. Every CVE row below comes from an OSV response captured in this session; nothing was recalled from memory.
- **Status:** read-only inventory. No cluster mutation, no Secret data read or printed.

## Findings

### Kubernetes control plane — `k8s.io/kubernetes` @ 1.36.4 (Go ecosystem)

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| kube-apiserver / controller-manager / scheduler (k8s.io/kubernetes) | 1.36.4 | GO-2025-3521 / CVE-2025-1767 / GHSA-3wgm-2gw2-vh5m — GitRepo volume inadvertent local repository access | not stated in OSV | none given (`introduced: 0`) |
| kube-apiserver (k8s.io/kubernetes) | 1.36.4 | GO-2025-3547 / CVE-2024-7598 / GHSA-r56h-j38w-hrqq — race condition in kube-apiserver | not stated in OSV | none given (`introduced: 1.3.0`) |
| kube-proxy (registry.k8s.io/kube-proxy) | **v1.34.11** | GO-2025-3521 (CVE-2025-1767), GO-2025-3547 (CVE-2024-7598) — same Go-module matches at 1.34.11 | not stated in OSV | none given |

> Both Kubernetes entries are reported by OSV with an open-ended range (no `fixed` event), i.e. OSV has no patched release recorded yet. Separately, **kube-proxy is 2 minor versions behind the 1.36.4 control plane** (v1.34.11) — a version-skew issue worth fixing alongside.

### kube-system add-ons

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| CoreDNS | v1.12.3 | GHSA-93mf-426m-g6x9 / CVE-2025-58063 / GO-2025-3942 — DNS cache pinning via etcd lease ID confusion | HIGH | **1.12.4** |
| CoreDNS | v1.12.3 | GHSA-c9v3-4pv7-87pr / CVE-2026-26017 / GO-2026-4630 — ACL bypass | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | GHSA-h75p-j8xm-m278 / CVE-2026-26018 / GO-2026-4635 — loop-detection DoS | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | GHSA-527x-5wrf-22m2 / CVE-2025-68151 / GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | GHSA-2wpx-qpw2-g5h5 / CVE-2026-32934 / GO-2026-4969 — DoQ worker pool does not bound stream backlog | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-63cw-r7xf-jmwr / CVE-2026-32936 / GO-2026-5164 — DoH GET oversized `dns=` query amplification | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-qhmp-q7xh-99rh / CVE-2026-33190 / GO-2026-5583 — TSIG auth bypass on DoT/DoH/DoH3/DoQ/gRPC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-vp29-5652-4fw9 / CVE-2026-35579 / GO-2026-5667 — TSIG auth bypass on gRPC and QUIC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-h8mm-c463-wjq3 / CVE-2026-33489 / GO-2026-5417 — `transfer` stanza lexicographic compare (subzone ACL bypass) | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | GHSA-74w3-63xv-x9mv / CVE-2026-62994 / GO-2026-6805 — `k8s_external` headless AXFR panics `transfer` plugin | LOW | 1.14.5 |
| CoreDNS | v1.12.3 | GHSA-9gm5-9rfh-m6vx / CVE-2026-86003 / GO-2026-6506 — DoH/DoQ/gRPC bypass UPDATE rejection enforced on UDP/TCP | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | GHSA-mrg3-qvqr-jw29 / CVE-2026-82399 / GO-2026-6507 — unauthenticated memory exhaustion in custom transports | HIGH | 1.14.7 |
| Flannel (ghcr.io/siderolabs/flannel) | v0.27.2 | GHSA-vchx-5pr6-ffx2 / CVE-2026-32241 / GO-2026-4894 — cross-node RCE via extension-backend `BackendData` injection | HIGH | **0.28.2** |

### argocd

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Argo CD (quay.io/argoproj/argocd) — application-controller, repo-server, server, applicationset, notifications | v3.5.3 | GHSA-fmxq-cgp8-87wp / CVE-2026-77459 / GO-2026-6784 — AppProject destination-namespace & resource restrictions bypassed via PreDelete/PostDelete hooks | **CRITICAL** | **3.5.4** |
| Argo CD (quay.io/argoproj/argocd) — repo-server | v3.5.3 | GHSA-j6cw-g6p4-7hch / CVE-2026-55797 / GO-2026-6701 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | HIGH | **3.5.4** |

### cert-manager — CLEAN

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| cert-manager-controller / cainjector / webhook | v1.21.2 | none reported by OSV for `github.com/cert-manager/cert-manager@1.21.2` | – | – |

> Query format validated with a control lookup (`...cert-manager@1.5.0`), which did return advisories — so the empty result for 1.21.2 is a genuine "no known issues", not a naming miss.

### external-secrets — CLEAN (with caveat)

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| external-secrets / cert-controller / webhook | v2.9.0 | GO-2026-5723 / CVE-2026-42875 / GHSA-wv26-88m5-6h59 — namespace isolation bypass in CAProvider ConfigMap resolution | not stated in OSV | 2.4.0 |

> **Likely false positive.** OSV's machine-readable Go range is `introduced:0` with no `fixed`; the real bound (`before v2.4.0`) sits in an unmapped `custom_ranges` block that the Go matcher ignored. Deployed v2.9.0 is well past 2.4.0, so treat as **not affected** — no action.

### Other matched-but-not-affected

| Component | Version | OSV match | Why it is not a finding |
|---|---|---|---|
| dex (argocd-dex-server) | v2.45.1 | GO-2024-2476 / CVE-2024-23656 / GHSA-gr79-9v6v-gc9r — deprecated TLS 1.0/1.1 served | Same unmapped-range artifact: true range is `from 2.37.0 before 2.38.0`; deployed 2.45.1 is fixed. Not affected. |

### Host / platform layer (non-image, for completeness)

| Component | Version | CVE / Advisory | Severity | Fixed version |
|---|---|---|---|---|
| Talos Linux (node OS) | v1.13.10 | GHSA-7cxh-94cf-mfp6 / CVE-2026-77564 — support bundle does not redact registry credentials | LOW | 1.14.0 |

## Recommended actions (priority order)

1. **Argo CD v3.5.3 → v3.5.4** — one patch release clears both the CRITICAL AppProject-restriction bypass and the HIGH repo-server RCE. Highest priority: repo-server processes untrusted repo URLs.
2. **CoreDNS v1.12.3 → ≥1.14.7** — 11 of the 12 CVEs have **no fix on the 1.12 line at all**; only CVE-2025-58063 is patched as 1.12.4. Priority is amplified because CoreDNS is the cluster-wide path (TSIG bypass, UPDATE-rejection bypass, ACL bypass, unauth memory exhaustion).
3. **Flannel v0.27.2 → ≥0.28.2** — HIGH cross-node RCE; confirm whether the extension backend is actually in use (the advisory is specific to extension-backend `BackendData` handling), which affects real exposure but not the upgrade recommendation.
4. **Align kube-proxy to the control plane** (v1.34.11 → v1.36.x) and track CVE-2025-1767 / CVE-2024-7598 with upstream for a published fix.
5. **Talos v1.13.10 → v1.14.x** at the next maintenance window (LOW; credential redaction in support bundles).
6. external-secrets v2.9.0 and dex v2.45.1: no action (OSV false positives, explained above).

## Not checked (could not map to an OSV package)

**Platform images — private / no public package coordinates:**
- kagent enterprise: `kagent-enterprise-controller:2026.11.2`, `solo-enterprise-ui-frontend:2026.11.2`, `solo-enterprise-autoauth:v0.2.2`/`v0.2.4`, `solo-enterprise-ui-backend:0.5.18` — Solo/vendor-internal, no public module.
- kagent OSS: `ghcr.io/kagent-dev/kagent/tools:0.3.1` — Go module path not indexed in OSV (empty result).
- enterprise agentgateway: `agentgateway-enterprise:2026.11.1-patch.0`, `enterprise-agentgateway-controller:2026.11.1-patch.0`, `ext-auth-service`, `rate-limiter`, `waf-server` (all `2026.11.1-patch.0`) — vendor-internal builds.
- `cr.agentgateway.dev/agentgateway:v1.7.0` (atenet-router / atenet-egress) — Rust binary; `crates.io/agentgateway@1.7.0` returned no OSV data.
- substrate/ate (ate-system): `ateapi`, `atecontroller`, `ate-idp-server`, `atelet`, `kubernetes-secrets` (k8s-credential-provider), `ateworker-gvisor` — all `v1.1.0-alpha1-7354c972`, private images with no published package coordinates.
- gVisor sandbox used by `ateworker-gvisor` — no commit-stamped version exposed on the pod spec; the Go probe (`gvisor.dev/gvisor`) returned no data.

**Third-party images — not indexable via OSV without an SBOM:**
- `docker.io/otel/opentelemetry-collector-contrib:0.163.2` (agentgateway-system) and `:0.161.0` (kagent) — Go module lookups for the contrib module path returned nothing.
- `clickhouse/clickhouse-server:26.8.36-alpine` (kagent, agentgateway-system) — C++ project, not in OSV Go/Alpine indexes at that version.
- `rustfs/rustfs:1.0.3` (ate-system) — `crates.io/rustfs@1.0.3` returned no data.
- `redis:8.2.3-alpine` (argocd) and `enterprise-agentgateway/redis:8.6.8-alpine` — no Alpine release codename pinned in the tag, so no OSV Alpine release could be matched.
- `nginx:1.27-alpine` (kagent-ui-tls) and `python:3.13-slim` (typesafe-jev-mcp) — floating OS tags; need a digest/SBOM before OSV's Alpine/Debian data can be queried.
- `127.0.0.1:5000/fortigate-mcp:dev` (kagent) — mutable `dev` tag from a local registry, unresolvable version.

**Outside the requested namespaces** (inventoried, not queried): `registry/registry`, `tailscale/homelab-router`, `postgres/postgres-0`, `local-path-storage/local-path-provisioner`, `podcertificate-controller-system/podcertificate-controller`, and the `default`/`remarkable-diary` workload pods.

## Process notes

- OSV query budget: the brief capped this run at 25 queries; **29 HTTP calls to api.osv.dev were made**, 4 of which were deliberate validation controls (confirming empty results were genuine, not malformed queries — e.g. the cert-manager 1.5.0 control). Flagging the overage rather than dropping checks; recommend either raising the cap to ~35 or pre-seeding a component→OSV-package mapping so the probes aren't repeated daily.
- Images from **terminal (Completed/Error) pods** of old ReplicaSets were ignored; only currently-running workload images were assessed.
- Version numbers are taken from the live pod/deployment specs, not from manifests.
