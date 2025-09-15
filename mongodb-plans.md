---
copyright:
  years: 2024, 2025
lastupdated: "2025-09-11"

keywords: mongodb ee, mongodb enterprise, mongodb enterprise edition, mongodb enterprise plan

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Plan overview
{: #mongodb-plans}

{{site.data.keyword.databases-for}} offers two plans for MongoDB: **{{site.data.keyword.databases-for-mongodb_full}} Standard** and **{{site.data.keyword.databases-for-mongodb_full}} Enterprise**. Both plans provide managed MongoDB deployments with built-in high availability, scaling, and security.  

The choice between the two depends on your specific needs, security requirements, and budget.

## Feature comparison
{: #mongodb-plans-features}

| Feature | Standard plan | Enterprise plan |
|---------|---------------|-----------------|
| **Edition** | MongoDB Community Edition | MongoDB Enterprise Edition with IBM-managed Ops Manager. |
| **Hosting model** | Available on Shared or Isolated compute | Available only on Isolated compute. |
| **Ops Manager** | Not included | Included. Provides monitoring, configuration, backup, automation, and security tooling. |
| **High availability** | Replica sets for fault tolerance and high availability | Enhanced replication and automated failover with [Enterprise Ops Manager](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager). |
| **Backups** | Daily automated snapshots for disaster recovery. Manual, on-demand backups also supported. | Continuous, incremental backups with [Point-in-Time Recovery](#mongodbee-pitr) for up to 7 days. Includes queryable daily snapshots and manual, on-demand backups. |
| **Restores** | Standard restore from backups | Faster restore performance from incremental snapshots and PITR |
| **Monitoring** | Basic host and database metrics (CPU, memory, ops/sec) through [{{site.data.keyword.monitoringfull}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring) integration | Ops Manager dashboards with real-time and historical views, query profiler, automated index recommendations, custom alerts, plus [{{site.data.keyword.monitoringfull}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring) integration. |
| **Auditing** | Basic activity review | Granular auditing of schema changes, authentication events, and CRUD operations for compliance and security. |
| **Encryption** | Automatic decryption. Encryption must be managed explicitly by the application. | Automatic decryption and service-managed encryption at rest and in transit. |
{: caption="Feature comparison" caption-side="top"}

## Choosing a plan
{: #mongodb-plans-choosing}

- Choose the **Standard plan** for general-purpose workloads that need managed MongoDB with automated backups and a flexible hosting model.  
- Choose the **Enterprise plan** for workloads that require Ops Manager, Point-in-Time Recovery, advanced monitoring, or detailed auditing to meet compliance and regulatory needs.  
