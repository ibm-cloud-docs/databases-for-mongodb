---
copyright:
  years: 2019, 2025
lastupdated: "2025-05-27"

keywords: mongodb, databases, upgrading, new deployment, major version, upgrade, new instance

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading to a new major version
{: #upgrading}

## In-place major version upgrades
{: #upgrading-in-place}

In-place major version upgrades (IPMVU) upgrade your database to the next new [major version](/docs/databases-for-mongodb?topic=databases-for-mongodb-versioning-policy#version-definitions) without the need for restoring a backup to a new deployment. The advantage of this type of upgrade is that connection strings do not change and therefore applications do not need updating in order to connect after the upgrade. Follow the process below before starting an in-place upgrade. In-place major version upgrade is supported for the MongoDB Community Edition. 

There are two options available, with backup before the upgrade and without backup before the upgrade. The service instance will be configured to perform *[setUserWriteBlockMode](https://www.mongodb.com/docs/manual/reference/command/setUserWriteBlockMode/#mongodb-dbcommand-dbcmd.setUserWriteBlockMode)* during backup and version upgrade to ensure a safe upgrade. As soon as the version upgrade of the database is completed, the *writeBlockMode* is removed.

When scheduling a backup, consider that the time to perform a version upgrade is strongly influenced by the backup time.
{: note}

### Before you begin
{: #upgrading-considerations}

Consider the following aspects before starting the upgrade procedure.

- Your deployment must be in a healthy state before upgrading.
- Depending on the database type, you may only be able to upgrade to the *next* major version, instead of specifying the version of your choice.
- Each major version contains some feautres that may not be backward-compatible with previous versions. Check the release notes from the database vendor to see any changes that may affect your applications.
- It is not possible to downgrade versions in-place. You can only downgrade from a backup.

### Upgrade procedure
{: #upgrading-in-place-procedure}

1. Create a new {{site.data.keyword.databases-for-mongodb}} to test the upgrade process <br> Create this new deployment with the current version you are using. You can create it by [restoring a backup]() from your existing deployment, so it contains data.
2. Point your staging application to the test deployment <br> Update your staging application to point to the test deployment. Confirm that your test application can connect successfully to the staging deployment and that the application operates as expected. Perform any required performance and operational testing of the staging environment.
3. Upgrade the major version of your test deployment by clicking on the **Upgrade major version** button on the *Overview* page. <br> This will put your database into read-only mode while the upgrade process completes. Note how long the upgrade takes to complete so that you can use the upgrade expiry setting to contain upgrades within your maintenance window.
4. Confirm that your staging application works with the new database version. <br> If your application works, this step confirms that it should be safe to upgrade your production database.
5. Upgrade your production database deployment to the new version. <br> Once you confirmed that your application works correctly by using the new version of the database, you can return to the management console and start the process of upgrading your production deployment.

  Create a backup before starting the in-place upgrade process. This step is optional, but strongly recommended.
  {: important}

  Once the in-place upgrade process starts, it cannot be stopped or rolled back. So, in the unlikely event of an error, your database deployment could become unrecoverable. Therefore, create a backup that you can then use to restore to a new instance.

### Upgrading through the CLI
{: #upgrading-in-place-cli}
{: cli}

1. Use the following command for help:

  ```sh
  ibmcloud cdb deployment-version-upgrade --help
  ```

  {: pre}

  The following shows an example output of the command:

  ```sh
  NAME:
    deployment-version-upgrade, version-upgrade - Upgrade the version of deployment. The database will be put into READ-ONLY mode during upgrade. It is strongly advised to test before upgrading. To learn more, refer to the version upgrade documentation.

  USAGE:
    ibmcloud cdb deployment-version-upgrade <NAME|ID> VERSION [-ea TIMESTAMP] [-ei VALUE] [--skip-backup] [--json] [--api-version] 

  OPTIONS:
    --expire-at, --ea  --expire-at value, --ea value  Option to specify how long to wait for the job to start before the upgrade is cancelled. Must be in the ISO8601 UTC format yyyy-mm-ddThh:mm:ssZ (e.g. 2025-01-31T14:45:00Z) and between 5 minutes and 24 hours from now. Cannot be used with expire-in
    --expire-in, --ei  --expire-in value, --ei value  Option to specify how long to wait for the job to start before the upgrade is cancelled. Must be in minutes(m) or hours(h). Cannot be used with expire-at. Min: 5m, Max: 24h. Default: 5m.
    --skip-backup, -s  --skip-backup, -s              Option to skip taking a backup before upgrading the database version. This is only applicable for supported database types. To learn more, refer to the version upgrade documentation.
    --json, -j         --json, -j                     Results as JSON.
    --api-version, -v  --api-version value, -v value  API Version used for request.
    --nowait, -n       --nowait, -n                   Do not wait for command completion.
  ```

  {: pre}

2. The following is a command success example and its output:

  ```sh
  ibmcloud cdb deployment-version-upgrade crn:v1:staging:public:databases-for-mongodb:eu-gb:a/b9552134280015ebfde430a819fa4bb3:f9928834-e503-43de-8d3c-f1663cf449fd:: "7.0"
  ```

  {: pre}


  ```sh
  The deployment is being upgraded to version 7.0 in this task:
  
  Key                   Value
  ID                    crn:v1:staging:public:databases-for-mongodb:eu-gb:a/b9552134280015ebfde430a819fa4bb3:f9928834-e503-43de-8d3c-f1663cf449fd:task:baf30e8b-19b9-4422-9322-570bea011d3f
  Deployment ID         crn:v1:staging:public:databases-for-mongodb:eu-gb:a/b9552134280015ebfde430a819fa4bb3:f9928834-e503-43de-8d3c-f1663cf449fd::
  Description           Upgrading instance
  Created At            2025-05-13T08:36:49Z
  Status                running
  Progress Percentage   0
  ```
  {: pre}

3. See the following failure output example including a hint:

  ```sh
  HINT: To view the list of allowed upgrade versions for this deployment, refer to 'Upgrade Transitions' after running:
  ibmcloud cdb deployment-capability-show crn:v1:staging:public:databases-for-mongodb:eu-gb:a/b9552134280015ebfde430a819fa4bb3:f9928834-e503-43de-8d3c-f1663cf449fd:: versions
  FAILED
  Error response from server. Status code: 422; message: {"errors":{"version":["In-place version task currently running. You cannot trigger another upgrade until the task is complete.","in-place upgrade from version 6.0 to 8.0 is not supported"]}}
  ```
  {: pre}

4. The following example shows the command to view upgrade and restore transitions for an instance:

  ```sh
  ibmcloud cdb deployment-capability-show crn:v1:staging:public:databases-for-mongodb:eu-gb:a/b9552134280015ebfde430a819fa4bb3:f9928834-e503-43de-8d3c-f1663cf449fd:: versions
  ```
  {: pre}

  Output:
  
  ```sh
  Retrieving capability information from a deployment for crn:v1:staging:public:databases-for-mongodb:eu-gb:a/b9552134280015ebfde430a819fa4bb3:b0aea38b-e56b-4bf8-b460-a604ff846af9::...
  OK
  
  Deployment   
  Type         mongodb
  Version      6.0
  Platform     classic
  HostFlavor   multitenant
  Location     eu-gb
  
  Capability                       
  type                             mongodb
  version                          6.0
  status                           deprecated
  is_preferred                     false
                                   
  -----Upgrade Transition 1-----   
  application                      mongodb
  method                           in-place
  skip_backup_supported            true
  from_version                     6.0
  to_version                       7.0
                                   
  -----Restore Transition 1-----   
  application                      mongodb
  method                           restore
  from_version                     6.0
  to_version                       7.0
  ```
  {: pre}

### Troubleshooting
{: #upgrading-in-place-troubleshooting}

#### User with *bypassWriteBlockingMode*
{: #upgrading-in-place-bypassWriteBlockingMode}

To ensure a safe upgrade, no user must be able to perform a write action during backup or upgrade. Before you enter *writeBlockMode*, a check is performed if any user has the privilege to *bypassWriteBlockingMode*. If such a user is identified, the task enters a failed state. Any retry will fail and only removing a user with such a privilege allows to execute the in-place major version upgrade.

#### Healthchecks
{: #upgrading-in-place-healthchecks}

If a service instance is low on resources, the task fails because a safe upgrade cannot be guaranteed under these circumstances. The resource consumption can be evaluated by using the [monitoring integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring&interface=ui). If not all database components are available to be upgraded, the upgrade task fails. This can happen due to maintenance. Tasks that failed due to failed healthchecks can be retried later. If the task continuously fails, open a support ticket with [IBM Cloud support](https://cloud.ibm.com/login?redirect=%2Funifiedsupport%2Fsupportcenter).

## Restoring from backup
{: #upgrading-restoring-from-backup}

Before a major version of a database reaches its end of life (EOL), upgrade to the next available major version.

Prepare to run on, and then migrate to, the latest version before the EOL date. For more information, see [Versioning Policy](/docs/cloud-databases?topic=cloud-databases-versioning-policy){: external}.

Rolling back versions is not supported.
{: .note}

Upgrade to the latest version of MongoDB available to {{site.data.keyword.databases-for-mongodb}}. Find the latest version from the catalog page, from the {{site.data.keyword.databases-for}} CLI plug-in command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show){: external}, or from the {{site.data.keyword.databases-for}} API [`/deployables`](/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeployables){: external} endpoint.

Upgrading is handled by [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups){: external} of your data into a new deployment. Restoring from a backup has various advantages:

- The original database stays running and production work can be uninterrupted.
- You can test the new database out of production and act on any application incompatibilities.
- The entire process can be rerun at any point.
- A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.

## Upgrade paths
{: #upgrading-paths}

| Current version | Major version upgrade path |
| ---- | ----- |
| MongoDB 6 | MongoDB 7 |
{: caption="Major version upgrade paths" caption-side="top"}

## Upgrading in the UI
{: #upgrading-ui}
{: ui}

For new hosting models, upgrading to a new major version is currently available through the [CLI](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=cli) and [API](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=api).
{: note}

You can upgrade to a new version by [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui#restore-backup) from the _Backups and restore_ page of your deployment on the {{site.data.keyword.cloud_notm}} console. Click **Restore backup** on a backup to open a page in a new tab where you can change some options for the new deployment. One of them is the database version, which is auto-populated with the versions available for you to upgrade to. Select a version and click **Restore backup** to start the provision and restore process.

## Upgrading through the CLI
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
  "version":"6.0"
}'
```
{: pre}

## Upgrading through the API
{: #upgrading-api}
{: api}

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=api#provision-controller-api) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version and backup ID. The new deployment has the same memory and disk allocation as the source deployment at the time of the backup.

```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-mongodb-standard",
    "backup_id": "crn:v1:bluemix:public:databases-for-mongodb:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
    "version":"6.0"
  }'
```
{: pre}
