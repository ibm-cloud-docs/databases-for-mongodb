---

copyright:
  years: 2025
lastupdated: "2025-10-23"

keywords: databases, mongodbee, Enterprise Edition, sharding, horizontal scaling

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Scaling with MongoDB EE sharding
{: #enterprise-sharding-scaling}

## Scaling shards via the CLI
{: #enterprise-sharding-scaling-shards}
{: cli}

Use the {{site.data.keyword.databases-for}} CLI to provision a new shard group. This operation adds compute and storage resources to your existing sharded cluster. The following steps outline the commands for adding shards to a {{site.data.keyword.databases-for-mongodb}} EE sharding cluster using the CLI.

```sh
ibmcloud cdb deployment-groups-set <instance_crn> member -b 6

...

Status                completed
Progress Percentage   100
Location              https://api.test.us-south.databases.cloud.ibm.com/v5/ibm/deployments/<instance_crn>::
OK
```
{: pre}

In the following example, the `member` allocation count is set to 6. The value must meet the following criteria:

- Minimum: 3 
- Maximum: 18
- Step size: Must be a multiple of 3 (for example, 3, 6, 9, and so on). Values, such as 5 or 7 are not valid.

You cannot reduce the allocation count after provisioning.
{: note}

## Scaling shards via the API
{: #mongodbee-sharding-scaling}
{: api}

Programmatically add shards by sending a `PATCH` request to the {{site.data.keyword.databases-for}} API. Use the JSON payload to define the member allocation count and resource configuration for the new shard group.

```sh

curl -X PATCH
  https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/members
  -H 'Authorization: Bearer <>'
  -H 'Content-Type: application/json'
  -d '{
    "members": {
      "allocation_count": 6
    }
  }'

```
{: pre}

To configure the number of shards, set the `allocation_count` parameter in your deployment configuration. The value must meet the following criteria:

- Minimum: 3 
- Maximum: 18
- Step size: Must be a multiple of 3 (for example, 3, 6, 9, and so on). Values, such as 5 or 7 are not valid.

If this parameter is not specified, it will default to the minimum value of 3 members.

You cannot reduce the allocation count after provisioning.
{: note}

## Scaling shards via Terraform
{: #mongodbee-sharding-scaling}
{: terraform}

When managing infrastructure as code, modify your Terraform configuration through the [`ibm_database` Resource for Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}. Use Terraform to instruct {{site.data.keyword.cloud}} to allocate resources for the additional shard. 

Use the following template to add an additional shard group to your existing Terraform configuration. 

```terraform

resource "ibm_database" "mongodbees_example" {
   name                 = "mongodbees_example"
   location             = "us-south"
   plan                 = "enterprise-sharding"
   service              = "databases-for-mongodb"
   service_endpoints    = "private"

    group {
    group_id = "member"

    members {
      allocation_count = 6
    }

    host_flavor {
      id = "b3c.4x16.encrypted"
    }
  }
 }

```
{: pre}

To configure the number of shards, set the `group.members.allocation_count` parameter in your deployment configuration. The value must meet the following criteria:
  
- Minimum: 3 
- Maximum: 18
- Step size: Must be a multiple of 3 (for example, 3, 6, 9, and so on). Values, such as 5 or 7 are not valid.

If this parameter is not specified, it will default to the minimum value of 3 members.

You cannot reduce the allocation count after provisioning.
{: note}
