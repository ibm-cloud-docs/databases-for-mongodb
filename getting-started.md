---
copyright:
  years: 2019, 2023
lastupdated: "2023-10-06"

keywords: mongodb, databases, mongodb compass, mongodbee, mongodb enterprise, mongodb ee provision, mongodb compass, mongodb ops manager

subcollection: databases-for-mongodb

content-type: tutorial
services: 
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting Started with {{site.data.keyword.databases-for-mongodb}}
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

{{site.data.keyword.databases-for-mongodb_full}} allows developers to take advantage of the latest MongoDB features: rich JSON documents, powerful query language, multi-document transactions, and authentic APIs. The service also automates common database administration tasks like high availability, backups, encryption, and infrastructure planning.

To get started with {{site.data.keyword.databases-for-mongodb_full}}, you need to take advantage of MongoDB Compass, an interactive GUI tool for querying, optimizing, and analyzing your MongoDB data. The full-featured [Compass Edition](https://docs.mongodb.com/compass/master/#available-compass-short-editions){: .external} provides basic tools for viewing your MongoDB databases. 

## Before you begin
{: #before-begin-mongodb}
{: step}

* You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external} and a {{site.data.keyword.databases-for-mongodb}} deployment. Give your deployment a memorable name that appears in your account's Resource List.

* Next, [set the Admin Password](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management&interface=ui#user-management-set-admin-password-ui) for your deployment. 

* Then, download and install [MongoDB Compass](https://docs.mongodb.com/compass/master/install/){: .external} from MongoDB. 

* If your deployment is not using public endpoints, take [these additional steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-endpoints#private-endpoints) to configure private endpoint access. 
  
   MongoDB cannot support both [public and private endpoints simultaneously](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-endpoints&interface=ui#provisioning-service-endpoints). This cannot be changed after provisioning.
   {: .important}

* Finally, review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-mongodb_full}} deployment.

## {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition
{: #mongodbee}

{{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition (EE) offers more functionality, including:
* Automatic, client-side encryption
* [Audit logging](/docs/databases-for-mongodb?topic=databases-for-mongodb-auditlogging)
* Federal Information Processing Standard (FIPS) approved encryption
* [Ops Manager](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager), which allows you to manage, monitor, and back up MongoDB deployments
* [MongoDB EE Analytics Add-On](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics), which allows you to make your query data compatible with business intelligence (BI) tools

### Provisioning {{site.data.keyword.databases-for-mongodb_full}} EE
{: #mongodbee-provision}

To take advantage of the additional functions of {{site.data.keyword.databases-for-mongodb_full}} EE, choose **Enterprise** during your {{site.data.keyword.databases-for-mongodb_full}} provisioning process.

## Connect with MongoDB Compass
{: #connecting-mongodb-compass}
{: step}

When you first open MongoDB Compass to the **New Connection** page, enter your deployment's connection information. All relevant connection information can be found within your deployment's **Overview** page.

To connect to your deployment with MongoDB Compass, complete the following steps:

- In **New Connection**, enter the **URI**. Copy this from the **Public Connections** Endpoint, within your deployment's **Overview**.
- Click **>Advanced Connection Options**.
- In the _Authentication_ tab, select _Username/Password_, and enter the credentials that you set for the admin user in your deployment's **Settings**. 
- Configure the **TLS/SSL settings.
    1. In your deployment's **Overview**, copy the certificate information from **TLS Certificate**.
    1. In your deployment's **Overview**, download the TLS certificate from the **Certificate Authority** section.
    1. In MongoDB Compass, click **Select Files** in the _Certificate Authority_ field and upload the certificate file to MongoDB Compass.
- (Optional) Give your {{site.data.keyword.databases-for-mongodb}} deployment a name.
- Click **Connect** to connect MongoDB Compass to your {{site.data.keyword.databases-for-mongodb}} deployment.

## Use MongoDB Compass
{: #using-mongodb-compass}
{: step}

After you connect to your deployment, you see a basic overview. Included is a simple summary of the cluster and the default databases. The cluster contains three nodes, the two data nodes and the third arbiter node, so it shows the three hosts and their replica set. Also shown is the current MongoDB version. {{site.data.keyword.databases-for-mongodb}} Standard uses the Community version while {{site.data.keyword.databases-for-mongodb}} EE uses the Enterprise version of the MongoDB database.

Next, you see the default databases for your deployment, which all hold information related to the database instance. `local` holds replication data. `config` holds data for cluster operations. `admin` holds user authentication data. MongoDB Compass might not have access to all the data in these databases for permissions and security reasons.

Now you can use MongoDB Compass to view any data you and your applications have stored in your deployment. You can also use MongoDB Compass to create new databases, collections, and documents. Specific information can be found in the [MongoDB Compass documentation](https://docs.mongodb.com/compass/current/){: .external}.

## Next Steps
{: #getting-started-mongodb-next-steps}

- For guidance on best practices, check out [Best Practices for MongoDB on the IBM Cloud](https://www.ibm.com/cloud/blog/best-practices-for-mongodb-on-the-ibm-cloud){: .external}. If you are using MongoDB for the first time, see the [official MongoDB documentation](https://docs.mongodb.com/){: .external}.

- Connect to and manage your MongoDB database through the [MongoDB Shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-connecting-cli-client) and explore the [OpsManager](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager) functions offered in {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployments.

- Looking for more tools on managing your databases and data? Connect to your deployment with the [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli) and the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference). You can also use the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

- If you plan to use {{site.data.keyword.databases-for-mongodb}} for your applications, check out [Connecting an external application](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-external-app), [Connecting an IBM Cloud application](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-connecting-ibmcloud-app), and [MongoDB Node.js Driver Connection Guide](https://docs.mongodb.com/drivers/node/current/fundamentals/connection/).

- For information on TLS/SSL certificate configuration in the API, review [Driver TLS and self-signed certificate support](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-external-app#mongodb-tls-certificate-support), [Using the self-signed certificate in mongo Shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-connecting-cli-client#connecting-cli-client-cert), and [MongoDB TLS/SSL Configuration for Clients](https://docs.mongodb.com/manual/tutorial/configure-ssl-clients/){: .external}.
  
- To ensure the stability of your applications and databases, check out[High-Availability](/docs/databases-for-mongodb?topic=databases-for-mongodb-high-availability) and [Performance](/docs/databases-for-mongodb?topic=databases-for-mongodb-performance).
