---
copyright:
  years: 2019, 2025
lastupdated: "2025-07-01"

keywords: mongodb, databases, upgrading, new deployment, major version, upgrade, new instance

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading to a new major version
{: #upgrading}

{{site.data.keyword.databases-for-mongodb}} provides two different upgrade paths:

- In-place upgrade to a new major version (currently supported for MongoDB Community Edition).
- Restoring from backup (supported for MongoDB Community, MongoDB Enterprise, and MongoDB Sharding Editions).

## In-place major version upgrades
{: #upgrading-in-place}

In-place major version upgrade allows you to upgrade your deployment to the next new [major version](/docs/databases-for-mongodb?topic=databases-for-mongodb-versioning-policy#version-definitions), eliminating the need to restore a backup into a new deployment. This approach maintains the same connection strings, without the need to reconfigure the deployment. However, if the new major version requires application adjustments, these must be addressed. 

There are two options when performing an in-place major version upgrade:

- In-place major version upgrade with backup: This path creates a backup before performing the actual upgrade, providing an added layer of safety.
  
   During the in-place major version upgrade window (including a backup), the deployment is set to [*setUserWriteBlockMode*](https://www.mongodb.com/docs/manual/reference/command/setUserWriteBlockMode/#mongodb-dbcommand-dbcmd.setUserWriteBlockMode), which only allows read operations but no write opertions to the deployment to ensure a safe upgrade. As soon as the major version upgrade of the deployment is completed, the *writeBlockMode* is removed. 
{: important}

- In-place major version upgrade without backup (not recommended): This option proceeds with the upgrade without creating a backup beforehand. In the event that the in-place upgrade is unsuccessful, you will need to restore your deployment from the latest backup into a new deployment to ensure data integrity and minimal downtime.

   Create a backup before starting the in-place upgrade process. This step is optional, but strongly recommended.
   {: important}

### Before you begin
{: #upgrading-considerations}

Consider the following aspects before starting the upgrade procedure.

- Your deployment must be in a healthy state before upgrading.
- Currently, you can only upgrade to the *next* major version, instead of specifying the version of your choice.
- Each major version contains some features that may not be backward-compatible with previous versions. Check the [release notes](https://www.mongodb.com/docs/manual/release-notes/) from the database vendor to see any changes that may affect your applications.
- Downgrading a deployment to a previous version is not supported.

### Upgrading in the UI
{: #upgrading-in-place-ui}
{: ui}

1. Create a new {{site.data.keyword.databases-for-mongodb}} to test the upgrade process. <br> Create the deployment by [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui#restore-backup) from your existing deployment with the same version.
2. Point your staging application to the test deployment. <br> Update your staging application to point to the test deployment. Confirm that your test application can connect successfully to the staging deployment and that the application operates as expected. Perform any required performance and operational testing of the staging environment.
3. Upgrade the major version of your test deployment by clicking on the **Upgrade major version** button on the *Overview* page. <br> This will put your database into READ-ONLY mode while the upgrade process completes. Note how long the upgrade takes to complete so that you can use the upgrade expiry setting to contain upgrades within your maintenance window.
4. Confirm that your staging application works with the new database version. <br> If your application works, this step confirms that it should be safe to upgrade your production database.
5. Upgrade your production database deployment to the new version. <br> Once you confirmed that your application works correctly by using the new version of the database, you can return to the management console and start the process of upgrading your production deployment. In the **Deployment details** section of the *Overview* page, click the **Upgrade major version** button and follow the steps.

   Once the in-place upgrade process starts, it cannot be stopped or rolled back. So, in the unlikely event of an error, your database deployment could become unrecoverable. Therefore, create a backup that you can then use to restore to a new deployment. If you select 'In-place major version upgrade with backup', the backup that is created can be used to restore in a new deployment.

The `expiration for starting upgrade` allows you to configure a 'timeout' period that the upgrade job must start within before it is automatically cancelled. In addition, test the upgrade in staging upfront to ensure that the upgrade completes within your desired time window. If, for example, you want to complete the upgrade within 1 hour, and you tested the upgrade and know that it takes 50 minutes, then your upgrade job must start within 10 minutes of you confirming that you want to upgrade. Therefore, set the expiration to 10 minutes, so that if it doesn't start within that time, it won't overrun your window.

### Upgrading through the API
{: #upgrading-in-place-api}
{: api}

Use the following command to upgrade in-place:

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/version?expiration_datetime='2027-07-07T17:17:17Z' -H 'Authorization: Bearer <>' -H 'Content-Type: application/json' -d '{"version": "7.2"}' \
```
{: pre}

The `expiration for starting upgrade` allows you to configure a 'timeout' period that the upgrade job must start within before it is automatically cancelled. In addition, test the upgrade in staging upfront to ensure that the upgrade completes within your desired time window. If, for example, you want to complete the upgrade within 1 hour, and you tested the upgrade and know that it takes 50 minutes, then your upgrade job must start within 10 minutes of you confirming that you want to upgrade. Therefore, set the expiration to 10 minutes, so that if it doesn't start within that time, it won't overrun your window. For more information, see the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#setdatabaseinplaceversionupgrade).

### Upgrading through the CLI
{: #upgrading-in-place-cli}
{: cli}

Available in CDB plugin version >= 0.20.0 
{: .note}

To view the list of allowed upgrade and restore transitions for the deployment:

```sh
ibmcloud cdb deployment-capability-show <NAME|CRN> versions
```

To upgrade command with required parameters:

```sh
ibmcloud cdb deployment-version-upgrade <NAME|CRN> <TARGET_VERSION>
```

To view full details of command parameters:

```sh
ibmcloud cdb deployment-version-upgrade --help
```

The `expiration for starting upgrade` allows you to configure a 'timeout' period that the upgrade job must start within before it is automatically cancelled. In addition, test the upgrade in staging upfront to ensure that the upgrade completes within your desired time window. If, for example, you want to complete the upgrade within 1 hour, and you tested the upgrade and know that it takes 50 minutes, then your upgrade job must start within 10 minutes of you confirming that you want to upgrade. Therefore, set the expiration to 10 minutes, so that if it doesn't start within that time, it won't overrun your window.

### Upgrading through Terraform
{: #upgrading-in-place-terraform}
{: terraform}

To upgrade, just add or change the `version` value in your configuration. There is also an optional bool flag, `version_upgrade_skip_backup`, that you can set to skip backup.

The database will be put into READ-ONLY mode during upgrade. It is highly recommended to test before upgrading.

Upgrading may require more time than the default timeout. A longer timeout value can be set with using the timeouts attribute.
{: .note}

Skipping a backup is not recommended. Skipping a backup before a version upgrade is dangerous and may result in data loss if the upgrade fails at any stage — there will be no immediate backup to restore from.
{: .attention}

Terraform has timeouts instead of expiration timestamps. Therefore, increase your timeout, as your timeout update value is used as the expiration. For example, if you set a timeout of 20 minutes, the expiration will be set to 20 minutes and if the upgrade does not start in that time frame, it expires and the upgrade will not start.

If an upgrade is in progress, note that some tasks may be queued and will not proceed until the version upgrade completes.

### Troubleshooting
{: #upgrading-in-place-troubleshooting}

#### User with *bypassWriteBlockingMode*
{: #upgrading-in-place-bypassWriteBlockingMode}

To ensure a safe upgrade, no user must be able to perform a write action during backup or upgrade. Before the database enters *writeBlockMode*, a check is performed if any user has the privilege to *bypassWriteBlockingMode*. If such a user is identified, the task enters a failed state. Any retry will fail and only removing a user with such a privilege allows to execute the in-place major version upgrade.

#### Healthchecks
{: #upgrading-in-place-healthchecks}

If a service instance is low on resources, the task fails because a safe upgrade cannot be guaranteed under these circumstances. The resource consumption can be evaluated by using the [monitoring integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring&interface=ui). If not all database components are available to be upgraded, the upgrade task fails. This can happen due to maintenance. Tasks that failed due to failed healthchecks can be retried later. If the task continuously fails, open a support ticket with [IBM Cloud support](https://cloud.ibm.com/login?redirect=%2Funifiedsupport%2Fsupportcenter).

## Restoring from backup
{: #upgrading-restoring-from-backup}

Before a major version of a database reaches its end of life (EOL), upgrade to the next available major version by restoring from a backup into a new database instance. 

Prepare to run on, and then migrate to, the latest version before the EOL date. For more information, see [Versioning Policy](/docs/cloud-databases?topic=cloud-databases-versioning-policy){: external}.

Rolling back versions is not supported.
{: .note}

Upgrade to the latest version of MongoDB available to {{site.data.keyword.databases-for-mongodb}}. Find the latest version from the catalog page, from the {{site.data.keyword.databases-for}} CLI plug-in command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show){: external}, or from the {{site.data.keyword.databases-for}} API [`/deployables`](/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeployables){: external} endpoint.

Upgrading is handled by [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups){: external} of your data into a new deployment. Restoring from a backup has various advantages:

- The original database stays running and production work can be uninterrupted.
- You can test the new database out of production and act on any application incompatibilities.
- The entire process can be rerun at any point.
- A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.

### Upgrade paths
{: #upgrading-paths}

| Current version | Major version upgrade path |
| ---- | ----- |
| MongoDB 6 | MongoDB 7 |
{: caption="Major version upgrade paths" caption-side="top"}

### Upgrading in the UI
{: #upgrading-ui}
{: ui}

For new hosting models (isolated compute and shared compute), upgrading to a new major version is available through the [CLI](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=cli) and [API](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=api).
{: note}

You can upgrade to a new version by [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui#restore-backup) from the _Backups and restore_ page of your deployment on the {{site.data.keyword.cloud_notm}} console. Click **Restore backup** on a backup to open a page in a new tab where you can change some options for the new deployment. One of them is the database version, which is auto-populated with the versions available for you to upgrade to. Select a version and click **Restore backup** to start the provision and restore process.

### Upgrading through the CLI
{: #upgrading-cli}
{: cli}

When you upgrade and restore from backup through the {{site.data.keyword.cloud_notm}} CLI, use the provisioning command from the resource controller.

```sh
ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE_ID> <SERVICE_PLAN_ID> <REGION>
```
{: pre}

The parameters `instance_name`, `service_id`, `service_plan_id`, and `region` are all required. You also supply the `-p` with the version and backup ID parameters in a JSON object. The new deployment is automatically sized with the same disk and memory as the source deployment at the time of the backup.

```sh
ibmcloud resource service-instance-create example-upgrade databases-for-mongodb standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-mongodb:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "version":"7.0"
}'
```
{: pre}

### Upgrading through the API
{: #upgrading-api}
{: api}

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=api#provision-controller-api) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version and backup ID. The new deployment has the same memory and disk allocation as the source deployment at the time of the backup.

```sh
curl -X POST   https://resource-controller.cloud.ibm.com/v2/resource_instances   -H 'Authorization: Bearer <>'   -H 'Content-Type: application/json'     -d '{
    "name": "my-instance",
    "target": "us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-mongodb-standard",
    "backup_id": "crn:v1:bluemix:public:databases-for-mongodb:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
    "version":"7.0"
  }'
```

### Upgrading through Terraform
{: #upgrading-terraform}
{: terraform}

Use Terraform to restore to a backup from an older version to a new version.

1. Set your `backup_id`. For more information, see [`backup_id`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database#backup_id){: external}.
1. Set your `version` in the version attribute. For more information, see [`version`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database#version){: external}.

The code looks like:

```tf
resource "ibm_database" "<your-instance>" {
  name                                 = "<your_database_name>"
  service                              = "<service>"
  plan                                 = "<plan>"
  location                             = "<region>"
  version                              = "<version>"
  backup_id                            = "<backup_id>"
}
```
{: codeblock}

For more information, see the [{{site.data.keyword.databases-for}} Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/database_backups){: external}
```
{: pre}
