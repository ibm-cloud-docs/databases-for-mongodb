---
copyright:
  years: 2019, 2023
lastupdated: "2023-05-25"

keywords: mongodb, databases, mongodb compass, mongodbee, mongodb enterprise, mongodb ee provision, mongodb compass, mongodb ops manager

subcollection: databases-for-mongodb

content-type: tutorial
services: 
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting Started 
{: #getting-started-new}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

Now that you've provisioned your {{site.data.keyword.databases-for-mongodb_full}} instance, let's create a basic ToDo app. This app will demonstrate to you how to:
- connect your {{site.data.keyword.databases-for-mongodb}} instance
- create a collection
- add data to that collection
- delete data from that collection
- update data from that collection

## Before you begin
{: #before-begin-mongodb-new}
{: step}

* You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external} and a {{site.data.keyword.databases-for-mongodb}} deployment. Give your deployment a memorable name that appears in your account's Resource List.

* Next, [set the Admin Password](/docs/databases-for-mongodb?topic=databases-for-mongodb-admin-password) for your deployment. 

* Then, download and install [MongoDB Compass](https://docs.mongodb.com/compass/master/install/){: .external} from MongoDB. 

* If your deployment does not use public endpoints, take [these additional steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-endpoints#private-endpoints) to configure private endpoint access. 
  
   MongoDB cannot support both [public and private endpoints simultaneously](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-endpoints&interface=ui#provisioning-service-endpoints). This cannot be changed after provisioning.
   {: .important}

* Finally, review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-mongodb_full}} deployment.

## Connect with MongoDB Compass
{: #connecting-mongodb-compass-new}
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

### Using MongoDB Compass
{: #using-mongodb-compass}

After you connect to your deployment, you see a basic overview. Included is a simple summary of the cluster and the default databases. The cluster contains three nodes, the two data nodes and the third arbiter node, so it shows the three hosts and their replica set. Also shown is the current MongoDB version. {{site.data.keyword.databases-for-mongodb}} Standard uses the Community version while {{site.data.keyword.databases-for-mongodb}} EE uses the Enterprise version of the MongoDB database.

Next, you see the default databases for your deployment, which all hold information related to the database instance. `local` holds replication data. `config` holds data for cluster operations. `admin` holds user authentication data. MongoDB Compass might not have access to all the data in these databases for permissions and security reasons.

Now you can use MongoDB Compass to view any data you and your applications have stored in your deployment. You can also use MongoDB Compass to create new databases, collections, and documents. Specific information can be found in the [MongoDB Compass documentation](https://docs.mongodb.com/compass/current/){: .external}.

## Create your ToDo App
{: #getting-started-create-todo-app}

First, import the necessary modules, using a command like:
{: javascript}
{: python}

```javascript
const express = require('express');
const MongoClient = require('mongodb').MongoClient;
// The code begins by importing the Express framework for building the server and the MongoDB client module for connecting to the MongoDB database.
```
{: pre}
{: javascript}

```python
from flask import Flask, request, jsonify
from pymongo import MongoClient
# The code begins by importing the Flask framework for building the server and the PyMongo library for connecting to the MongoDB database.
```
{: pre}
{: python}

Next, create the Express app and define the port, using a command like:
{: javascript}

Next, create the Flask app:
{: python}

```javascript
const app = express();
const port = 3000;
```
{: pre}
{: javascript}

```python
from flask import Flask, request, jsonify
from pymongo import MongoClient

```
{: pre}
{: python}

Now, establish a connection to MongoDB:
{: javascript}

```javascript
const uri = 'YOUR_MONGODB_CONNECTION_URI';

MongoClient.connect(uri, { useUnifiedTopology: true })
  .then(client => {
    const db = client.db('todoapp');
    const collection = db.collection('todos');
// This code establishes a connection to the MongoDB database using the provided connection URI. It then creates a reference to the todos collection within the todoapp database.
```
{: pre}
{: javascript}

```python
uri = 'YOUR_MONGODB_CONNECTION_URI'
client = MongoClient(uri)
db = client.todoapp
collection = db.todos
# This code establishes a connection to the MongoDB database using the provided connection URI. It then creates references to the todos collection within the todoapp database.
```
{: pre}
{: python}
