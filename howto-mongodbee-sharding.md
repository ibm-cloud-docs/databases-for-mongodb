---
copyright:
  years: 2020, 2023
lastupdated: "2023-04-11"

keywords: databases, mongodbee, Enterprise Edition, sharding, horizontal scaling

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise Edition Sharding
{: #mongodbee-sharding}

{{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Sharding allows you to distribute data across multiple machines.

## What problems does {{site.data.keyword.databases-for-mongodb}} EE Sharding solve?
{: #mongodbee-sharding-how-problem}

1. **Issue 1** 


1. **Issue 2** 

## MongoDB EE Sharding Add-On Considerations
{: #mongodbee-sharding-consider}

## Provisioning with {{site.data.keyword.databases-for-mongodb}} EE Sharding
{: #mongodbee-sharding-node-provisioning}

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
  description = "BI Connector connection string"
  value       = data.ibm_database_connection.mongodb_conn.sharding.0.composed.0
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
1. [Create](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance) a {{site.data.keyword.databases-for-mongodb}} EE deployment.
2. After that you can add a MongoDB sharding `group` to your deployment by using the [Scale Group](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#setdeploymentscalinggroup) method.

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

To get the connection strings to connect to the MongoDB EE Shard, follow the instructions [here](https://cloud.ibm.com/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings).
