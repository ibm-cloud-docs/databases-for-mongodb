---
copyright:
  years: 2020, 2022
lastupdated: "2022-09-30"

keywords: databases, opsman, mongodbee, Enterprise Edition, ops manager, pitr, mongodb point-in-time recovery, mongodb pitr, mongodb terraform

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:preview: .preview}
{:important: .important}

# Point-in-time Recovery
{: #pitr}

{{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition Point-In-Time Recovery (PITR) is available for select clients.{: preview}

{{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition offers Point-In-Time Recovery (PITR) using the timestamp that is returned by the {{site.data.keyword.databases-for}} API `point-in-time-recovery` timestamp API.

## Get Point-in-time-recovery timestamp by using the {{site.data.keyword.databases-for}} API
{: #pitr-recovery-api}

To return the earliest available time for PITR, use the [point-in-time-recovery timestamp endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#capability).

```sh
{
    "point_in_time_recovery_data": {
        "earliest_point_in_time_recovery_time": "2019-09-09T23:16:00Z"
    }
}
```
{: .codeblock}

The point-in-time-recovery timestamp endpoint always returns *current time* - *1 week*.{: important}


## Recovery
{: #recovery}

Backups are restored to a new deployment. After the new deployment finishes provisioning, your data in the backup file is restored into the new deployment. Backups are also restorable across accounts, but only by using the API and only if the user that is running the restore has access to both the source and destination accounts. 

The new deployment is automatically sized to the same disk and memory allocation as the source deployment at the time of the backup from which you restore. Especially in the case of PITR, that might not be the current size of your deployment. If you need to adjust the resources that are allocated to the new deployment, use the optional fields in the UI, CLI, or API to resize the new deployment. If the deployment is not given enough resources the restore fails, so allocate enough for your data and workload.

While storage and memory are restored to the same as the source deployment, specific instance configurations are not automatically set for the new instance. In this case, rerunning the configuration after a restore might be needed. Note any instance modifications before running the restore (parameters like `shared_buffers`, `max_connections`, `deadlock_timeout`, `archive_timeout`) to ensure accurate settings for the instance after the restore is complete.

Do not delete the source deployment while the backup is restoring. You must wait until the new deployment is provisioned and the backup is restored before deleting the old deployment. Deleting a deployment also deletes its backups. So, not only will the restore fail, but you might not be able to recover the backup.
{: note}

### Restoring a backup by using Terraform
{: #restore-terraform}

Before restoring, ensure that your `point_in_time_recovery_time` is no older than one week. If the timestamp is any older than 7 days, down to the second, validation fails.{: important}

Use the [`ibm_database_point_in_time_recovery`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/database_pitr){: .external} data source to restore your database instance with [`point_in_time_recovery`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database#sample-database-instance-by-using-point_in_time_recovery){: .external}.

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
  region = "eu-fr2"
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
  location          = "eu-fr2"

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


## Verifying PITR
{: #pitr-verify}

To verify the correct recovery time, check the database logs. Checking the database logs requires the [Logging Integration](/docs/databases-for-mongodb?topic=cloud-databases-logging) to be set up on your deployment.

When performing a recovery, your data is restored from the latest available backup. Any outstanding transactions from the WAL log are used to restore your database up to the time to which you recovered. After the recovery is finished, and the transactions are run, the logs display a message. You can check that your logs have the message,
```sh
LOG:  last completed transaction was at log time 2019-09-03 19:40:48.997696+00
```

There are two scenarios where recovery does not show up in the logs. 
1. Your deployment has a recent full backup and there is no activity after the backup was taken that needs to be replayed.
2. If you entered a time to recover to that is **after** the current time or is past latest available point-in-time recovery point.

In both cases the recovery is usually still successful, but there won't be an entry in the logs to check the exact time to which the database was restored.
