---
copyright:
  years: 2017,2018
lastupdated: "2018-12-10"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Connecting with the {{site.data.keyword.cloud_notm}} CLI
{: #connecting-cli-plugin}

Use the {{site.data.keyword.cloud_notm}} CLI and the {{site.data.keyword.databases-for}} CLI plug-in to access and manage {{site.data.keyword.databases-for-mongodb_full}}. The {{site.data.keyword.cloud_notm}} CLI is a general-purpose developer tool that provides access to your {{site.data.keyword.cloud_notm}} account and services through a command line interface. The {{site.data.keyword.databases-for}} CLI plug-in provides the `cdb` commands to use with your {{site.data.keyword.databases-for-mongodb}} deployment.

Installation instructions for both the {{site.data.keyword.cloud_notm}} CLI and the {{site.data.keyword.databases-for}} CLI plug-in are available on the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference) reference page.



## Accessing `mongo` from the CLI 

If you need to open a connection directly to your MongoDB, the `ibmcloud cdb deployment-connections` command handles everything that is involved in creating the client connection. For example, to connect to a deployment named  "example-mongo", use the following command.

```
ibmcloud cdb deployment-connections example-mongo --start
```
Or
```
ibmcloud cdb cxn example-mongo -s
```

If you don't specify a user, the `deployment-connections` command uses the admin user by default. In order to connect with the admin user, you have to [set its password](./admin-password.html).
{: .tip}

The command prompts for the password and then runs the mongo shell to connect to the database. For more information about connecting to MongoDB directly, see the [Connecting with the Mongo Shell](./connecting-cli-client.html) page.

