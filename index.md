---

Copyright:
  years: 2018, 2019
lastupdated: "2019-01-24"

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# About {{site.data.keyword.databases-for-mongodb_full_notm}}
{: #about}

{{site.data.keyword.databases-for-mongodb_full}} is a managed MongoDB service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services.

## Provisioning {{site.data.keyword.databases-for-mongodb}}

{{site.data.keyword.databases-for-mongodb}} is an {{site.data.keyword.cloud_notm}} service. Provisioning and account management is handled through your {{site.data.keyword.cloud_notm}} account. If you already have an account, you can provision {{site.data.keyword.databases-for-mongodb}} from the [{{site.data.keyword.cloud_notm}} catalog](https://{DomainName}/catalog/services/databases-for-mongodb).

For detailed provisioning information, including {{site.data.keyword.cloud_notm}} CLI and Terraform instructions, see the [Provisioning](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-provisioning) page.

If you don't yet have an {{site.data.keyword.cloud_notm}} account, sign up on the [registration](https://{DomainName}/registration/) page.

### Managing Access to {{site.data.keyword.databases-for-mongodb}}

{{site.data.keyword.databases-for-mongodb}} is an Identity and Access Management (IAM) integrated service. Access to the service is governed by the roles and attributes that are consistent across IAM-integrated services in {{site.data.keyword.cloud_notm}}. Get started with managing your users on the [IAM Getting Started tutorial](/docs/iam?topic=iam-getstarted). For more information on IAM, see the [What is IAM?](/docs/iam?topic=iam-iamoverview) documentation.

More information on IAM roles and actions for the {{site.data.keyword.databases-for-mongodb}} service is available on the [Access Management](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-iam) page.

## Using {{site.data.keyword.databases-for-mongodb}}

{{site.data.keyword.databases-for-mongodb}} provides a UI, accessible by selecting _Manage_ from the left sidebar of your service and opening the management panel. You get a quick [Overview](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-dashboard-overview) of your service as well as configuration settings on the [Settings](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-dashboard-settings) tab and access to your backups on the [Backups](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-dashboard-backups) tab.

### Using the command-line interface

The [{{site.data.keyword.cloud_notm}} command-line interface](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud-cli) provides in interactive terminal for your {{site.data.keyword.cloud_notm}} account and your {{site.data.keyword.cloud_notm}} services. The cloud databases plug-in extends this functionality to your {{site.data.keyword.databases-for-mongodb}} deployments. Installation instructions and a full command reference are on the [Cloud Databases CLI Plug-in](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference) page.

### Using the cloud databases API

{{site.data.keyword.databases-for-mongodb}} is compatible with the {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} API, so you can access and manage your service programmatically. Each region has a unique endpoint, so you can find the API foundation endpoint for your deployment on the [_Overview_](./dashboard-overview.html) page. The {{site.data.keyword.IBM_notm}} API documentation contains the full [{{site.data.keyword.databases-for}} API reference](https://{DomainName}/apidocs/cloud-databases-api). Authentication is IAM-based and you use your {{site.data.keyword.cloud_notm}} account's platform API keys to access the API. More information on API keys is in the [IAM documentation](/docs/iam?topic=iam-userroles#userroles).

## Connecting to {{site.data.keyword.databases-for-mongodb}}

General information on getting connection strings for your applications and for users you provision on your account can be found on the [Getting Connection Strings](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) page.

{{site.data.keyword.databases-for-mongodb}} deployments are secured with authentication and SSL/TLS encrypted connections. Deployments are provisioned with an admin user. [Set the admin password](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-admin-password) to use it to access your deployment. Then, if you want to manage the MongoDB databases directly, [connect by using the `mongo` shell](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-mongo-shell).

Specific guidance on connecting with MongoDB drivers is on the [Connecting External Applications](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-external-app) page. If you want to connect a Cloud Foundry application that is running in {{site.data.keyword.cloud_notm}}, see the [Connecting an {{site.data.keyword.cloud_notm}} Application](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-ibmcloud-app) page.

## Other {{site.data.keyword.cloud_notm}} Integrations

{{site.data.keyword.databases-for-mongodb}} deployments offer other cloud services integrations. 
- View events with [Activity Tracker](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-activity-tracker)
- View usage metrics with the [Monitoring](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-monitoring) service
- BYOK encryption is available if you use [Key Protect](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-key-protect)









