---

copyright:
  years: 2024
lastupdated: "2024-05-28"

keywords: mongodb ee, mongodb enterprise, mongodb enterprise edition, mongodb eneterprise plan

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Plan overview 
{: #mongodb-plans}

{{site.data.keyword.databases-for}} offers two plans for MongoDB: **{{site.data.keyword.databases-for-mongodb_full}} Standard** and **{{site.data.keyword.databases-for-mongodb_full}} Enterprise**. {{site.data.keyword.databases-for-mongodb}} is a fully managed NoSQL database service based on the MongoDB Community Edition. {{site.data.keyword.databases-for-mongodb}} Enterprise offers advanced features, such as the [MongoDB Ops Manager](#mongodbee-ops-manager), the [Analytics Add-on](#mongodbee-analytics-addon), and [point-in-time recovery](#mongodbee-pitr).

The choice between the two depends on your specific needs, security requirements, and budget.

**{{site.data.keyword.databases-for-mongodb_full}} Enterprise** is only available on Isolated Compute and Dedicated Cores hosting models. Dedicated cores will be replaced by Isolated Compute as of May, 2025.
{: note}

## Enterprise Edition features
{: #mongodb-plans-ee}

### Ops Manager
{: #mongodbee-ops-manager}

The Ops Manager is only available with an {{site.data.keyword.databases-for-mongodb}} Enterprise Plan deployment. MongoDB Ops Manager allows administrators to monitor, deploy, configure, and optimize MongoDB deployments. Ops Manager offers a range of features that help streamline database management and enhance operational efficiency. Here are some key benefits and features of MongoDB Ops Manager:

- Monitoring and Alerting
- Backup and Recovery
- Automation and Deployment
- Performance Optimization
- [Security and Compliance](/docs/databases-for-mongodb?topic=databases-for-mongodb-manage-security-compliance&interface=api)
- Scalability and Capacity Planning

For more information, see [{{site.data.keyword.databases-for-mongodb}} Enterprise Ops Manager](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager).


### Analytics Add-On
{: #mongodbee-analytics-addon}

The {{site.data.keyword.databases-for-mongodb}} Enterprise Plan Analytics Add-On enhances the capabilities of {{site.data.keyword.databases-for-mongodb}}. The add-on enables users to leverage powerful analytics and business intelligence (BI) tools to extract insights and perform advanced analytics on their MongoDB data. Here are the key features and benefits of the {{site.data.keyword.databases-for-mongodb}} Enterprise Plan Analytics Add-On:

1. Integration with [MongoDB Connector for BI](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics&interface=api#mongodbee-analytics-connector-bi): The add-on allows you to connect popular business intelligence (BI) tools, such as Tableau, Power BI, or Looker, directly to your MongoDB database. It leverages the MongoDB Connector for BI, which provides an SQL interface to query and analyze the data stored in MongoDB collections.

2. SQL-based Analytics: With the add-on, you can use SQL queries to perform analytics and reporting on your MongoDB data. This simplifies the process of extracting insights from the database and enables users familiar with SQL to leverage their existing skills for MongoDB analytics.

4. Performance Optimization: The add-on optimizes query performance by utilizing MongoDB's native indexes and query execution plans. It leverages the rich indexing capabilities of MongoDB to speed up data retrieval and processing, allowing for efficient analytics on large datasets.

6. Scalability and Availability: The add-on is designed to work seamlessly with {{site.data.keyword.databases-for-mongodb}} Enterprise deployments, providing scalability and high availability. It can handle large volumes of data and support high-performance analytics for demanding workloads.

By using the {{site.data.keyword.databases-for-mongodb}} Enterprise Plan Analytics Add-On, unlock the full potential of your MongoDB data for analytics and reporting purposes. It enables integration with popular BI tools, SQL-based analytics, performance optimization, and data governance, allowing you to derive valuable insights and make data-driven decisions from your MongoDB databases.

For more information, see [{{site.data.keyword.databases-for-mongodb}} Enterprise Analytics Add-On](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics).
--->

### Point-in-time recovery
{: #mongodbee-pitr}

{{site.data.keyword.databases-for-mongodb}} Enterprise Plan offers point-in-time recovery (PITR). PITR restores an instance to a specific moment in time, allowing you to recover your data up to a particular point, such as just before a critical error or data loss occurred. This feature is essential for ensuring data durability and minimizing the impact of accidental data deletions or database corruption.For more information, see [{{site.data.keyword.databases-for-mongodb}} Enterprise Plan Point-in-time Recovery](/docs/databases-for-mongodb?topic=databases-for-mongodb-pitr).
