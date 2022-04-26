---
copyright:
  years: 2020, 2022
lastupdated: "2022-04-26"

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
{:important: .important}

# MongoDB Enterprise Edition Analytics Add-On
{: #mongodbee-analytics}

The {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Analytics Add-On allows you to execute long-running analytical queries and/or provision a [MongoDB Connector for business intelligence(BI)](https://docs.mongodb.com/bi-connector/current/) to make your query data compatible with BI tools, such as [Tableau](https://www.tableau.com/).

The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On is made up of two components:
- [The Analytics node](#mongodbee-analytics-node)
- [The connector for Business Intelligence (BI)](#mongodbee-analytics-connector-bi)

## What problems does the {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On solve?
{: #mongodbee-analytics-how-problem}

1. **Most BI tools do not work with the MongoDB document data model.** 

    MongoDB's document data model is made up of complex documents with arbitrary, nested data. This schema makes storing data flexible, easy, and scalable. However, most BI tools require data to be in tabular format, which is rigidly defined and stored in tables, not documents. The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On converts MongoDB document data into SQL-readable tabular data that can be queried and displayed by BI tools.

1. **BI queries are expensive and can degrade database performance.** 

    Long-running queries can negatively impact the operational workflow of your deployment. The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On introduces an extra data member, which isolates analytics workloads from operational workloads. The Analytics Node data is kept in sync with the other nodes, so any queries performed against it produce the same results.


### The Analytics Node
{: #mongodbee-analytics-node}

The Analytics node isolates analytics from operational workload, allowing for long-running queries that do not impact operational workflow performance. You can use the Analytics node directly using MongoDB queries, or through the BI Connector if you want to run SQL queries.

Enabling the Analytics node without engaging the connector for BI allows you to run document-type MongoDB queries or test a query on production data _without_ affecting your application. 
{: .note}

You can access your Analytics node directly through a connection string, e.g.:

```shell
mongodb://$USERNAME:$PASSWORD@host-0:30783,host-1:30783,host-2:30783/?readPreference=secondary&readPreferenceTags=nodeType%3AANALYTICS&replicaSet=replset
```

### The connector for BI
{: #mongodbee-analytics-connector-bi}

Traditional BI tools are designed to work with tabular, row-and-column data. The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On connector for BI allows you to query MongoDB data with SQL using tools, such as Tableau, by connecting to the Analytics node and providing an SQL interface.

You can access your BI Connector through the ODBC connector of your BI tool by using your username and password and the host URL, which will be something like:

```shell
xyz1234-scfr5rer-496hjgo6ghtg-biconnector.abc12345deft7.databases.appdomain.cloud:32757
```

The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On connector for BI cannot be enabled without the Analytics node.
{: .important}

## MongoDB EE Analytics Add-On Considerations
{: #mongodbee-analytics-consider}

Before taking advantage of the {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On, consider the following:

- The add-on is available only for {{site.data.keyword.databases-for-mongodb}} EE.
- Once it's enable, you cannot deprovision the analytics node.
- You cannot scale the disk space of the Analytics member. 
  
    Scaling the disk space of the main database members will result in proportional scaling of the Analytics member.
    {: .important}

## Provisioning an Analytics Node and BI Connector
{: #mongodbee-analytics-node-provisioning}

### Provisioning using Terraform
{: #mongodbee-analytics-node-provisioning-terraform}

Analytics Node and BI Connector are `group` attributes that can be added to a Terraform script. For an example of how to add these to a {{site.data.keyword.databases-for-mongodb}} EE deployment (and obtain the connection strings to access them) [see this Terraform documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database#sample-database-instance-by-using-group-attributes). 


### Provisioning through the {{site.data.keyword.cloud_notm}} Databases API
{: #mongodbee-analytics-node-provisioning-api}

Provisioning via the API is a two-step process: 

1. [Create](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance) a {{site.data.keyword.databases-for-mongodb}} EE deployment.

2. Next, add the Analytics Node and BI Connector `group` to your deployment by using the [Update Resource Instance](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#update-resource-instance) method.

Example for Analytics Node:

```shell
curl --request PATCH \
  --url https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/analytics \
  --header 'Authorization: Bearer <> \
  --header 'Content-Type: application/json' \
  --data '{
    "group": {
        "members": {
            "allocation_count": 1
        }
    }
}'
```

Example for BI Connector:

```shell
curl --request PATCH \
  --url https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/bi_connector \
  --header 'Authorization: Bearer <> \
  --header 'Content-Type: application/json' \
  --data '{
    "group": {
        "members": {
            "allocation_count": 1
        }
    }
}'
```
Remember that the Analytics Node has to be scaled **before** the BI Connector or your request will fail. {: .important}

To get the connection strings to connect to the Analytics Node and/or BI Connector, follow the instructions [here](https://cloud.ibm.com/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings).

