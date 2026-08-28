# Azure Monitoring & Observability: HA Web Infrastructure

**Author:** Maurrin Carter  
**Technologies:** Azure Monitor, Log Analytics (KQL), Azure Monitor Agent (AMA), Data Collection Rules (DCR), Grafana, Prometheus  

---

## Project Overview

This project demonstrates a production-grade monitoring and observability architecture built on Microsoft Azure. It covers the end-to-end configuration of Azure Monitor, Log Analytics, and Grafana to track the health, availability, and real-time performance of a high-availability web infrastructure.

---

## Architecture & Infrastructure Summary

| Resource | Resource Name / Details | Purpose |
|---|---|---|
| **Resource Group** | `ha-web-infrastructure-rg` | Primary resource boundary |
| **Virtual Machines** | `vm-web-01`, `vm-web-02`, `vm-db-01`, `vm-db-02` | High-availability compute tier |
| **Load Balancer** | `ha-load-balancer` | Traffic distribution and health probing |
| **Log Analytics** | `ha-web-logs` | Centralized telemetry, logs, and query engine |
| **Data Collection** | `ha-linux-dcr` (DCR) | Ingestion rules for Syslog, Perf, and Heartbeats |

---

## 1. Azure Monitor & Agent Configuration

The virtual machines were configured with the **Azure Monitor Linux Agent (AMA)** and onboarded via System-Assigned Managed Identities. A **Data Collection Rule (DCR)** was provisioned in `eastus2` to route performance metrics, syslog streams, and agent heartbeats directly to `ha-web-logs`.

### Resource Group Overview
![Resource Group Overview 1](./screenshots/azure-monitor/01_resource-group-overview-pg1.png)
![Resource Group Overview 2](./screenshots/azure-monitor/01_resource-group-overview-pg2.png)
![Resource Group Overview 3](./screenshots/azure-monitor/01_resource-group-overview-pg3.png)
![Resource Group Overview 4](./screenshots/azure-monitor/01_resource-group-overview-pg4.png)

### Agent & Data Collection Configuration
![VM Web 01 Agent Installed](./screenshots/azure-monitor/02_vm-web-01-agent-installed.png)
![Data Collection Rules](./screenshots/azure-monitor/03_data-collection-rules.png)
![Log Analytics Workspace Overview](./screenshots/azure-monitor/10_log-analytics-workspace-overview.png.png)

---

## 2. Alert Rules & Action Groups

Alerting policies were established to catch infrastructure bottlenecks. Static and dynamic thresholds were paired with dedicated Action Groups to deliver automated notifications upon threshold breach.

### Alert Rule Definitions
![Alert Rules List Page 1](./screenshots/azure-monitor/05_alert-rules-list-pg1.png)
![Alert Rules List Page 2](./screenshots/azure-monitor/05_alert-rules-list-pg2.png)
![Alert Rule Details CPU VM Web 01](./screenshots/azure-monitor/06_alert-rule-detail-cpu-vm-web-01.png)

### Action Groups & Notification Delivery
![Action Group Configuration](./screenshots/azure-monitor/08_action-group-config.png)
![Action Group Notification Delivery Page 1](./screenshots/azure-monitor/09_action-group-notification-delivered-pg1.png)
![Action Group Notification Delivery Page 2](./screenshots/azure-monitor/09_action-group-notification-delivered-pg2.png)
![Alert Fired State](./screenshots/azure-monitor/07_alert-fired-state.png)

---

## 3. Log Analytics & Key KQL Queries

Log Analytics was used to execute KQL queries against the `Heartbeat`, `Perf`, and `Syslog` tables for operational validation.

### 1. Active Heartbeat Check
```kql
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
| project Computer, OSType, Version, RemoteIPCountry, TimeGenerated
| order by TimeGenerated desc
