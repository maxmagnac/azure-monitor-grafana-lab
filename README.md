# Azure Monitoring & Observability: HA Web Infrastructure

**Author:** Maurrin Carter  
**Technologies:** Azure Monitor, Log Analytics (KQL), Azure Monitor Agent (AMA), Data Collection Rules (DCR), Grafana, Prometheus  

---

## Project Overview

This project demonstrates a production-grade monitoring and observability architecture built on Microsoft Azure. It covers end-to-end configuration of Azure Monitor, Log Analytics, and Grafana to track the health, availability, and performance of a high-availability web infrastructure.

---

## Architecture & Infrastructure Summary

| Resource | Resource Name / Details | Purpose |
|---|---|---|
| **Resource Group** | `ha-web-infrastructure-rg` | Primary resource container |
| **Virtual Machines** | `vm-web-01`, `vm-web-02`, `vm-db-01`, `vm-db-02` | Workload compute instances |
| **Load Balancer** | `ha-load-balancer` | Traffic distribution and health probing |
| **Log Analytics** | `ha-web-logs` | Centralized telemetry and log ingestion |
| **Data Collection** | `ha-linux-dcr` (DCR) | Policy-driven syslog, metric, and heartbeat pipeline |

---

## Observability Stack & Tools

- **Azure Monitor & AMA:** Deployed Azure Monitor Linux Agent via Managed Identities and Data Collection Rules (DCR) for agent-based telemetry ingestion.
- **Log Analytics & KQL:** Structured querying across `Heartbeat`, `Perf`, and `Syslog` tables for operational health checks.
- **Grafana:** Real-time dashboards visualizing CPU utilization, memory thresholds, and node health via Azure Monitor data source integration.
- **Prometheus:** Metrics scraping pipeline integrated into dashboard visualizations.

---

## Screenshots

| # | File Path | Description |
|---|---|---|
| 01 | `screenshots/azure-monitor/01-workspace-overview.png` | Log Analytics workspace and ingestion configuration |
| 02 | `screenshots/azure-monitor/02-alert-rules.png` | Azure Monitor alert rules and Action Group targets |
| 03 | `screenshots/azure-monitor/03-heartbeat-query.png` | KQL Heartbeat query confirming all VM instances actively reporting |
| 04 | `screenshots/grafana/04-datasource-setup.png` | Grafana connected via Azure Monitor Service Principal |
| 05 | `screenshots/grafana/05-cpu-performance.png` | Dashboard telemetry validating stress-test spikes and VM lifecycle events |

---

## Key KQL Queries

### 1. Active Heartbeat Verification
```kql
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
| project Computer, OSType, Version, RemoteIPCountry, TimeGenerated
| order by TimeGenerated desc
