---
copyright:
  years: 2020, 2025
lastupdated: "2025-02-27"

keywords: databases, opsman, mongodbee, Enterprise Edition, analytics, bi connector

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise Edition Analytics Add-On
{: #mongodbee-analytics}

Analytics Add-On for {{site.data.keyword.databases-for-mongodb}} Enterprise Edition is deprecated after March 31,2025.
{: deprecated}

The {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Analytics Add-On allows you to run long-running analytical queries or provision a [MongoDB Connector for business intelligence(BI)](https://docs.mongodb.com/bi-connector/current/){: external} to make your query data compatible with BI tools, such as [Tableau](https://www.tableau.com/){: external}.

The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On is made up of two components:
- [The Analytics node](#mongodbee-analytics-node)
- [The connector for Business Intelligence (BI)](#mongodbee-analytics-connector-bi)

## What problems does the {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On solve?
{: #mongodbee-analytics-how-problem}

1. **Most BI tools do not work with the MongoDB document data model.** 

    MongoDB's document data model is made up of complex documents with arbitrary, nested data. This schema makes storing data flexible, easy, and scalable. However, most BI tools require data to be in tabular format, which is rigidly defined and stored in tables, not documents. The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On converts MongoDB document data into SQL-readable tabular data that can be queried and displayed by BI tools.

1. **BI queries are expensive and can degrade database performance.**

    Long-running queries can negatively impact the operational workflow of your deployment. The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On introduces an extra data member, which isolates analytics workloads from operational workloads. The Analytics Node data is kept in sync with the other nodes, so any queries run against it produce the same results.


### The Analytics node
{: #mongodbee-analytics-node}

The Analytics node isolates analytics from operational workload, allowing for long-running queries that do not impact operational workflow performance. You can use the Analytics node directly by using MongoDB queries, or through the BI Connector if you want to run SQL queries.

Enabling the Analytics node without engaging the connector for BI allows you to run document-type MongoDB queries or test a query on production data _without_ affecting your application.
{: .note}

You can access your Analytics node directly through a connection string, for example:

```sh
mongodb://$USERNAME:$PASSWORD@host-0:30783,host-1:30783,host-2:30783/?readPreference=secondary&readPreferenceTags=nodeType%3AANALYTICS&replicaSet=replset
```

### The connector for BI
{: #mongodbee-analytics-connector-bi}

Traditional BI tools are designed to work with tabular, row-and-column data. The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On connector for BI allows you to query MongoDB data with SQL using tools, such as Tableau, by connecting to the Analytics node and providing an SQL interface.

You can access your BI Connector through the ODBC connector of your BI tool by using your username and password and the host URL, which looks like:

```sh
xyz1234-scfr5rer-496hjgo6ghtg-biconnector.abc12345deft7.databases.appdomain.cloud:32757
```

The {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On connector for BI cannot be enabled without the Analytics node.
{: .important}

## MongoDB EE Analytics Add-On considerations
{: #mongodbee-analytics-consider}

Before taking advantage of the {{site.data.keyword.databases-for-mongodb}} EE Analytics Add-On, consider the following:

- The add-on is available only for {{site.data.keyword.databases-for-mongodb}} EE.
- Once enabled, you cannot deprovision the analytics node.
- You cannot scale the disk space of the Analytics member.
- An Analytics node is priced as a data member. For more information on {{site.data.keyword.databases-for-mongodb}} EE, see [Pricing](/docs/databases-for-mongodb?topic=databases-for-mongodb-pricing&interface=api).

    Scaling the disk space of the main database members results in proportional scaling of the Analytics member.
    {: .important}

## Provisioning an Analytics node and BI connector
{: #mongodbee-analytics-node-provisioning}

### Provision using Terraform
{: #mongodbee-analytics-node-provisioning-terraform}
{: terraform}

Analytics Node and BI Connector are `group` attributes that can be added to a Terraform script. You can see an example here:

```terraform
data "ibm_resource_group" "test_acc" {
  is_default = true
}

resource "ibm_database" "mongodb_enterprise" {
  resource_group_id = data.ibm_resource_group.test_acc.id
  name              = "test"
  service           = "databases-for-mongodb"
  plan              = "enterprise"
  location          = "us-south"
  adminpassword     = "password12"
  tags              = ["one:two"]

  group {
    group_id = "member"

    host_flavor {
      id = "b3c.8x32.encrypted"
    }

    disk {
      allocation_mb = 256000
    }
  }

  group {
    group_id = "analytics"

    members { 
      allocation_count = 1
    }
  }

  group {
    group_id = "bi_connector"

    members { 
      allocation_count = 1
    }
  }

  timeouts {
    create = "120m"
    update = "120m"
    delete = "15m"
  }
}

data "ibm_database_connection" "mongodb_conn" {
  deployment_id = ibm_database.mongodb_enterprise.id
  user_type     = "database"
  user_id       = "admin"
  endpoint_type = "public"
}

output "bi_connector_connection" {
  description = "BI Connector connection string"
  value       = data.ibm_database_connection.mongodb_conn.bi_connector.0.composed.0
}

output "analytics_connection" {
  description = "Analytics Node connection string"
  value       = data.ibm_database_connection.mongodb_conn.analytics.0.composed.0
}
```

For more information, see our [Terraform documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database#sample-mongodb-enterprise-database-instance-with-bi-connector-and-analytics){: external}. 

Remember that the Analytics Node must be scaled **before** the BI Connector or your request will fail. {: .important}

### Provisioning through the {{site.data.keyword.cloud_notm}} Databases API
{: #mongodbee-analytics-node-provisioning-api}
{: api}

Provisioning via the API is a two-step process: 
1. [Create](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning) a {{site.data.keyword.databases-for-mongodb}} EE deployment.
2. After that you can add the Analytics Node and BI Connector `group` to your deployment by using the [Scale Group](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#setdeploymentscalinggroup) method.

Example for Analytics Node:

```sh
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

```sh
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
Remember that the Analytics Node must be deployed **before** the BI Connector or your request will fail. {: .important}

To get the connection strings to connect to the Analytics Node and/or BI Connector, follow the instructions [here](https://cloud.ibm.com/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings).

## Connect using BI tools
{: #mongodbee-analytics-node-connecting-bi-tools}

To connect to the MongoDB Connector for BI using popular BI tools, see:
* [Microsoft Power BI Desktop](https://www.mongodb.com/docs/bi-connector/current/connect/powerbi/){: external}
* [Tableau](https://help.tableau.com/current/pro/desktop/en-us/examples_mongodb.htm){: external}
