---
copyright:
  years: 2020, 2023
lastupdated: "2023-09-21"

keywords: databases, mongodbee, Enterprise Edition, sharding, horizontal scaling

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise Edition Sharding
{: #mongodbee-sharding}

{{site.data.keyword.databases-for-mongodb_full}} EE (Enterprise Edition) Sharding allows you to distribute data across multiple machines.
{: beta}

## What problems does {{site.data.keyword.databases-for-mongodb}} EE Sharding solve?
{: #mongodbee-sharding-how-problem}

When increased demand or workload requires database scaling, you can scale vertically or horizontally. Vertical scaling, adding additional CPUs or RAM, alleviates increased workload but has physical limitations because vertical scaling requires physical hardware. {{site.data.keyword.databases-for}} supports a maximum of 4 TB of storage. An alternative to scaling vertically is [sharding](https://www.mongodb.com/docs/v4.4/sharding/){: external}, or horizontal scaling. Horizontal scaling adds cluster nodes, enabling data to be distributed as shards among the nodes. This distribution allows you to scale proportionally without the same physical limitations as vertical scaling. Scaling horizontally, instead of vertically, allows for much greater infrastructure growth and flexibility.

## MongoDB EE Sharding Add-On Considerations
{: #mongodbee-sharding-consider}

- Sharding is not only an infrastructure operation. It is a shared responsibility between the provider and the customer. {{site.data.keyword.databases-for}} is responsible for deploying nodes according to user needs, scaling vertically or horizontally. You are expected to:
   - [Enable sharding in each of the databases](#mongodbee-sharding-enable-sharding-databases) in your {{site.data.keyword.databases-for-mongodb}} EE Sharding deployment.
   - [Enable sharding in each of the collections](#mongodbee-sharding-enable-sharding-collections) of a database. Unsharded collections get stored in a single shard. These unbalanced nodes can get full before others and cause operational problems.
   - Choose a suitable [shard key](#mongodbee-sharding-enable-sharding-collections-shard-key) for each collection to avoid overloading some shards when retrieving data.
   - Optimize querying to avoid scatter/gather operations across shards that are more time-consuming and computationally expensive.

   For more information, see [Responsibilities for {{site.data.keyword.databases-for}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-responsibilities-cloud-databases).
   {: tip}

- {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Sharding is only for versions of MongoDB EE >= 5.0.
- Additional infrastructure does come with additional cost. For more information, see [Pricing](/docs/databases-for-mongodb?topic=databases-for-mongodb-pricing).
- After creation, shards cannot be deleted.
- Existing unsharded clusters cannot be converted to sharded.

## Provisioning with {{site.data.keyword.databases-for-mongodb}} EE Sharding
{: #mongodbee-sharding-node-provisioning}

### Provision through the UI
{: #mongodbee-sharding-node-provisioning-ui}
{: ui}

Provision through the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mongodb){: external}. 
- Within **Service Configuration**, find the **Database Edition** menu. 
- Select *Enterprise Sharding*.

For more information, see [Provisioning](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning).

### Provision through Terraform
{: #mongodbee-sharding-node-provisioning-terraform}
{: terraform}

{{site.data.keyword.databases-for-mongodb}} EE Sharding is a `group` attribute that can be added to a Terraform script. See an example here:

```terraform
data "ibm_resource_group" "test_acc" {
  is_default = true
}

resource "ibm_database" "mongodb_enterprise" {
  resource_group_id = data.ibm_resource_group.test_acc.id
  name              = "test"
  service           = "databases-for-mongodb"
  plan              = "enterprise-sharding"
  location          = "us-south"
  adminpassword     = "password12"
  tags              = ["one:two"]

  group {
    group_id = "members"
    group_shard_id = "itsashard"

    memory { 
      allocation_mb = 24576
    }

    disk { 
      allocation_mb = 122880
    }

    cpu {
      allocation_count = 6
    }
  }
  timeouts {
    create = "120m"
    update = "120m"
    delete = "15m"
  }
}

data "ibm_database_connection" "mongodb_shard" {
  deployment_id = ibm_database.mongodb_enterprise.id
  user_type     = "database"
  user_id       = "admin"
  endpoint_type = "public"
}

output "sharding_connection" {
  description = "Sharding connection string"
  value       = data.ibm_database_connection.mongodb_conn.sharding.0.composed.0
}
```
{: codeblock}

For more information, see [Terraform documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external}. 

### Provisioning through the {{site.data.keyword.cloud_notm}} Databases API
{: #mongodbee-sharding-node-provisioning-api}
{: api}

Follow these steps to provision using the [Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller){: external}.

1. Obtain an [IAM token from your API token](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#authentication){: external}.
1. You need to know the ID of the resource group that you would like to deploy to. This information is available through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_groups).

   Use a command like: 
   ```sh
   ibmcloud resource groups
   ```
   {: pre}

1. You need to know the region you would like to deploy to.

   To list all of the regions that deployments can be provisioned into from the current region, use the [{{site.data.keyword.databases-for}} CLI plug-in](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference){: external}. 
   
   The command looks like: 

   ```sh
   ibmcloud cdb regions --json
   ```
   {: pre}


   Once you have all the information, [provision a new resource instance](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance){: external} with the {{site.data.keyword.cloud_notm}} Resource Controller.
   
   ```sh
   curl -X POST \
     https://resource-controller.cloud.ibm.com/v2/resource_instances \
     -H 'Authorization: Bearer <>' \
     -H 'Content-Type: application/json' \
       -d '{
       "name": "my-instance",
       "target": "blue-us-south",
       "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
       "resource_plan_id": "databases-for-mongodb-enterprise-sharding"
     }'
   ```
   {: .pre}

   The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required.
   {: required}

## Configure Sharding
{: #mongodbee-sharding-config}

### Enable sharding in your databases and collections
{: #mongodbee-sharding-enable-sharding-databases}

For database and collection creation, a useful GUI tool is [MongoDB Compass](https://www.mongodb.com/products/tools/compass){: external}.

Start by creating a database. Choose your **Database Name** and **Collection Name**.

To enable sharding in your databases and collections, use the [MongoDB Shell, `mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-shell--mongosh-){: external}.

### Enable sharding in your databases 
{: #mongodbee-sharding-enable-sharding-databases}

By default, all databases in MongoDB are not sharded. Enable sharding for each database with a command like:

```sh
sh.enableSharding("DATABASE_NAME")
```
{: pre}

For more information, see [MongoDB sh.enableSharding()](https://www.mongodb.com/docs/manual/reference/method/sh.enableSharding/#sh.enablesharding--){: external}.

### Enable sharding in your collections
{: #mongodbee-sharding-enable-sharding-collections}

By default, all collections in MongoDB are not sharded. To use sharding, enable sharding for each collection with a command like:

```sh
sh.shardCollection("namespace",{key}}
```
{: pre}

The parameters for the `shardCollection` command are:
- `namespace`: The namespace of the collection to shard in the form `"<database>.<collection>"`.
- `key`: The document that specifies the field or fields to use as the shard key.

For more information, see [MongoDB sh.shardCollection()](https://www.mongodb.com/docs/manual/reference/method/sh.shardCollection/#sh.shardcollection--){: external}.

#### Your MongoDB Shard Key
{: #mongodbee-sharding-enable-sharding-collections-shard-key}

A [shard key](https://www.mongodb.com/docs/manual/core/sharding-shard-key/#shard-keys){: external} is a field in a MongoDB document that is used to distribute the document across multiple shards. The [cardinality of a shard key](https://www.mongodb.com/docs/v5.0/core/sharding-choose-a-shard-key/#shard-key-cardinality){: external} is crucial to the effectiveness of horizontal scaling in the cluster. High cardinality allows MongoDB to distribute documents evenly throughout the cluster.

For more information, see [Shard Keys](https://www.mongodb.com/docs/manual/core/sharding-shard-key/#shard-keys){: external}.

### {{site.data.keyword.databases-for-mongodb}} EE Sharding Connection Strings
{: #mongodbee-sharding-connection-string}

The `mongos` router is the interface between a MongoDB cluster and applications. Because {{site.data.keyword.databases-for-mongodb}} EE Sharding clusters connect to applications through the `mongos` router, instead of the directly to the cluster members, the connection strings are different than they would be for a standard {{site.data.keyword.databases-for}} deployment.
