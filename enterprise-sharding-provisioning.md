---

copyright:
  years: 2025
lastupdated: "2025-10-23"

keywords: databases, mongodbee, Enterprise Edition, sharding, horizontal scaling

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning with MongoDB EE Sharding
{: #enterprise-sharding-provisioning}

## Provisioning through the {{site.data.keyword.cloud_notm}} console
{: #enterprise-sharding-provisioning}
{: ui}

Provision through the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mongodb){: external}.

- Within **Service configuration**, find the **Database edition** menu. 
- Select *Enterprise Sharding*.

{{site.data.keyword.databases-for-mongodb}} EE Sharding requires {{site.data.keyword.databases-for}} Isolated Compute. 
{: requirement}

## Provisioning through the CLI
{: #enterprise-sharding-provisioning-cli}
{: cli}

Before provisioning, follow the instructions provided in the documentation to install the [{{site.data.keyword.cloud_notm}} CLI tool](/docs/cli?topic=cli-install-ibmcloud-cli){: external}.

1. Log in to {{site.data.keyword.cloud_notm}}. If you use a federated user ID, it's important that you switch to a one-time passcode (`ibmcloud login --sso`), or use an API key (`ibmcloud --apikey key or @key_file`) to authenticate. For more information about how to log in by using the CLI, see [General CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login){: external} under `ibmcloud login`.

   ```sh
   ibmcloud login
   ```
   {: pre}

{{site.data.keyword.databases-for-mongodb}} EE Sharding requires {{site.data.keyword.databases-for}} Isolated Compute.
{: requirement}

2. Provision your database with the following command:

   ```sh
   ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE_NAME> <SERVICE_PLAN_NAME> <LOCATION> <RESOURCE_GROUP> -p '{"members_host_flavor": "<members_host_flavor value>"}' --service-endpoints="<Endpoint>"
   ```
   {: pre}

  For example, to provision a {{site.data.keyword.databases-for-mongodb}} EE Sharding instance, use a command like:
  
  ```sh
   ibmcloud resource service-instance-create test-database databases-for-mongodb-development enterprise-sharding us-south -p '{ "members_allocation_count": 9, "members_host_flavor": "b3c.4x16.encrypted" }' --service-endpoints private
   ```
   {: pre}

For more information about parameters, see [Provisioning](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=cli){: external}.


## Provisioning through Terraform
{: #enterprise-sharding-provisioning}
{: terraform}

{{site.data.keyword.databases-for-mongodb}} EE Sharding requires {{site.data.keyword.databases-for}} Isolated Compute. To configure Isolated Compute, follow [these steps](docs/cloud-databases?topic=cloud-databases-hosting-models){: external}.
{: requirement}

{{site.data.keyword.databases-for-mongodb}} EE Sharding is a `plan` attribute that can be added to a Terraform script. The plan name is `enterprise-sharding`.

### Isolated Compute
{: #enterprise-sharding-provisioning-iso-compute}
{: terraform}

Isolated Compute is a secure single-tenant hosting option that provides dedicated compute resources, storage bandwidth, and hypervisor-level isolation for complex, high-performance enterprise workloads.

{{site.data.keyword.databases-for-mongodb}} EE sharding requires Isolated Compute. To provision with Isolated Compute, add `host_flavor` to your Terraform script, along with the appropriate resource allocation.

The script looks like:

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
    group_id = "member"

    host_flavor {
      id = "b3c.8x32.encrypted"
    }

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
  value       = data.ibm_database_connection.mongodb_shard.mongodb.0.composed.0
}
```
{: codeblock}

If successful, expect an output that looks like:

```text
sharding_connection = "mongodb://admin:$PASSWORD@71dff696-864b-4051-9f3a-db1445567912-r-0.bn5hbied0ao9rn2ced1g.databases.appdomain.cloud:30092,71dff696-864b-4051-9f3a-db1445567912-r-1.bn5hbied0ao9rn2ced1g.databases.appdomain.cloud:30122,71dff696-864b-4051-9f3a-db1445567912-r-2.bn5hbied0ao9rn2ced1g.databases.appdomain.cloud:31235?authMechanism=SCRAM-SHA-256&authSource=admin&tls=true"
```

For more information, see the [Terraform documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external}.

## Provisioning through the {{site.data.keyword.cloud_notm}} Databases API
{: #enterprise-sharding-provisioning}
{: api}

{{site.data.keyword.databases-for-mongodb}} EE sharding requires {{site.data.keyword.databases-for}} Isolated Compute. To configure Isolated Compute, follow [these steps](docs/cloud-databases?topic=cloud-databases-hosting-models){: external}.
{: requirement}

To provision using the [Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller){: external}, perform the following steps.

1. Obtain an [IAM token from your API token](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#authentication){: external}.
1. You need the ID of the resource group that you want to provision into. This information is available through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_groups).

   Use a command like:
   
   ```sh
   ibmcloud resource groups
   ```
   {: pre}

1. You need to know the region you want to provision into.

   To list all of the regions that instances can be provisioned into from the current region, use the [{{site.data.keyword.databases-for}} CLI plug-in](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference){: external}.

   The command looks like:

   ```sh
   ibmcloud cdb regions --json
   ```
   {: pre}

   Once you have all the information, [provision a new resource instance](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance){: external} with the {{site.data.keyword.cloud_notm}} Resource Controller.

   ```sh
   curl -X POST
   https://resource-controller.cloud.ibm.com/v2/resource_instances
   -H 'Authorization: Bearer <>'
   -H 'Content-Type: application/json'
   -d '{
      "name": "my-instance",
      "target": "blue-us-south",
      "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
      "resource_plan_id": "databases-for-mongodb-enterprise-sharding",
      "parameters": {
         "members_allocation_count": 6,
         "members_host_flavor": "b3c.16x64.encrypted"
      }
   }'
   ```
   {: .pre}

   The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required.
   {: required}

## Configuring sharding
{: #enterprise-sharding-config}

### Enabling sharding in your databases and collections
{: #enterprise-sharding-enable-sharding-databases}

For database and collection creation, a useful GUI tool is [MongoDB Compass](https://www.mongodb.com/products/tools/compass){: external}.
{: tip}

First, create a database. Then, choose your **Database name** and **Collection name**.

Next, enable sharding in your databases and collections using the [MongoDB Shell, `mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-shell--mongosh-){: external}. For more information, see [MongoDB sh.enableSharding](https://www.mongodb.com/docs/manual/reference/method/sh.enableSharding/#sh.enablesharding){: external} and [MongoDB sh.shardCollection](https://www.mongodb.com/docs/manual/reference/method/sh.shardCollection/#sh.shardcollection){: external}.

### Your MongoDB shard key
{: #enterprise-sharding-enable-sharding-shard-key}

A [shard key](https://www.mongodb.com/docs/manual/core/sharding-shard-key/#shard-keys){: external} is a field in a MongoDB document that is used to distribute the document across multiple shards. The [cardinality of a shard key](https://www.mongodb.com/docs/v5.0/core/sharding-choose-a-shard-key/#shard-key-cardinality){: external} is crucial to the effectiveness of horizontal scaling in the cluster. High cardinality allows MongoDB to distribute documents evenly throughout the cluster.

For more information, see [shard keys](https://www.mongodb.com/docs/manual/core/sharding-shard-key/#shard-keys){: external}.

### {{site.data.keyword.databases-for-mongodb}} EE sharding connection strings
{: #enterprise-sharding-connection-strings}

The `mongos` router is the interface between a MongoDB cluster and applications. Because {{site.data.keyword.databases-for-mongodb}} EE Sharding clusters connect to applications through the `mongos` router, instead of directly to the cluster members, the connection strings are different than they would be for a non-sharded {{site.data.keyword.databases-for}} instance.
