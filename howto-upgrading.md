---
copyright:
  years: 2019, 2021
lastupdated: "2021-12-22"

keyowrds: mongodb, databases, upgrading

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:tip: .tip}


# Upgrading to a new Major Version
{: #upgrading}

Once a major version of a database is at its End Of Life (EOL), it is necessary to upgrade to the next available major version. You can upgrade {{site.data.keyword.databases-for-mongodb_full}} deployments to use the newest version of MongoDB. It is possible to upgrade from MongoDB 3.x to 4.x. We recommend preparing to run on, and then migrating to, the latest version prior to the EOL date [as documented here](/docs/databases-for-mongodb?topic=cloud-databases-versioning-policy#major-versions-defined). Note that downgrading versions is not supported. 

You may upgrade to the latest version of MongoDB available to {{site.data.keyword.databases-for-mongodb}}. You can find the latest version from the catalog page, from the cloud databases cli plugin command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show), or from the cloud databases API [`/deployables`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-all-deployable-databases) endpoint.

Upgrading is handled through [restoring a backup](/docs/databases-for-mongodb?topic=cloud-databases-dashboard-backups#restoring-a-backup) of your data into a new deployment. Restoring from a backup has a number of advantages:

- The original database stays running and production work can be uninterrupted.
- You can test the new database out of production and act on any application incompatibilities.
- The entire process can be rerun at any point.
- A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.

## Upgrade paths
{: #upgrading-paths}

|Current Version|	Major Version Upgrade Path
|----|-----|
|MongoDB 3.4|	-> MongoDB 3.6 -> 4.0 -> 4.2| 
|MongoDB 3.6|	-> MongoDB 4.0 -> 4.2|
|MongoDB 4.0|	-> MongoDB 4.2 |
|MongoDB 4.2| Latest version |
{: caption="Table 1. Major version upgrade paths" caption-side="top"}

To upgrade an existing MongoDB deployment to 4.0, you must be running a 3.6-series release. Likewise, to upgrade an existing MongoDB deployment to 4.2, you must be running a 4.0-series release. To upgrade from a version earlier than the  noted series, you must successively upgrade major releases until you have upgraded to the appropriate series. For example, if you are running a 3.6-series, you must upgrade first to 4.0 before you can upgrade to 4.2.
{: .note}

## Upgrading in the UI
{: #upgrading-ui}

You can upgrade to a new version when [restoring a backup](/docs/databases-for-mongodb?topic=cloud-databases-dashboard-backups#restoring-a-backup) from the _Backups_ tab of your _Deployment Overview_. Clicking **Restore** on a backup brings up a dialog box where you can change some options for the new deployment. One of them is the database version, which is auto-populated with the versions available for you to upgrade to. Select a version and click **Restore** to start the provision and restore process.

## Upgrading through the CLI
{: #upgrading-cli}

When you upgrade and restore from backup through the  {{site.data.keyword.cloud_notm}} CLI, use the provisioning command from the resource controller.
```shell
ibmcloud resource service-instance-create <service-name> <service-id> <service-plan-id> <region>
```
The parameters `service-name`, `service-id`, `service-plan-id`, and `region` are all required. You also supply the `-p` with the version and backup ID parameters in a JSON object. The new deployment is automatically sized with the same disk and memory as the source deployment at the time of the backup.

```shell
ibmcloud resource service-instance-create example-upgrade databases-for-mongodb standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-mongodb:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "version":6.5
}'
```

## Upgrading through the API
{: #upgrading-api}

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](/docs/databases-for-mongodb?topic=cloud-databases-provisioning#provisioning-through-the-resource-controller-api) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version and backup ID. The new deployment has the same memory and disk allocation as the source deployment at the time of the backup.
```curl
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "bluemix-us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-mongodb-standard",
    "backup_id": "crn:v1:bluemix:public:databases-for-mongodb:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
    "version":6.5
  }'
```

## After the primary has been upgraded
{: #setFCV}

When upgrading {{site.data.keyword.databases-for-mongodb}} Community Edition from 3.6 -> 4.0 the [`FeatureCompatibilityVersion`](https://docs.mongodb.com/manual/reference/command/setFeatureCompatibilityVersion) flag needs to be updated to enable the [4.0 features that persist data incompatible](https://docs.mongodb.com/manual/release-notes/4.0-compatibility/#compatibility-enabled) with earlier versions of MongoDB. This is only required, however, when upgrading due to an EOL forced migration. 

On the primary, run the `setFeatureCompatibilityVersion` command in the admin database:
```shell
db.adminCommand( { setFeatureCompatibilityVersion: "4.0" } )
```
{: .pre}

You can only issue the `setFeatureCompatibilityVersion` against the admin database. Likewise, ensure that no initial sync is in progress as running the command while an initial sync is in progress will cause the initial sync to restart. If for any reason the command does not complete successfully, you can safely retry the command on the primary.
{: .note}

## Migration Notes for New MongoDB 4.x Users
{: #upgrading-migration-notes}

As with any switch between major versions, there are major and sometimes breaking changes. The MongoDB documentation has a full overview of the changes available in [Compatibility Changes in MongoDB 4.0](https://docs.mongodb.com/manual/release-notes/4.0-compatibility/). Since the upgraded version runs in a new deployment, you can test against it while continuing to run your application on your current MongoDB 3.x deployment.

Before upgrading to a new major version, ensure that your application will continue to work with MongoDB 4.x. by double checking feature compatibility.
{: .tip}
