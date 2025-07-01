---
copyright:

  years: 2025, 2025
lastupdated: "2025-07-01"

keywords: databases, mongodbee, Enterprise Edition, sharding, horizontal scaling

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise Edition sharding
{: #mongodbee-sharding}

{{site.data.keyword.databases-for-mongodb_full}} EE (Enterprise Edition) Sharding allows you to distribute data across multiple machines.
{: beta}

## What problems does {{site.data.keyword.databases-for-mongodb}} EE sharding solve?
{: #mongodbee-sharding-how-problem}

When increased demand or workload requires database scaling, you can scale vertically or horizontally. Vertical scaling, adding additional CPUs or RAM, alleviates increased workload but has physical limitations because vertical scaling requires physical hardware. {{site.data.keyword.databases-for}} supports a maximum of 4 TB of storage. An alternative to scaling vertically is [sharding](https://www.mongodb.com/docs/v4.4/sharding/){: external}, or horizontal scaling. Horizontal scaling adds cluster nodes, enabling data to be distributed as shards among the nodes. This distribution allows you to scale proportionally without the same physical limitations as vertical scaling. Scaling horizontally, instead of vertically, allows for much greater infrastructure growth and flexibility.

## MongoDB EE sharding considerations
{: #mongodbee-sharding-consider}

- {{site.data.keyword.databases-for-mongodb}} EE sharding requires [{{site.data.keyword.databases-for}} Isolated Compute](docs/cloud-databases?topic=cloud-databases-hosting-models){: external}.
- Sharding is not only an infrastructure operation. It is a shared responsibility between the provider and the customer. {{site.data.keyword.databases-for}} is responsible for provisioning nodes according to user needs, scaling vertically or horizontally. You are expected to:
   - [Enable sharding in each of the databases](#mongodbee-sharding-enable-sharding-databases) in your {{site.data.keyword.databases-for-mongodb}} EE Sharding instance.
   - [Enable sharding in each of the collections](#mongodbee-sharding-enable-sharding-collections) of a database. Unsharded collections get stored in a single shard. These unbalanced nodes can get full before others and cause operational problems.
   - Choose a suitable [shard key](#mongodbee-sharding-enable-sharding-collections-shard-key) for each collection to avoid overloading some shards when retrieving data.
   - Optimize querying to avoid scatter/gather operations across shards that are more time-consuming and computationally expensive.

   For more information, see [Responsibilities for {{site.data.keyword.databases-for}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-responsibilities-cloud-databases).
   {: tip}

- {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) sharding is only for versions of MongoDB EE >= 5.0.
- Additional infrastructure does come with additional cost. For more information, see [Pricing](/docs/databases-for-mongodb?topic=databases-for-mongodb-pricing).
- Once added, shards cannot be removed. Plan your scaling strategy accordingly.
- Existing unsharded deployments cannot be converted to sharded.

## Provisioning with {{site.data.keyword.databases-for-mongodb}} EE sharding
{: #mongodbee-sharding-node-provisioning}

### Provision through the UI
{: #mongodbee-sharding-node-provisioning-ui}
{: ui}

Provision through the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mongodb){: external}.

- Within **Service configuration**, find the **Database edition** menu. 
- Select *Enterprise sharding*.

{{site.data.keyword.databases-for-mongodb}} EE sharding requires {{site.data.keyword.databases-for}} Isolated Compute. To configure Isolated Compute, follow the appropriate steps at [Hosting Models](docs/cloud-databases?topic=cloud-databases-hosting-models){: external}.
{: requirement}

Provision through the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mongodb){: external}.

- Within **Service Configuration**, find the **Database Edition** menu.
- Select *Enterprise sharding*.

### Provision through Terraform
{: #mongodbee-sharding-node-provisioning-terraform}
{: terraform}

{{site.data.keyword.databases-for-mongodb}} EE sharding requires {{site.data.keyword.databases-for}} Isolated Compute. To configure Isolated Compute, follow the appropriate steps at [Hosting Models](docs/cloud-databases?topic=cloud-databases-hosting-models){: external}.
{: requirement}

{{site.data.keyword.databases-for-mongodb}} EE sharding is a `plan` attribute that can be added to a Terraform script. The plan name is `enterprise-sharding`.

#### Isolated Compute
{: #mongodbee-sharding-node-provisioning-terraform-iso-compute}

Through dedicated computing resources and security, Isolated Compute is the service for secure enterprise data hosting.

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

For more information, see [Terraform documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external}.

### Provisioning through the {{site.data.keyword.cloud_notm}} Databases API
{: #mongodbee-sharding-node-provisioning-api}
{: api}

{{site.data.keyword.databases-for-mongodb}} EE sharding requires {{site.data.keyword.databases-for}} Isolated Compute. To configure Isolated Compute, follow the appropriate steps at [Hosting Models](docs/cloud-databases?topic=cloud-databases-hosting-models){: external}.
{: requirement}

Follow these steps to provision using the [Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller){: external}.

1. Obtain an [IAM token from your API token](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#authentication){: external}.
1. You need to know the ID of the resource group that you would like to provision into. This information is available through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_groups).

   Use a command like:
   
   ```sh
   ibmcloud resource groups
   ```
   {: pre}

1. You need to know the region you would like to provision into.

   To list all of the regions that instances can be provisioned into from the current region, use the [{{site.data.keyword.databases-for}} CLI plug-in](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference){: external}.

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

## Configure sharding
{: #mongodbee-sharding-config}

### Enable sharding in your databases and collections
{: #mongodbee-sharding-enable-sharding-databases}

For database and collection creation, a useful GUI tool is [MongoDB Compass](https://www.mongodb.com/products/tools/compass){: external}.
{: tip}

First, create a database. Then, choose your **Database name** and **Collection name**.

Now, enable sharding in your databases and collections using the [MongoDB Shell, `mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-shell--mongosh-){: external}. For more information, see [MongoDB sh.enableSharding](https://www.mongodb.com/docs/manual/reference/method/sh.enableSharding/#sh.enablesharding){: external} and [MongoDB sh.shardCollection](https://www.mongodb.com/docs/manual/reference/method/sh.shardCollection/#sh.shardcollection){: external}

### Your MongoDB shard key
{: #mongodbee-sharding-enable-sharding-collections-shard-key}

A [shard key](https://www.mongodb.com/docs/manual/core/sharding-shard-key/#shard-keys){: external} is a field in a MongoDB document that is used to distribute the document across multiple shards. The [cardinality of a shard key](https://www.mongodb.com/docs/v5.0/core/sharding-choose-a-shard-key/#shard-key-cardinality){: external} is crucial to the effectiveness of horizontal scaling in the cluster. High cardinality allows MongoDB to distribute documents evenly throughout the cluster.

For more information, see [shard keys](https://www.mongodb.com/docs/manual/core/sharding-shard-key/#shard-keys){: external}.

### {{site.data.keyword.databases-for-mongodb}} EE sharding connection strings
{: #mongodbee-sharding-connection-string}

The `mongos` router is the interface between a MongoDB cluster and applications. Because {{site.data.keyword.databases-for-mongodb}} EE Sharding clusters connect to applications through the `mongos` router, instead of the directly to the cluster members, the connection strings are different than they would be for a standard {{site.data.keyword.databases-for}} instance.

### Scaling shards in {{site.data.keyword.databases-for-mongodb}} EE sharding
{: #mongodbee-sharding-scaling}

## Scaling shards via the CLI
{: #mongodbee-sharding-scaling-cli}
{: cli}

Use the {{site.data.keyword.databases-for}} CLI to provision a new shard group. This operation adds compute and storage resources to your existing sharded cluster.
The following steps outline the commands for adding shards to a {{site.data.keyword.databases-for-mongodb}} EE sharding cluster using the CLI.

```sh
# TODO: Fix with right commands
ibmcloud cdb deployment-group-create <deployment_name> --group-id shard --members 1 --cpu <cpu_units> --memory <memory_mb> --disk <disk_mb>
```
{: pre}

Once the command completes, verify that the shard joined the cluster by running:

```sh
# TODO: Insert the CLI command to list or describe the shards in the deployment
ibmcloud cdb deployment-show <deployment_name>
```
{: pre}

Review the output to confirm that the new shard is marked as active and healthy.

## Scaling shards via the API
{: #mongodbee-sharding-scaling-api}
{: api}

Programmatically add shards by sending a `POST` request to the {{site.data.keyword.databases-for}} API. The JSON payload defines the size and count of the new shard nodes. Replace the placeholders with your deployment ID and resource specifications. ## todo- step count

```sh
# TODO: get right post command
curl -X POST \
  https://cloud.ibm.com/databases/<deployment_id>/groups \
  -H "Authorization: Bearer <IAM_token>" \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "shard",
    "members": 1,
    "cpu": { "allocation_count": <cpu_units> },
    "memory": { "allocation_mb": <memory_mb> },
    "disk": { "allocation_mb": <disk_mb> }
  }'
'
```
{: pre}

After the request succeeds, check the deployment details to ensure the new shard is part of the cluster:

```ssh
# TODO: Fix with right commands
curl -X GET \
  https://cloud.ibm.com/databases/<deployment_id> \
  -H "Authorization: Bearer <IAM_token>"
```
{: pre}

## Scaling shards via Terraform
{: #mongodbee-sharding-scaling-terraform}
{: terraform}

When managing infrastructure as code, modify your Terraform configuration through [`ibm_database` Resource for Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}. Use Terraform to instruct {{site.data.keyword.cloud}} to allocate resources for the additional shard. Use the following template to add an additional shard group to your existing Terraform configuration. 

```terraform
# TODO: fix with right params
resource "ibm_database" "mongodb_sharded" {
  name              = "my-mongodb-sharded"
  service           = "databases-for-mongodb"
  plan              = "enterprise-sharding"
  location          = "us-south"
  resource_group_id = data.ibm_resource_group.default.id
  adminpassword     = "<your_admin_password>"

  # Existing shard groups (if any) remain here
  # group {
  #   group_id = "shard"
  #   members  = 1
  #
  #   cpu {
  #     allocation_count = <existing_cpu_units>
  #   }
  #
  #   memory {
  #     allocation_mb = <existing_memory_mb>
  #   }
  #
  #   disk {
  #     allocation_mb = <existing_disk_mb>
  #   }
  # }

  # Add a new shard group
  group {
    group_id = "shard"
    members  = 1

    cpu {
      allocation_count = <new_cpu_units>
    }

    memory {
      allocation_mb = <new_memory_mb>
    }

    disk {
      allocation_mb = <new_disk_mb>
    }
  }
}

```
{: pre}

Monitor the Terraform output to confirm that the additional shard was created and integrated into your sharded cluster.

By following these guidelines and utilizing the provided methods, you can effectively scale your {{site.data.keyword.databases-for-mongodb}} EE sharding clusters on {{site.data.keyword.cloud}} to meet your application's demands.
