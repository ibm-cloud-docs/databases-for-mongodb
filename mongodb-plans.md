---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-22"

keywords: mongodb ee, mongodb enterprise, mongodb enterprise edition, mongodb eneterprise plan

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Plan overview 
{: #mongodb-plans-overview}

{{site.data.keyword.databases-for}} offers two plans for MongoDB: **{{site.data.keyword.databases-for-mongodb_full}} Standard** and **{{site.data.keyword.databases-for-mongodb_full}} Enterprise**. Both plans provide managed MongoDB deployments with high availability, scaling, and security.  

## Feature comparison
{: #mongodb-plans-features}

| Feature | Standard plan | Enterprise plan |
|---------|---------------|-----------------|
| **Edition** | MongoDB Community Edition. Available on Shared Compute and Isolated Compute. | MongoDB Enterprise Edition with IBM-managed Ops Manager. Available only on Isolated Compute. |
| **Hosting model** | Can be deployed on Shared and Isolated compute. | Only available on Isolated Compute. |
| **Ops manager** | Not included. | Included. Provides integrated tooling for monitoring, configuration, backup, automation, and security. |
| **High availability** | Replica sets for high availability. | Advanced replication and automated failover, with Ops Manager for backup and point-in-time recovery. |
| **Backups** | Daily automated snapshots for disaster recovery. Manual on-demand backups are also available. | Continuous, incremental backups with **Point-in-Time Recovery (PITR)** for up to 7 days. Includes queryable daily snapshots and manual on-demand backups. |
| **Restores** | Standard restore performance from backups. | Faster restore times from incremental snapshots and backups. |
| **Monitoring** | Basic host and database metrics (CPU, memory, operations per second) through integration with the {{site.data.keyword.monitoringfull}} service. | Ops Manager dashboards with real-time and historical views. Advanced performance monitoring, query profiler, automated index recommendations, and custom alerts, along with {{site.data.keyword.monitoringfull}} service integration. |
| **Auditing** | Basic capabilities to review activity. | Granular auditing of schema changes, authentication events, and CRUD operations for compliance and security needs. |
| **Encryption** | Automatic decryption with explicit encryption. | Automatic decryption and automatic encryption managed within the service. |
{: caption="Feature comparison" caption-side="top"}

## Choosing a plan
{: #mongodb-plans-choosing}

- Select the **{{site.data.keyword.databases-for-mongodb_full}} Standard** plan for general-purpose workloads that need managed MongoDB with automated backups and flexible hosting models.  
- Select the **{{site.data.keyword.databases-for-mongodb_full}} Enterprise** plan for workloads that require Ops Manager, Point-in-Time Recovery, advanced monitoring, or detailed auditing for compliance.  
