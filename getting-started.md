---
copyright:
  years: 2019, 2021
lastupdated: "2021-02-02"

keywords: mongodb, databases, mongodb compass, mongodbee

subcollection: databases-for-mongodb

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Getting Started Tutorial
{: #getting-started}

This tutorial is a short introduction to using {{site.data.keyword.databases-for-mongodb_full}} Standard or {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployments. MongoDB Compass is a GUI for MongoDB, provided by the developers for MongoDB. The full-featured [Compass Edition](https://docs.mongodb.com/compass/master/#available-compass-short-editions) provides basic tools for viewing your MongoDB databases. You can download a stand-alone edition from MongoDB and connect it to your {{site.data.keyword.databases-for-mongodb}} deployment.

## Before you begin

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){:new_window}.
- And a {{site.data.keyword.databases-for-mongodb}} deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/databases-for-mongodb). Give your deployment a memorable name that appears in your account's Resource List.
- [Set the Admin Password](/docs/databases-for-mongodb?topic=databases-for-mongodb-admin-password) for your deployment.
- Download and install [MongoDB Compass](https://docs.mongodb.com/compass/master/install/) from MongoDB.
- If your deployment is not using public endpoints, you must take [these additional steps](https://cloud.ibm.com/docs/databases-for-mongodb?topic=cloud-databases-service-endpoints#private-endpoint-connections) to configure private endpoint access. MongoDB supports only [public or private endpoints](https://cloud.ibm.com/docs/databases-for-mongodb?topic=cloud-databases-service-endpoints#provisioning-with-service-endpoints), not both simultaneously. 

Review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-mongodb_full}} deployment.
## Connecting with MongoDB Compass

When you first open MongoDB Compass, you get a **Connect to Host** page. This page is where you enter the connection information for your deployment. 

![Default Connect to Host page](images/getting-started-connect-to-host.png)

On your deployment's _Manage_ page, there is a panel with all the relevant connection information.

![Endpoints panel](images/getting-started-endpoints-panel.png)

To fill out the MongoDB Compass page,

- For _Hostname_, you can use either of the two hostnames for your deployment.
- In the _Authentication_ field, select `Username/Password`, and enter the credentials that you set for the admin user in the prerequisites. The _Authentication Database_ should stay at the default of 'admin'.
- Enter the _Replica Set_ name of your deployment (it is probably `replset`) into the _Replica Set Name_ field on MongoDB Compass.
- Configure the _SSL_ settings.
    1. Copy the certificate information from the _Endpoints_ panel.
    2. Save the certificate to a file. (You can use the name that is provided in the download, or your own file name.)
    3. Set the **SSL** field in MongoDB Compass to _Server Validation_.
    4. Click **Select Files** in the _Certificate Authority_ field and upload the certificate file to MongoDB Compass.
- If you want to, you can give your {{site.data.keyword.databases-for-mongodb}} deployment a name.

![Completed Connect to Host page](images/getting-started-connect-to-host-complete.png)

Click the **Connect** button to connect MongoDB Compass to your {{site.data.keyword.databases-for-mongodb}} deployment.

## Using MongoDB Compass

Once you have connected to your deployment, you see a basic overview. Included is a simple summary of the cluster and the default databases. The cluster contains three nodes, the two data nodes and the third arbiter node, so it shows the three hosts and their replica set. Also shown is the current MongoDB version; {{site.data.keyword.databases-for-mongodb}} Standard uses the Community version while {{site.data.keyword.databases-for-mongodb}} Enterprise Edition uses the Enterprise version of the MongoDB database.

![MongoDB Compass page](images/getting-started-compass-page.png)

Next, you see the default databases for your deployment, which all hold information related to the database instance. `local` holds replication data. `config` holds data for cluster operations. `admin` holds user authentication data. MongoDB Compass might not have access to all the data in these databases for permissions and security reasons.

Now you can use MongoDB Compass to view any data you and your applications have stored in your deployment. You can also use MongoDB Compass to create new databases, collections, and documents. Specific information can be found in the [MongoDB Compass documentation](https://docs.mongodb.com/compass/current/).

## Next Steps

If you are just using MongoDB for the first time, it is a good idea to take a tour through the [official MongoDB documentation](https://docs.mongodb.com/). 

You can connect to and manage your MongoDB through the [Mongo shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongo-shell).

Explore the [OpsManager](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager) functionality offered in {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployments.

Looking for more tools on managing your databases and data? You can connect to your deployment with [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli) and the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference). Or use the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you are planning to use {{site.data.keyword.databases-for-mongodb}} for your applications, check out some of our other documentation pages.
- [Connecting an external application](/docs/databases-for-mongodb?topic=databases-for-mongodb-external-app)
- [Connecting an IBM Cloud application](/docs/databases-for-mongodb?topic=databases-for-mongodb-ibmcloud-app)
- [Connecting with the mongo Shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongo-shell)
- [MongoDB Node.js Driver — Connection Guide](https://docs.mongodb.com/drivers/node/fundamentals/connection)

For information on TLS/SSL certificate configuration in the API, review the documentation at: 
- [Driver TLS and self-signed certificate support](/docs/databases-for-mongodb?topic=databases-for-mongodb-external-app#driver-tls-and-self-signed-certificate-support)
- [Using the self-signed certificate in mongo Shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongo-shell#using-the-self-signed-certificate)
- [MongoDB TLS/SSL Configuration for Clients](https://docs.mongodb.com/manual/tutorial/configure-ssl-clients/) 
  

Also, to ensure the stability of your applications and your database, check out the pages on 
- [High-Availability](/docs/databases-for-mongodb?topic=databases-for-mongodb-high-availability)
- [Performance](/docs/databases-for-mongodb?topic=databases-for-mongodb-performance)


