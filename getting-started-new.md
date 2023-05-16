---
copyright:
  years: 2019, 2023
lastupdated: "2023-01-06"

keywords: mongodb, databases, mongodb compass, mongodbee, mongodb enterprise, mongodb ee provision, mongodb compass, mongodb ops manager

subcollection: databases-for-mongodb

content-type: tutorial
services: 
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting Started
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

## Before you begin
{: #before-begin-mongodb}
{: step}

* You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external} and a {{site.data.keyword.databases-for-mongodb}} deployment. Give your deployment a memorable name that appears in your account's Resource List.

* Next, [set the Admin Password](/docs/databases-for-mongodb?topic=databases-for-mongodb-admin-password) for your deployment. 

* Then, download and install [MongoDB Compass](https://docs.mongodb.com/compass/master/install/){: .external} from MongoDB. 

* If your deployment is not using public endpoints, take [these additional steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-endpoints#private-endpoints) to configure private endpoint access. 
  
   MongoDB cannot support both [public and private endpoints simultaneously](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-endpoints&interface=ui#provisioning-service-endpoints). This cannot be changed after provisioning.
   {: .important}

* Finally, review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-mongodb_full}} deployment.

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

### Using MongoDB Compass
{: #using-mongodb-compass}

After you connect to your deployment, you see a basic overview. Included is a simple summary of the cluster and the default databases. The cluster contains three nodes, the two data nodes and the third arbiter node, so it shows the three hosts and their replica set. Also shown is the current MongoDB version. {{site.data.keyword.databases-for-mongodb}} Standard uses the Community version while {{site.data.keyword.databases-for-mongodb}} EE uses the Enterprise version of the MongoDB database.

Next, you see the default databases for your deployment, which all hold information related to the database instance. `local` holds replication data. `config` holds data for cluster operations. `admin` holds user authentication data. MongoDB Compass might not have access to all the data in these databases for permissions and security reasons.

Now you can use MongoDB Compass to view any data you and your applications have stored in your deployment. You can also use MongoDB Compass to create new databases, collections, and documents. Specific information can be found in the [MongoDB Compass documentation](https://docs.mongodb.com/compass/current/){: .external}.

## Connect to MongoDB
{: #connect-mongodb}

### Connect to MongoDB with Python
{: #connect-mongodb-python}
{: python}

In this code, you need to replace the following placeholders with your actual connection details:

`localhost`: Replace with the hostname or IP address of your MongoDB server.
27017: Replace with the port number of your MongoDB server. The default port for MongoDB is 27017.
`your_username`: Replace with the username for authentication (if applicable). If authentication is not enabled, you can omit the username and password variables from the connection string.
`your_password`: Replace with the password for authentication (if applicable).
`your_database_name`: Replace with the name of the database you want to connect to.
The code creates a connection string using the provided connection parameters and the f-string formatting method. It then establishes a connection to the MongoDB server using MongoClient() from PyMongo. Finally, it prints the version of the connected MongoDB server using client.server_info()['version'].

```python
from pymongo import MongoClient

# Connection parameters
host = 'localhost'       # MongoDB server hostname or IP address
port = 27017             # MongoDB server port (default is 27017)
username = 'your_username'   # Username for authentication (if applicable)
password = 'your_password'   # Password for authentication (if applicable)
database_name = 'your_database_name'   # Name of the database to connect to

# Create a MongoDB connection string
connection_string = f"mongodb://{username}:{password}@{host}:{port}/{database_name}"

# Connect to MongoDB
client = MongoClient(connection_string)

# Print the MongoDB server version
print("Connected to MongoDB server:", client.server_info()['version'])

```
{: codeblock}
{: python}

### Connect to MongoDB with Javascript
{: #connect-mongodb-javascript}
{: javascript}

Here's a JavaScript code snippet that demonstrates how to connect to a MongoDB server using the Mongoose library, which is a MongoDB object modeling tool for Node.js:

```javascript
const { MongoClient } = require('mongodb');

// Connection parameters
const host = 'localhost';           // MongoDB server hostname or IP address
const port = 27017;                 // MongoDB server port (default is 27017)
const username = 'your_username';   // Username for authentication (if applicable)
const password = 'your_password';   // Password for authentication (if applicable)
const databaseName = 'your_database_name';   // Name of the database to connect to

// Create a MongoDB connection string
const uri = `mongodb://${username}:${password}@${host}:${port}/${databaseName}`;

// Create a new MongoClient
const client = new MongoClient(uri);

// Connect to MongoDB
client.connect((err) => {
  if (err) {
    console.error('Failed to connect to MongoDB:', err);
    return;
  }
  console.log('Connected to MongoDB server');

  // Use the connected client to interact with the database

  // Close the connection when finished
  client.close();
});

```
{: codeblock}
{: javascript}

### Connect to MongoDB with Go
{: #connect-mongodb-go}
{: go}

```go
package main

import (
	"context"
	"fmt"
	"log"
	"time"

	"go.mongodb.org/mongo-driver/mongo"
	"go.mongodb.org/mongo-driver/mongo/options"
)

func main() {
	// Connection parameters
	uri := "mongodb://localhost:27017" // MongoDB server URI
	databaseName := "your_database_name" // Name of the database to connect to

	// Set up the MongoDB client options
	clientOptions := options.Client().ApplyURI(uri)

	// Connect to MongoDB
	client, err := mongo.NewClient(clientOptions)
	if err != nil {
		log.Fatal("Failed to create MongoDB client:", err)
	}

	ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
	defer cancel()

	err = client.Connect(ctx)
	if err != nil {
		log.Fatal("Failed to connect to MongoDB:", err)
	}
	defer client.Disconnect(ctx)

	fmt.Println("Connected to MongoDB server")

	// Use the connected client to interact with the database
	// ...

	// Close the connection when finished
}
```
{: codeblock}
{: go}

In this code, you need to replace the following placeholders with your actual connection details:

`mongodb://localhost:27017`: Replace with the URI of your MongoDB server. The default port for MongoDB is 27017.
`your_database_name`: Replace with the name of the database you want to connect to.
The code sets up the MongoDB client options using the provided URI. It then creates a new MongoDB client using mongo.NewClient() and attempts to connect to the MongoDB server using client.Connect(). If the connection is successful, it prints a success message to the console.

After establishing the connection, you can use the client object to interact with the database. Remember to defer client.Disconnect() to close the connection when you are finished with your database operations.

Make sure to import the necessary packages by running the command go get go.mongodb.org/mongo-driver/mongo to download the MongoDB Go driver before running the code.
{: note}
