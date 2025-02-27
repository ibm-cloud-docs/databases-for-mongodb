---
copyright:
  years: 2019, 2024
lastupdated: "2024-02-09"

keywords: mongodb, databases, upgrading, new deployment, major version, upgrade, new instance

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading to a new Major Version
{: #upgrading}

When a major version of a database is at its end of life (EOL), upgrade to the next available major version.

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

| Current Version |	Major Version Upgrade Path |
| ---- | ----- |
| MongoDB 6 |	-> MongoDB 7 |
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
