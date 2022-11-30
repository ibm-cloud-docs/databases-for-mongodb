---
copyright:
  years: 2020, 2022
lastupdated: "2022-12-30"

keywords: databases, opsman, mongodbee, Enterprise Edition, ops manager, pitr, mongodb point-in-time recovery, mongodb pitr, mongodb terraform

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Point-in-time Recovery (PITR)
{: #pitr}

{{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition offers PITR using any timestamp greater than the earliest available recovery point. To discover the earliest recovery point through the API, use the [point-in-time-recovery timestamp endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#capability).

```sh
{
    "point_in_time_recovery_data": {
        "earliest_point_in_time_recovery_time": "2019-09-09T23:16:00Z"
    }
}
```
{: .codeblock}

In this phase, the point-in-time-recovery timestamp endpoint always returns *current time* - *approximately one week*.{: important}

## Recovery
{: #recovery}

Backups are restored to a new deployment. After the new deployment finishes provisioning, your data in the backup file is restored into the new deployment. Backups are also restorable across accounts, but only by using the API and only if the user that is running the restore has access to both the source and destination accounts. 

The new deployment is automatically sized to the same disk and memory allocation as the source deployment at the time of the backup from which you restore. Especially with PITR, that might not be the current size of your deployment. If you need to adjust the resources that are allocated to the new deployment, use the optional fields in the UI, CLI, or API to resize the new deployment. If the deployment is not given enough resources the restore fails, so allocate enough for your data and workload.
{: .important}

While storage and memory are restored to the same as the source deployment, specific instance configurations are not automatically set for the new instance. In this case, rerunning the configuration after a restore might be needed. Note any instance modifications before running the restore (parameters like `shared_buffers`, `max_connections`, `deadlock_timeout`, `archive_timeout`) to ensure accurate settings for the instance after the restore is complete.

Do not delete the source deployment while the backup is restoring. You must wait until the new deployment is provisioned and the backup is restored before deleting the old deployment. Deleting a deployment also deletes its backups. So, not only does the restore fail, but you might not be able to recover the backup.
{: note}

### Recovering through the UI
{: #pitr-ui}
{: ui}

To initiate a PITR, enter the time that you want to restore back to in Coordinated Universal Time.

The point-in-time-recovery timestamp must be formatted as follows: `%Y-%m-%dT%H:%M:%SZ`.
{: important}

To restore to the most recent available time, select that option. Clicking **Restore** brings up the options for your recovery. Enter a name, select the version, region, and allocated resources for the new deployment. Click **Recover** to start the process.

If you use Key Protect and have a key, you must use the CLI to recover, and a command is provided for your convenience.

### Recovery with the CLI
{: #pitr-cli}
{: cli}

The Resource Controller supports provisioning of database deployments, and provisioning and restoring are the responsibility of the Resource Controller CLI. Use the [`resource service-instance-create`](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_instance_create) command.

For PITR, use the `point_in_time_recovery_time` and `point_in_time_recovery_deployment_id` parameters. The `point_in_time_recovery_deployment_id` is the source deployment's ID and `point_in_time_recovery_time` is the timestamp in Coordinated Universal Time you want to restore to. To restore to the latest available point-in-time, use `"point_in_time_recovery_time":" "`.

The point-in-time-recovery timestamp must be formatted as follows: `%Y-%m-%dT%H:%M:%SZ`.
{: important}

```sh
ibmcloud resource service-instance-create <SERVICE_INSTANCE_NAME> <service-id> <region> -p '{"point_in_time_recovery_deployment_id":"DEPLOYMENT_ID", "point_in_time_recovery_time":"TIMESTAMP", "version":" "}'
```
{: pre}

A pre-formatted command for a specific backup or PITR is available in detailed view of the backup.
{: .tip}

When restoring through the CLI, optional parameters are available. Use them to customize resources or use a Key Protect key for BYOK encryption on the new deployment.
```sh
ibmcloud resource service-instance-create <SERVICE_INSTANCE_NAME> <service-id> standard <region> <--service-endpoints SERVICE_ENDPOINTS_TYPE> -p
'{"point_in_time_recovery_deployment_id":"DEPLOYMENT_ID", "point_in_time_recovery_time":"TIMESTAMP","key_protect_key":"KEY_PROTECT_KEY_CRN", "members_disk_allocation_mb":"DESIRED_DISK_IN_MB", "members_memory_allocation_mb":"DESIRED_MEMORY_IN_MB", "members_cpu_allocation_count":"NUMBER_OF_CORES", "version":" "}'
```
{: pre}

### Recovery with the API
{: #pitr-api}
{: api}

The Resource Controller supports provisioning of database deployments, and provisioning and restoring are the responsibility of the Resource Controller API. You need to complete [the necessary steps to use the resource controller API](/docs/databases-for-postgresql?topic=cloud-databases-provisioning#provisioning-through-the-resource-controller-api) before you can use it to restore from a backup. 

After you have all the information, the create request is a `POST` to the [`/resource_instances`](https://{DomainName}/apidocs/resource-controller#create-provision-a-new-resource-instance) endpoint.

```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "<SERVICE_INSTANCE_NAME>",
    "target": "<region>",
    "resource_group": "<your-resource-group>",
    "resource_plan_id": "<service-id>"
    "point_in_time_recovery_time":"<TIMESTAMP>",
    "point_in_time_recovery_deployment_id":"<DEPLOYMENT_ID>"
  }'
```
{: pre}

The point-in-time-recovery timestamp must be formatted as follows: `%Y-%m-%dT%H:%M:%SZ`.
{: important}

The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. The `target` is the region where you want the new deployment to be located, which can be a different region from the source deployment. Cross region restores are supported, except for restoring a `eu-de` backup to another region.

For PITR, use the `point_in_time_recovery_time` and `point_in_time_recovery_deployment_id` parameters. The `point_in_time_recovery_deployment_id` is the source deployment's ID and `point_in_time_recovery_time` is the timestamp in Coordinated Universal Time you want to restore to. To restore to the latest available point-in-time use `"point_in_time_recovery_time":" "`.

If you need to adjust resources or use a Key Protect key, add the optional parameters `key_protect_key`, `members_disk_allocation_mb`, `members_memory_allocation_mb`, and/or `members_cpu_allocation_count`, and their values to the body of the request.

### Restoring a backup with Terraform
{: #restore-terraform}
{: terraform}

Before restoring, ensure that your `point_in_time_recovery_time` is no older than one week. If the timestamp is any older than 7 days, down to the second, validation fails.{: important}

Use the [`ibm_database_point_in_time_recovery`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/database_pitr){: .external} data source to restore your database instance with [`point_in_time_recovery`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database#sample-database-instance-by-using-point_in_time_recovery){: .external}.

The point-in-time-recovery timestamp must be formatted as follows: `%Y-%m-%dT%H:%M:%SZ`.
{: important}

Your Terraform script looks like this. 
```sh
terraform {
  required_providers {
     ibm = {
       version = "1.44.3"
       source  = "IBM-Cloud/ibm"
    }
  }
}

variable "ibmcloud_api_key" {
  description = "<Enter your IBM Cloud API Key>"
}

provider "ibm" {
  region = "us-south"
  ibmcloud_api_key = var.ibmcloud_api_key
}

data "ibm_resource_group" "default_group" {
  is_default = true
}

resource "ibm_database" "mongodb_enterprise" {
  resource_group_id = data.ibm_resource_group.default_group.id
  name              = "testing-mongodb-pitr"
  service           = "databases-for-mongodb"
  plan              = "enterprise"
  location          = "us-south"

  point_in_time_recovery_deployment_id = "<crn>"
  point_in_time_recovery_time = "2022-09-14T14:47:45Z"

  group {
    group_id = "member"

    members {
      allocation_count = 3
    }

    cpu {
      allocation_count = 6
    }

    memory {
      allocation_mb = 14336
    }

    disk {
      allocation_mb = 20480
    }
  }
}
```
{: codeblock}
 
