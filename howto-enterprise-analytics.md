---
copyright:
  years: 2020
lastupdated: "2022-02-17"

keywords: databases, opsman, mongodbee, Enterprise Edition, analytics, bi connector

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# MongoDB Enterprise Edition Analytics Add-On
{: #mongodbee-analytics}

The {{site.data.keyword.databases-for-mongodb}} Enterprise Edition Analytics Add-On allows you to execute long-running analytical queries, without affecting the performance of your regular operational workflow, as well as provision a [MongoDB Connector for business intelligence(BI)](https://docs.mongodb.com/bi-connector/current/) to make your query data compatible with BI tools, such as [Tableau](https://www.tableau.com/).

## How does the MongoDB Enterprise Edition Analytics Add-On work?
{: #mongodbee-analytics-how}

### What problems does the add-on solve?
{: #mongodbee-analytics-how-problem}

1. Most BI tools do not work with MongoDB document data model. 

    MongoDB uses a document data model, made up of complex documents with arbitrary, nested data. This document model schema makes storing data flexible, easy, and scalable. However, most BI tools require data to be in tabular format, which is rigidly defined and stored in tables, not documents. The {{site.data.keyword.databases-for-mongodb}} Enterprise Edition Analytics Add-On converts MongoDB document data into SQL-readable tabular data:

    ![Connector for BI converting document model to table](images/bi-connector-model.png){: caption="Figure 1. Connector for BI converting document model to table" caption-side="bottom"}

1. BI queries are expensive and can degrade database performance. 
    Long-running queries can negatively impact the operational workflow of your deployment. The {{site.data.keyword.databases-for-mongodb}} Enterprise Edition Analytics Add-On introduces an extra data member, which isolates analytics from operation.

    ![Introducing an extra data member](images/bi-connector-extra-data-member.png){: caption="Figure 1. Introducing an extra data member" caption-side="bottom"}
    

The {{site.data.keyword.databases-for-mongodb}} Enterprise Edition Analytics Add-On consists of 2 components:
- the Analytics Node
- connector for BI

### The Analytics Node
{: #mongodbee-analytics-node}

The Analytics Node isolates the analytics from the operational workload, allowing for long-running queries that do not impact operational workflow performance. You can use the Analytics Node directly using MongoDB queries or by using SQL queries, if they are enabled by the connector for BI.

### The connector for BI
{: #mongodbee-analytics-connector-bi}

Traditional BI tools are designed to work with tabular, row-and-column data. The {{site.data.keyword.databases-for-mongodb}} Enterprise Edition Analytics Add-On connector for BI allows you to query MongoDB data with SQL using tools, such as Tableau, by connecting to the Analytics Node and providing an SQL interface.

# MongoDB Enterprise Edition Analytics Add-On Considerations
{: #mongodbee-analytics-consider}

Before taking advantage of the {{site.data.keyword.databases-for-mongodb}} Enterprise Edition Analytics Add-On, consider the following:

- The add-on is available only for {{site.data.keyword.databases-for-mongodb}} Enterprise Edition.
- A connector for BI cannot be provisioned unless an Analytics Node is already provisioned.
- You cannot deprovision the analytics node once it is enabled.
- You cannot scale the disk space of the Analytics members.
- Analytics nodes support horizontal scaling of up to 1 member.
