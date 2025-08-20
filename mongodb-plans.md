---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-20"

keywords: mongodb ee, mongodb enterprise, mongodb enterprise edition, mongodb eneterprise plan

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Plan overview 
{: #mongodb-plans}

{{site.data.keyword.databases-for}} offers two plans for MongoDB: **{{site.data.keyword.databases-for-mongodb_full}} Standard** and **{{site.data.keyword.databases-for-mongodb_full}} Enterprise**. {{site.data.keyword.databases-for-mongodb}} is a fully managed NoSQL database service based on the MongoDB Community Edition. {{site.data.keyword.databases-for-mongodb}} Enterprise offers advanced features, such as the [MongoDB Ops Manager](#mongodbee-ops-manager) and [point-in-time recovery](#mongodbee-pitr).

The choice between the two depends on your specific needs, security requirements, and budget.

**{{site.data.keyword.databases-for-mongodb_full}} Enterprise** is only available on Isolated Compute and Dedicated Cores hosting models. Dedicated cores will be replaced by Isolated Compute as of May, 2025.
{: note}

## Enterprise Edition features
{: #mongodb-plans-ee}

### Ops Manager
{: #mongodbee-ops-manager}

The Ops Manager is only available with an {{site.data.keyword.databases-for-mongodb}} Enterprise Plan deployment. MongoDB Ops Manager allows administrators to monitor, deploy, configure, and optimize MongoDB deployments. Ops Manager offers a range of features that help streamline database management and enhance operational efficiency. Here are some key benefits and features of MongoDB Ops Manager:

- Monitoring and alerting
- Backup and recovery
- Automation and deployment
- Performance optimization
- [Security and compliance](/docs/databases-for-mongodb?topic=databases-for-mongodb-manage-security-compliance&interface=api)
- Scalability and capacity planning

For more information, see [{{site.data.keyword.databases-for-mongodb}} Enterprise Ops Manager](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager).



### Point-in-time recovery
{: #mongodbee-pitr}

{{site.data.keyword.databases-for-mongodb}} Enterprise Plan offers point-in-time recovery (PITR). PITR restores an instance to a specific moment in time, allowing you to recover your data up to a particular point, such as just before a critical error or data loss occurred. This feature is essential for ensuring data durability and minimizing the impact of accidental data deletions or database corruption.For more information, see [{{site.data.keyword.databases-for-mongodb}} Enterprise Plan Point-in-time Recovery](/docs/databases-for-mongodb?topic=databases-for-mongodb-pitr).
