# agent-summary

Daily reports written by kagent agents running on **Agent Substrate** (cluster `omni-viper`).
Each report is committed by the agent itself, from its gVisor sandbox, through the GitHub MCP server.

## Layout

```
reports/
  <report>/
    latest.md               ← always the newest run (overwritten)
    YYYY/YYYY-MM-DD.md      ← one file per day (history)
```

| Report | Agent | Schedule (America/Toronto) | Sources |
| --- | --- | --- | --- |
| `fortigate-health` | `fortigate` | daily 09:00 | `fg_resource_usage`, `fg_system_status` on fw-maniak-hq |
| `k8s-cve` | `k8s-cve-checker` | daily 09:15 | Kubernetes version + container images → [OSV.dev](https://osv.dev) |
| `fortiguard-cve` | `fortigate-cve-watch` | daily 09:30 | FortiOS version → FortiGuard PSIRT + [Outbreak Alerts](https://fortiguard.fortinet.com/outbreak-alert) RSS |

Every report starts with a one-line verdict, then the tables. Agents are read-only on the systems they inspect;
the only thing they write is this repo.

Manifests: `apps/kagent/agent-cve-watchers.yaml` and `apps/kagent/github-mcp.yaml` in the viper-env repo.
