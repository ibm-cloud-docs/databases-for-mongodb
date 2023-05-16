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

```javascript
const { MongoClient } = require('mongodb');

// Connection URL
const url = 'mongodb://localhost:27017';

// Database Name
const dbName = 'myproject';

// Create a new MongoClient
const client = new MongoClient(url);

async function run() {
  try {
    // Connect to the MongoDB server
    await client.connect();
    console.log('Connected successfully to server');

    // Select a database
    const db = client.db(dbName);

    // Select a collection
    const collection = db.collection('mycollection');

    // Insert a document into a collection
    const document = {
      title: 'MongoDB Tutorial',
      description: 'Learn MongoDB with JavaScript',
      by: 'user',
      url: 'http://www.example.com',
      tags: ['mongodb', 'database', 'NoSQL'],
      likes: 100
    };
    const insertResult = await collection.insertOne(document);
    console.log('Document inserted:', insertResult.insertedId);

    // Find documents in a collection
    const findResult = await collection.find({}).toArray();
    console.log('Found the following records:');
    console.log(findResult);

    // Update a document in a collection
    const filter = { title: 'MongoDB Tutorial' };
    const update = { $set: { likes: 200 } };
    const updateResult = await collection.updateOne(filter, update);
    console.log('Document updated:', updateResult.modifiedCount);

    // Delete a document from a collection
    const deleteResult = await collection.deleteOne(filter);
    console.log('Document deleted:', deleteResult.deletedCount);
  } catch (error) {
    console.log('Error:', error);
  } finally {
    // Close the MongoDB connection
    await client.close();
  }
}

run();

```
{: codeblock}
{: javascript}

```python
from pymongo import MongoClient

# Connection URL
url = "mongodb://localhost:27017"

# Database Name
db_name = "myproject"

# Create a MongoClient
client = MongoClient(url)

# Select a database
db = client[db_name]

# Select a collection
collection = db.mycollection

# Insert a document into a collection
document = {
    "title": "MongoDB Tutorial",
    "description": "Learn MongoDB with Python",
    "by": "user",
    "url": "http://www.example.com",
    "tags": ["mongodb", "database", "NoSQL"],
    "likes": 100
}
collection.insert_one(document)

# Find documents in a collection
cursor = collection.find({})
for document in cursor:
    print(document)

# Update a document in a collection
filter = {"title": "MongoDB Tutorial"}
update = {"$set": {"likes": 200}}
collection.update_one(filter, update)

# Delete a document from a collection
filter = {"title": "MongoDB Tutorial"}
collection.delete_one(filter)
```
{: codeblock}
{: python}

```go
package main

import (
	"context"
	"fmt"
	"log"

	"go.mongodb.org/mongo-driver/mongo"
	"go.mongodb.org/mongo-driver/mongo/options"
	"go.mongodb.org/mongo-driver/mongo/readpref"
)

// Connection URL
const url = "mongodb://localhost:27017"

// Database Name
const dbName = "myproject"

// Collection Name
const collectionName = "mycollection"

// Document struct
type Document struct {
	Title       string   `bson:"title"`
	Description string   `bson:"description"`
	By          string   `bson:"by"`
	URL         string   `bson:"url"`
	Tags        []string `bson:"tags"`
	Likes       int      `bson:"likes"`
}

func main() {
	// Create a MongoDB client
	clientOptions := options.Client().ApplyURI(url)
	client, err := mongo.Connect(context.Background(), clientOptions)
	if err != nil {
		log.Fatal(err)
	}
	defer func() {
		if err = client.Disconnect(context.Background()); err != nil {
			log.Fatal(err)
		}
	}()

	// Ping the MongoDB server
	err = client.Ping(context.Background(), readpref.Primary())
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("Connected successfully to server")

	// Get the database and collection
	db := client.Database(dbName)
	collection := db.Collection(collectionName)

	// Insert a document into the collection
	document := Document{
		Title:       "MongoDB Tutorial",
		Description: "Learn MongoDB with Go",
		By:          "ChatGPT",
		URL:         "http://www.example.com",
		Tags:        []string{"mongodb", "database", "NoSQL"},
		Likes:        100,
	}
	insertResult, err := collection.InsertOne(context.Background(), document)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Document inserted:", insertResult.InsertedID)

	// Find documents in the collection
	cursor, err := collection.Find(context.Background(), nil)
	if err != nil {
		log.Fatal(err)
	}
	var documents []Document
	err = cursor.All(context.Background(), &documents)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Found the following records:")
	for _, doc := range documents {
		fmt.Println(doc)
	}

	// Update a document in the collection
	filter := Document{Title: "MongoDB Tutorial"}
	update := Document{Likes: 200}
	updateResult, err := collection.UpdateOne(context.Background(), filter, bson.M{"$set": update})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Document updated:", updateResult.ModifiedCount)

	// Delete a document from the collection
	deleteResult, err := collection.DeleteOne(context.Background(), filter)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Document deleted:", deleteResult.DeletedCount)
}
```
{: codeblock}
{: python}
