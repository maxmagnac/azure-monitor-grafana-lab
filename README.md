# Azure Monitoring & Observability: HA Web Infrastructure

**Author:** Maurrin Carter

## Project Overview

This project demonstrates a production-grade monitoring and observability stack built on Microsoft Azure. It covers end-to-end configuration of Azure Monitor, Log Analytics, and Grafana to track the health and performance of a high-availability web infrastructure.

## Infrastructure Summary

| Resource | Name |
|---|---|
| Resource Group | `ha-web-infrastructure-rg` |
| Virtual Machines | `vm-web-01`, `vm-web-02`, `vm-db-01`, `vm-db-02` |
| Log Analytics Workspace | `ha-web-logs` |
| Load Balancer | `ha-load-balancer` |

## Tools Used

- **Azure Monitor** - Metrics collection, alert rules, and action groups
- **Log Analytics** - KQL-based querying and heartbeat monitoring
- **Grafana** - Real-time dashboard visualization connected via Azure Monitor data source
- **Prometheus** - Metrics scraping integrated with Grafana

## Screenshots

| # | Screenshot | Description |
|---|---|---|
| 01 | `screenshots/azure-monitor/` | Log Analytics workspace and alert configuration |
| 02 | `screenshots/azure-monitor/` | Azure Monitor alert rules and action groups |
| 03 | `screenshots/azure-monitor/` | Heartbeat query results confirming all VMs reporting |
| 04 | `screenshots/grafana/` | Grafana connected to Azure Monitor data source |
| 05 | `screenshots/grafana/` | CPU dashboard showing reboot spike and stress test validation |

## Key KQL Queries

**Heartbeat Check:**
```kql
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
CPU Trend:

Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5m)
What This Project Meant to Me
This project was important to me because it showed that I can set up alerting and monitoring using the same tools used in cloud engineering operations. Working with Azure Monitor and Grafana gave me hands-on experience with the exact stack that cloud and DevOps engineers use to keep production systems healthy and observable.

Skills Demonstrated
Azure Monitor configuration and alert rule creation
Log Analytics KQL query writing
Grafana dashboard setup with Azure Monitor integration
Service principal configuration and Azure RBAC assignment
End-to-end observability pipeline validation

---
