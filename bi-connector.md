---
copyright:
  years: 2017, 2020
lastupdated: "2022-02-04"

keywords: mongodb, databases, connecting, pymongo, java driver, self-signed certificate, mongodbee, bi connector

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:term: .term}

# What is the MongoDB Connector for BI?
{: #mongodb-bi-connector}

As detailed in the MongoDB [documentation](https://docs.mongodb.com/bi-connector/current/), the MongoDB Connector for business intelligence (BI) allows you to run SQL queries against collections, visualize MongoDB data with Tableau, and analyze MongoDB data with Excel. 

# Using a BI Connector 
{: #mongodb-using-bi-connector}

To use a BI connector tool with your MongoDB deployment, you need to provision a read only replica and enable it to be an [Analytics node](https://docs.atlas.mongodb.com/reference/faq/deployment/#what-are-analytics-nodes-), which is a specialized read-only node used to isolate queries that you don't want to affect your operational workload.


