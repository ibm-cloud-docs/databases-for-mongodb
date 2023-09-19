---
copyright:
  years: 2020, 2023
lastupdated: "2023-09-19"

keywords: databases, mongodbee, Enterprise Edition, sharding, horizontal scaling

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise Edition Sharding
{: #mongodbee-sharding}

{{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Sharding allows you to distribute data across multiple machines.

## What problems does {{site.data.keyword.databases-for-mongodb}} EE Sharding solve?
{: #mongodbee-sharding-how-problem}

When increased demand or workload requires database scaling, you can scale vertically or horizontally. Vertical scaling, adding additional CPUs or RAM, alleviates increased workload but has physical limitations because vertical scaling requires physical hardware. {{site.data.keyword.databases-for}} supports a maximum of 4TB of storage. An alternative to scaling vertically is [sharding](https://www.mongodb.com/docs/v4.4/sharding/){: external}, or horizontal scaling. Horizontal scaling adds cluster nodes, enabling data to be distributed as shards among the nodes. This distribution allows you to scale proportionally without the same physical limitations as vertical scaling. Scaling horizontally, instead of vertically, allows for much greater infrastructure growth and flexibility.

## MongoDB EE Sharding Add-On Considerations
{: #mongodbee-sharding-consider}

- Sharding is not only an infrastructure operation. It is a shared customer/provider responsibility. {{site.data.keyword.databases-for}} is responsible for deploying nodes according to user needs, scaling vertically or horizontally. You are expected to:
   - enable sharding in each of the databases in your {{site.data.keyword.databases-for-mongodb}} EE Sharding deployment.
   - enable sharding in each of the collections of a database. Otherwise, unsharded collections get stored in a single shard, therefore unbalancing nodes. These unbalanced nodes may get full before others and cause operational problems.
   - Choose a suitable shard key for each collection to avoid overloading some shards when retrieving data.
   - Optimize querying to try to avoid scatter/gather operations across shards that are more time consuming and computationally expensive.

   For more information, see [Responsibilities for Cloud Databases](/docs/databases-for-mongodb?topic=databases-for-mongodb-responsibilities-cloud-databases).
   {: tip}

- {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Sharding is only for versions of MongoDB EE >= 5.0.
- Additional infrastructure does come with additional cost. For more information on pricing, see [Pricing](/docs/databases-for-mongodb?topic=databases-for-mongodb-pricing).
- Once created, shards cannot be deleted.
- Existing unsharded clusters cannot be converted to sharded.

## Provisioning with {{site.data.keyword.databases-for-mongodb}} EE Sharding
{: #mongodbee-sharding-node-provisioning}

### Provision using the UI
{: #mongodbee-sharding-node-provisioning-ui}
{: ui}

Provision through the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mongodb){: external}. 
- Within **Service Configuration**, find the **Database Edition** dropdown menu. 
- Select *Enterprise Sharding*.

For more information on {{site.data.keyword.databases-for-mongodb}} provisioning, see [Provisioning](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning).

### Provision using Terraform
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
  plan              = "enterprise"
  location          = "us-south"
  adminpassword     = "password12"
  tags              = ["one:two"]

  group {
    group_id = "members",
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

For more information, see [Terraform documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external}. 

### Provisioning through the {{site.data.keyword.cloud_notm}} Databases API
{: #mongodbee-sharding-node-provisioning-api}
{: api}

Provisioning via the API is a two-step process: 
1. [Create](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance) a {{site.data.keyword.databases-for-mongodb}} EE Sharded deployment.
2. After your deployment is provisioned, add a MongoDB sharding `group` to your deployment by using the [Scale Group](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#setdeploymentscalinggroup) method.

```sh
curl --request PATCH \
  --url https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/members \
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

### {{site.data.keyword.databases-for-mongodb}} EE Sharding Connection Strings
{: #mongodbee-sharding-connection-string}

The `mongos` router is the interface between a MongoDB cluster and applications. Because {{site.data.keyword.databases-for-mongodb}} EE Sharding clusters connect to applications through the `mongos` router, instead of the directly to the cluster members, the process of getting connection strings is different than it would be for a standard {{site.data.keyword.databases-for}} deployment. 



