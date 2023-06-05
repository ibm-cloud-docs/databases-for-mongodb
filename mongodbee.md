---

copyright:
  years: 2023
lastupdated: "2023-06-05"

keywords: mongodb ee, mongodb enterprise, mongodb enterprise edition, mongodb eneterprise plan

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.databases-for-mongodb}} Enterprise
{: #mongodb-ee-info}

{{site.data.keyword.databases-for}} offers two plans for MongoDB: {{site.data.keyword.databases-for-mongodb_full}} and {{site.data.keyword.databases-for-mongodb_full}} Enterprise. {{site.data.keyword.databases-for-mongodb}} is a fully managed NoSQL database service based on the MongoDB Community Edition, while {{site.data.keyword.databases-for-mongodb}} Enterprise offers advanced features, enhanced security, and compliance capabilities based on the MongoDB Enterprise Edition. The choice between the two depends on your specific needs, security requirements, and budget.

## Differences and Advantages
{: #mongodb-ee-diff-adv}

There are differences between the two. To select the right plan, check your specific needs, security requirements, and budget.

| Feature                           | IBM Cloud MongoDB                | IBM Cloud MongoDB Enterprise     |
|-----------------------------------|---------------------------------|---------------------------------|
| Database Edition                  | MongoDB Community Edition       | MongoDB Enterprise Edition      |
| Managed Service                   | Yes                             | Yes                             |
| Basic Monitoring                  | Yes                             | Yes                             |
| Automated Backups                 | Yes                             | Yes                             |
| Scaling                           | Yes                             | Yes                             |
| High Availability                 | Yes                             | Yes                             |
| Field-Level Encryption            | No                              | Yes                             |
| In-Memory Storage Engine          | No                              | Yes                             |
| Advanced Security Options         | No                              | Yes                             |
| Comprehensive Monitoring          | No                              | Yes                             |
| Ops Manager         | No                              | Yes                             |
| Analytics Add-On         | No                              | Yes                             |
| Integration with Security & Compliance Center | No                     | Yes                             |
{: caption="Table 1. {{site.data.keyword.databases-for-mongodb}} Plan Comparison" caption-side="top"}

### Ops Manager
{: #mongodbee-ops-manager}

The Ops Manager is only available with an {{site.data.keyword.databases-for-mongodb}} Enterprise Plan deployment. MongoDB Ops Manager allows administrators to monitor, deploy, configure, and optimize MongoDB deployments. Ops Manager offers a range of features that help streamline database management and enhance operational efficiency. Here are some key benefits and features of MongoDB Ops Manager:

- Monitoring and Alerting
- Backup and Recovery
- Automation and Deployment
- Performance Optimization
- [Security and Compliance](/docs/databases-for-mongodb?topic=databases-for-mongodb-manage-security-compliance&interface=api)
- Scalability and Capacity Planning

For more information, see [{{site.data.keyword.databases-for-mongodb}} Enterprise Ops Manager](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager&interface=cli).

### {{site.data.keyword.databases-for-mongodb}} Enterprise Plan Analytics Add-On
{: #mongodbee-analytics-addon}

The {{site.data.keyword.databases-for-mongodb}} Enterprise Plan Analytics Add-On enhances the capabilities of {{site.data.keyword.databases-for-mongodb}}. The add-on enables users to leverage powerful analytics and business intelligence (BI) tools to extract insights and perform advanced analytics on their MongoDB data. Here are the key features and benefits of the {{site.data.keyword.databases-for-mongodb}} Enterprise Plan Analytics Add-On:

1. Integration with [MongoDB Connector for BI](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics&interface=api#mongodbee-analytics-connector-bi): The add-on allows you to connect popular business intelligence (BI) tools, such as Tableau, Power BI, or Looker, directly to your MongoDB database. It leverages the MongoDB Connector for BI, which provides a SQL interface to query and analyze the data stored in MongoDB collections.

2. SQL-based Analytics: With the add-on, you can use SQL queries to perform analytics and reporting on your MongoDB data. This simplifies the process of extracting insights from the database and enables users familiar with SQL to leverage their existing skills for MongoDB analytics.

3. Aggregation Framework: The add-on supports MongoDB's Aggregation Framework, which provides a powerful and flexible way to process and transform data within the database. You can use the Aggregation Framework to perform complex data transformations, aggregations, and calculations to generate meaningful insights from your MongoDB collections.

4. Performance Optimization: The add-on optimizes query performance by utilizing MongoDB's native indexes and query execution plans. It leverages the rich indexing capabilities of MongoDB to speed up data retrieval and processing, allowing for efficient analytics on large datasets.

5. Data Governance and Security: The IBM Cloud MongoDB Enterprise Analytics add-on provides security and governance features to protect your data. It integrates with the security mechanisms and access controls of MongoDB Enterprise, ensuring that only authorized users can access and analyze the data.

6. Scalability and Availability: The add-on is designed to work seamlessly with{{site.data.keyword.databases-for-mongodb}} Enterprise deployments, providing scalability and high availability. It can handle large volumes of data and support high-performance analytics for demanding workloads.

By using the {{site.data.keyword.databases-for-mongodb}} Enterprise Plan Analytics Add-On, unlock the full potential of your MongoDB data for analytics and reporting purposes. It enables integration with popular BI tools, SQL-based analytics, performance optimization, and data governance, allowing you to derive valuable insights and make data-driven decisions from your MongoDB databases.

### Features and Capabilities
{: #mongodb-ee-feat-cap}

{{site.data.keyword.databases-for-mongodb}} is based on the open-source MongoDB Community Edition. It provides a fully managed NoSQL database service with features like automated backups, scaling, monitoring, and high availability. It is suitable for most general-purpose MongoDB workloads.

On the other hand, {{site.data.keyword.databases-for-mongodb}} Enterprise offers additional advanced features and capabilities. It is based on the MongoDB Enterprise Edition, which includes enterprise-grade features such as field-level encryption, in-memory storage engine, advanced security options, and comprehensive monitoring and analytics capabilities. MongoDB Enterprise is designed for more demanding and complex workloads with specific security, compliance, and performance requirements.

### Security and Compliance
{: #mongodb-ee-sec-comp}

{{site.data.keyword.databases-for-mongodb}} Enterprise provides enhanced security features compared to the standard version. It offers field-level encryption, which allows you to encrypt specific fields within documents, providing an additional layer of security for sensitive data. This feature is not available in the regular MongoDB version.

Furthermore, {{site.data.keyword.databases-for-mongodb}} Enterprise offers additional security controls, [audit logging](/docs/databases-for-mongodb?topic=databases-for-mongodb-auditlogging&interface=cli), and integrations with [{{site.data.keyword.compliance_full}}](/docs/security-compliance?topic=security-compliance-getting-started). These features are designed to meet the requirements of regulated industries and organizations with stringent security and compliance needs.

