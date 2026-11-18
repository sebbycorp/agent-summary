# k8s-cve - 2026-11-18

**Verdict: ACTION NEEDED** — Argo CD v3.5.3 is affected by a CRITICAL AppProject-restriction bypass (CVE-2026-77459, fixed in 3.5.4), CoreDNS v1.12.3 carries 12 unpatched advisories (10 HIGH), and the two Redis instances sit on 8.2.3, which has five High RCE-class advisories fixed in 8.2.6.

- **Cluster:** omni-viper (single control-plane node `talos-cyv-ekb`)
- **Kubernetes:** v1.36.4 · Talos (v1.13.10) · containerd 2.2.7 · kernel 6.18.48-talos
- **Checked:** 2026-11-18 (America/Toronto) — read-only inventory + OSV.dev queries
- **OSV.dev:** reachable, every query answered. **30** `/v1/query` calls issued against a soft budget of 25 — the overage was spent on re-checking anomalous `fixed:` ranges (external-secrets, dex, flannel), on the nginx/redis lookups, and on two parser-retry re-runs. Kubernetes and the seven platform images were prioritised; everything else is listed under "not checked".

## 1. Inventory (deduplicated images)

| Image | Namespace(s) | Mapped OSV package |
|---|---|---|
| `registry.k8s.io/kube-proxy:v1.34.11` | kube-system | Go `k8s.io/kubernetes` @ 1.34.11 |
| `registry.k8s.io/coredns/coredns:v1.12.3` | kube-system | Go `github.com/coredns/coredns` |
| `ghcr.io/siderolabs/flannel:v0.27.2` | kube-system | Go `github.com/flannel-io/flannel` |
| kube-apiserver / -scheduler / -controller-manager v1.36.4 (Talos static pods) | kube-system | Go `k8s.io/kubernetes` @ 1.36.4 |
| `quay.io/argoproj/argocd:v3.5.3` | argocd | Go `github.com/argoproj/argo-cd/v3` |
| `ghcr.io/dexidp/dex:v2.45.1` | argocd | Go `github.com/dexidp/dex` |
| `public.ecr.aws/docker/library/redis:8.2.3-alpine` | argocd | `redis` @ 8.2.3 |
| `quay.io/jetstack/cert-manager-{controller,cainjector,webhook}:v1.21.2` | cert-manager | Go `github.com/cert-manager/cert-manager` |
| `ghcr.io/external-secrets/external-secrets:v2.9.0` | external-secrets | Go `github.com/external-secrets/external-secrets` |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/agentgateway-enterprise:2026.11.0` | agentgateway-system | not mappable |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/enterprise-agentgateway-controller:2026.11.0` | agentgateway-system | not mappable |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/{ext-auth-service,rate-limiter,waf-server}:2026.11.0` | agentgateway-system | not mappable |
| `us-docker.pkg.dev/solo-public/enterprise-agentgateway/redis:8.6.7-alpine` | agentgateway-system | `redis` @ 8.6.7 |
| `us-docker.pkg.dev/solo-public/substrate-enterprise/{ateapi,atecontroller,ate-idp-server,atelet,ateworker-gvisor,kubernetes-secrets}:v0.6.0-b15382d4` | ate-system | not mappable |
| `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e@sha256:ebebf5a7…` | ate-system | not mappable |
| `rustfs/rustfs:1.0.0-beta.3@sha256:378642b0…` | ate-system | not mappable |
| `us-docker.pkg.dev/solo-public/kagent-enterprise/kagent-enterprise-controller:1.0.0-alpha12` | kagent | not mappable |
| `us-docker.pkg.dev/solo-public/solo-enterprise/solo-enterprise-{ui-frontend,autoauth}` (0.5.9, 1.0.0-alpha12, v0.2.2, v0.2.3) | kagent, ate-system, agentgateway-system | not mappable |
| `ghcr.io/kagent-dev/kagent/tools:0.3.0` | kagent | Go `github.com/kagent-dev/kagent` (no entry) |
| `docker.io/otel/opentelemetry-collector-contrib:0.161.0` | kagent, agentgateway-system | Go module (no entry) |
| `clickhouse/clickhouse-server:26.3.35-alpine`, `26.3.17-alpine` | agentgateway-system, kagent | not mappable |
| `nginx:1.27-alpine` | kagent | Alpine `nginx` |
| `pgvector/pgvector:pg16`, `127.0.0.1:5000/fortigate-mcp:dev`, `python:3.13-slim`, registry/tailscale/agent-desk/local-path/podcertificate workloads | postgres, registry, tailscale, kagent, default, local-path-storage, podcertificate-controller-system, remarkable-diary | not mappable / out of scope |

## 2. Findings

Every row below is taken from an OSV.dev response obtained in this session. `sev` is as OSV reports it; "Not ranked" means OSV carried no severity label for that record.

### 2.1 Action needed

| Component | Installed | CVE / advisory | Severity | Fixed version |
|---|---|---|---|---|
| Argo CD (all `argocd:v3.5.3` containers) | v3.5.3 | CVE-2026-77459 · GHSA-fmxq-cgp8-87wp · GO-2026-6784 · BIT-argo-cd-2026-77459 — AppProject destination-namespace / resource restrictions bypassed by PreDelete & PostDelete hooks | **CRITICAL** (CVSS 3.1 `AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H`) | **3.5.4** (also 3.4.10, 3.3.15, 3.6.0-rc2) |
| Argo CD (repo-server) | v3.5.3 | CVE-2026-55797 · GHSA-j6cw-g6p4-7hch · GO-2026-6701 · related CVE-2026-113708 / CVE-2026-113709 — repo-server command injection via crafted SSH repository SOCKS5 proxy URL | HIGH (CVSS 3.1 `AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H`) | **3.5.4** |
| CoreDNS | v1.12.3 | CVE-2025-58063 · GHSA-93mf-426m-g6x9 · GO-2025-3942 — DNS cache pinning via etcd lease-ID confusion | HIGH | 1.12.4 |
| CoreDNS | v1.12.3 | CVE-2025-68151 · GHSA-527x-5wrf-22m2 · GO-2026-4289 — gRPC/HTTPS/HTTP3 servers lack resource limits (DoS) | MODERATE | 1.14.0 |
| CoreDNS | v1.12.3 | CVE-2026-26017 · GHSA-c9v3-4pv7-87pr · GO-2026-4630 — ACL bypass | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-26018 · GHSA-h75p-j8xm-m278 · GO-2026-4635 — loop-detection DoS | HIGH | 1.14.2 |
| CoreDNS | v1.12.3 | CVE-2026-32934 · GHSA-2wpx-qpw2-g5h5 · GO-2026-4969 — DoQ worker pool does not bound stream backlog | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-32936 · GHSA-63cw-r7xf-jmwr · GO-2026-5164 — DoH GET oversized `dns=` parameter, pre-validation CPU/memory exhaustion | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33190 · GHSA-qhmp-q7xh-99rh · GO-2026-5583 — TSIG authentication bypass on DoT/DoH/DoH3/DoQ/gRPC | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-35579 · GHSA-vp29-5652-4fw9 — TSIG authentication bypass on gRPC and QUIC transports | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-33489 · GHSA-h8mm-c463-wjq3 · GO-2026-5417 — transfer stanza lexicographic compare (subzone ACL bypass) | HIGH | 1.14.3 |
| CoreDNS | v1.12.3 | CVE-2026-86003 · GHSA-9gm5-9rfh-m6vx · GO-2026-6506 — DoH/DoQ/gRPC bypass of UPDATE rejection enforced on UDP/TCP | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-82399 · GHSA-mrg3-qvqr-jw29 · GO-2026-6507 — unauthenticated memory exhaustion in custom transports | HIGH | 1.14.7 |
| CoreDNS | v1.12.3 | CVE-2026-62994 · GHSA-74w3-63xv-x9mv · GO-2026-6805 — `k8s_external` headless AXFR empty batch panics | LOW | 1.14.5 |
| Redis — argocd cache (`redis:8.2.3-alpine`) | 8.2.3 | CVE-2026-23479 · BIT-redis-2026-23479 — use-after-free in unblock-client flow (RCE) | High | **8.2.6** (8.4.3 / 8.6.3) |
| Redis — argocd cache | 8.2.3 | CVE-2026-23631 · BIT-redis-2026-23631 — Lua use-after-free (RCE) | High | **8.2.6** |
| Redis — argocd cache | 8.2.3 | CVE-2026-25243 · BIT-redis-2026-25243 — RESTORE invalid memory access (RCE) | High | **8.2.6** |
| Redis — argocd cache | 8.2.3 | CVE-2026-25588 · BIT-redis-2026-25588 — RedisTimeSeries RESTORE invalid memory access (RCE) | High | **8.2.6** |
| Redis — argocd cache | 8.2.3 | CVE-2026-25589 · BIT-redis-2026-25589 — RedisBloom RESTORE invalid memory access (RCE) | High | **8.2.6** |
| Flannel CNI (`flannel:v0.27.2`) | 0.27.2 | CVE-2026-32241 · GHSA-vchx-5pr6-ffx2 · GO-2026-4894 — cross-node RCE via Extension-backend `backend-data` annotation injection | HIGH (conditional — see note) | 0.28.2 |
| Dex (argocd-dex-server) | v2.45.1 | CVE-2024-23656 · GO-2024-2476 · GHSA-gr79-9v6v-gc9r — TLS config discarded; serves TLS 1.0/1.1 + weak ciphers | Not ranked (low confidence) | 2.38.0 (per record's ECOSYSTEM range) |

**Analyst notes (not OSV data, judgement only):**
- **CoreDNS mitigating context:** the deployment only exposes UDP/TCP port 53 plus metrics (9153) and does not serve DoH/DoQ/DoT/gRPC, so most of the HIGH set targets transports this instance does not enable. `registry.k8s.io/coredns/coredns:v1.12.3` is still two minor lines behind (1.14.7 covers everything above), so the upgrade is worth doing as routine hygiene rather than an emergency.
- **Flannel conditional:** GHSA-vchx-5pr6-ffx2 is stated by the advisory to affect clusters using the experimental **Extension** backend. Verify the Flannel configmap backend before treating this as exploitable; with vxlan/wireguard it is not reachable, though 0.28.2 is still the correct target.
- **Dex caveat:** the OSV Go record lists `last_known_affected_version_range "<= 2.39.3"`, an explicit non-authoritative heuristic, while its ECOSYSTEM range is only `2.37.0 → 2.38.0`. Since the running version 2.45.1 is above 2.38.0, treat this as **likely not affected / needs source confirmation**, not as a confirmed finding.
- **Redis caveat:** queried with `ecosystem=Bitnami, package=redis` as a proxy for upstream Redis 8.2.3. CVE-2026-25588/25589 involve the RedisTimeSeries/RedisBloom modules, which the stock `redis:8.2.3-alpine` image may not load — check `MODULE LIST` before assuming exposure. The cache is internal to Argo CD, which lowers (but does not remove) the RCE risk.
- **Argo CD is the priority item:** CVE-2026-77459 is the only CRITICAL in the cluster, is reachable by anyone with write access to a synced Git repo, and the fix is a patch bump (v3.5.3 → v3.5.4). Upgrade argocd-server, repo-server, application-controller, applicationset-controller and notifications-controller together.

### 2.2 Clean (queried, no applicable OSV findings)

| Component | Version | OSV query | Result |
|---|---|---|---|
| Kubernetes control plane (apiserver, controller-manager, scheduler) | 1.36.4 | Go `k8s.io/kubernetes` @ 1.36.4 | 2 records returned, both **superseded/unresolved** — see 2.3 |
| kube-proxy | v1.34.11 | Go `k8s.io/kubernetes` @ 1.34.11 | Same 2 unresolved records |
| cert-manager (controller, cainjector, webhook) | v1.21.2 | Go `github.com/cert-manager/cert-manager` @ 1.21.2 | **No entry.** Control query at 1.8.0 did return CVE-2024-12401 (MODERATE, fixed 1.16.2), confirming the package name is correct and 1.21.2 is ahead of the only advisory |
| External Secrets | v2.9.0 | Go `github.com/external-secrets/external-secrets` @ 2.9.0 | 1 record, **not applicable** — see 2.3 |
| Redis — agentgateway ext-cache (`redis:8.6.7-alpine`) | 8.6.7 | `redis` @ 8.6.7 | No entry — 8.6.7 is ahead of the 8.6.3 patch level for the 8.2.3 findings |

### 2.3 Queried, dismissed as false positives (for the record)

- `k8s.io/kubernetes` **GO-2025-3521** (CVE-2025-1767, GitRepo volume local-repository access) and **GO-2025-3547** (CVE-2024-7598, kube-apiserver race condition): both open-ended `introduced` ranges with **no `fixed` event** and no OSV severity, i.e. Go vuln-db records that were never given patch bounds (both are long-published 2024/2025 CVEs). They match *every* Kubernetes version including 1.36.4 and 1.34.11 and carry no actionable signal; do not treat as open findings. Verify against the Kubernetes security-announce threads if you want closure.
- `github.com/external-secrets/external-secrets` **GO-2026-5723** (CVE-2026-42875, CAProvider ConfigMap namespace-isolation bypass): the record's own details state the real affected range is "before **v2.4.0**" and that the source advisory could not be mapped to standard Go module versions, so the Go record was widened to `introduced 0, fixed 2.4.0`. Installed v2.9.0 > 2.4.0 → **not affected**.
- `github.com/dexidp/dex` **GO-2024-2476** — kept in the action table at low confidence, see caveat above.

### 2.4 nginx (kagent-ui-tls) — stale branch, review

| Component | Installed | Advisory | Severity | Fixed version |
|---|---|---|---|---|
| nginx (kagent-ui-tls sidecar) | `nginx:1.27-alpine` (queried as 1.27.5) | ALPINE-CVE-2025-53859, ALPINE-CVE-2026-1642, ALPINE-CVE-2026-27651, ALPINE-CVE-2026-27654, ALPINE-CVE-2026-27784, ALPINE-CVE-2026-28753, ALPINE-CVE-2026-28755, ALPINE-CVE-2026-32647, ALPINE-CVE-2026-40460, ALPINE-CVE-2026-40701, ALPINE-CVE-2026-42055, ALPINE-CVE-2026-42533, ALPINE-CVE-2026-42934, ALPINE-CVE-2026-42945, ALPINE-CVE-2026-42946, ALPINE-CVE-2026-48142, ALPINE-CVE-2026-49975, ALPINE-CVE-2026-56434, ALPINE-CVE-2026-60005, ALPINE-CVE-2026-9256 (20 advisories) | Not ranked by OSV (CVSS 3.1/4.0 vectors recorded; mostly DoS / integrity-impact) | 1.28.2-r0 / 1.28.3-r0…-r6 / 1.30.x-r0 |

The Alpine DB only lists fixes in the **1.28.x and 1.30.x** series — no 1.27.x backport appears in any of the 20 records, which is the signature of an end-of-life branch. Recommended: move `kagent-ui-tls` to `nginx:1.30-alpine` (or at minimum 1.28-alpine). Note this row is a *branch-level* signal, not 20 individually exploitable findings; exploitability depends on the TLS proxy's server-block configuration.

## 3. Not checked (could not be mapped to an OSV package)

Queried where possible and returned no OSV entry, then left unverified:

- `ghcr.io/kagent-dev/kagent/tools:0.3.0` — Go query `github.com/kagent-dev/kagent@0.3.0` → no entry (registry module path unconfirmed)
- `docker.io/otel/opentelemetry-collector-contrib:0.161.0` — Go queries of both the contrib and core collector modules → no entry at that version (a control query also returned nothing, so treat as "not covered by OSV Go DB", **not** "clean")

No OSV mapping attempted (proprietary, or no upstream package identity), all **unchecked**:

1. `agentgateway-enterprise:2026.11.0` and `enterprise-agentgateway-controller:2026.11.0` — Solo.io enterprise, closed source
2. `ext-auth-service`, `rate-limiter`, `waf-server` `:2026.11.0` — Solo.io enterprise
3. Substrate/ATE: `ateapi`, `atecontroller`, `ate-idp-server`, `atelet`, `ateworker-gvisor`, `kubernetes-secrets` `v0.6.0-b15382d4`
4. `ghcr.io/kagent-dev/substrate/agentgateway:cd65181fd52e` — an OSV query against `crates.io/agentgateway` found no entry for this commit-pinned build; the upstream Rust crate version inside the image is unknown, so no meaningful query is possible
5. Solo enterprise UI/auth: `solo-enterprise-ui-frontend:0.5.9` / `:1.0.0-alpha12`, `solo-enterprise-autoauth:v0.2.2` / `v0.2.3`, `kagent-enterprise-controller:1.0.0-alpha12`
6. `clickhouse/clickhouse-server:26.3.35-alpine`, `26.3.17-alpine`
7. `rustfs/rustfs:1.0.0-beta.3`
8. `pgvector/pgvector:pg16` (no Postgres minor version in the tag)
9. `python:3.13-slim` (typesafe-jev-mcp), `127.0.0.1:5000/fortigate-mcp:dev` (`:dev` tag — unpinnable)
10. Non-target namespaces / out of scope for this run: `registry:…`, `tailscale` homelab-router, `local-path-provisioner`, `podcertificate-controller`, `agent-desk`, and the stale `default/` lab pods

**These are the blind spots.** The agentgateway / substrate / solo-enterprise images are the largest gap: they are core to the kagent + ate-system data path and are not scannable via OSV, so they need vendor advisories or an image-scan tool (Trivy/Grype against the registry) rather than OSV.

## 4. Recommended actions

1. **Argo CD v3.5.3 → v3.5.4** — closes the only CRITICAL (CVE-2026-77459) plus the HIGH repo-server command injection. Until then, restrict write access to synced Git repos and tighten application-controller RBAC (advisory-listed workaround; no config toggle exists).
2. **Redis 8.2.3 → 8.2.6** for `argocd-redis` (and confirm the ext-cache 8.6.7 image is genuinely ≥ 8.6.3).
3. **CoreDNS v1.12.3 → 1.14.7** — routine bump; no DoH/DoQ/gRPC listener is exposed today, so schedule rather than emergency-patch.
4. **nginx `1.27-alpine` → `1.30-alpine`** on `kagent-ui-tls` (1.27 branch has no fixes listed).
5. **Flannel v0.27.2 → v0.28.2**, after confirming which backend the CNI config uses.
6. Ask vendors for SBOM/advisory feeds for the Solo enterprise agentgateway + substrate/ate images; add an image scanner to cover everything in §3.
7. Confirm the `dex:v2.45.1` TLS finding against the upstream advisory before acting.

## 5. Methodology / limits

- Inventory: `kubectl get nodes -o wide`, all-namespaces pod listing, plus `deployment`/`statefulset`/`daemonset` listings in the seven target namespaces and the CoreDNS deployment YAML (pod listings do not expose image versions; workloads and the YAML are authoritative).
- Vulnerability source: **OSV.dev `/v1/query` only**, 30 POST queries via BusyBox `wget` from the sandbox; responses parsed for `id`, `aliases` (CVE…), `summary`, `severity`/CVSS vector and `fixed` events only.
- Control-plane components are reported as `k8s.io/kubernetes` at the node/server version; Talos 1.13.10 and the container host OS/kernel were **not** scanned (no OS-level OSV mapping in scope).
- No registry manifest / SBOM inspection was performed, so image-internal packages (base OS, vendored libraries, bundled runtimes) are out of reach — this scan covers only the application identity implied by the image tag. Several tags (`:dev`, `1.27-alpine`, `pg16`) do not pin an exact build at all.
- Two queries returned an empty/`NO_ENTRY` answer where I would have expected data (otel collector, kagent tools); these are recorded as **not covered**, never as clean.
- Nothing in this report is inferred from memory: if a CVE is not in the tables above, it was not seen in an OSV response during this session.
