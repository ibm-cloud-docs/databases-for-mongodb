---
copyright:
  years: 2019, 2023
lastupdated: "2023-05-26"

keywords: mongodb, databases, mongodb compass, mongodbee, mongodb enterprise, mongodb ee provision, mongodb compass, mongodb ops manager

subcollection: databases-for-mongodb

content-type: tutorial
services: 
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}
{:go: .ph data-hd-programlang="go"}

# Getting Started 
{: #getting-started-new}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

Now that you've provisioned your {{site.data.keyword.databases-for-mongodb_full}} instance, let's create a basic ToDo app. This app will demonstrate how to:
- connect to your {{site.data.keyword.databases-for-mongodb}} instance
- create a collection
- add data
- update data

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

## Create your ToDo App
{: #getting-started-create-todo-app}

Create a ToDo app that allows you to add and update items on a ToDo list. 

The examples below use multiple programming languages. Switch between the languages of your choice for the relevant steps and code snippets.
{: note}

The ToDo app in this tutorial sets up a basic Express server that connects to a MongoDB database, provides endpoints for retrieving and creating todos, and listens for incoming requests on port `3000`.
{: javascript}

The ToDo app in this tutorial sets up a basic Flask server that connects to a MongoDB database, provides endpoints for retrieving and creating todos, and listens for incoming requests on port `3000`.
{: python}

The ToDo app in this tutorial sets up a basic HTTP server using the Gorilla Mux router, connects to a MongoDB database, provides handlers for retrieving and creating todos, and listens for incoming requests on port 3000.
{: go}

First, import the necessary modules, using a command like:
{: javascript}
{: python}

First, import the necessary packages: 
{: go}

```go
import (
	"log"
	"net/http"

	"github.com/gorilla/mux"
	"go.mongodb.org/mongo-driver/mongo"
	"go.mongodb.org/mongo-driver/mongo/options"
)
```
{: pre}
{: go}

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
app = Flask(__name__)
```
{: pre}
{: python}

Now, establish a connection to MongoDB:
{: javascript}
{: python}

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

Now, define handlers for retrieving and creating todos:

Now, set up middleware for parsing request bodies:
{: javascript}

```javascript
app.use(express.urlencoded({ extended: true }));
app.use(express.json());
// This code configure Express middleware to parse URL-encoded and JSON request bodies.
```
{: pre}
{: javascript}

Now, define a route for retrieving todos:
{: javascript}
{: python}

```javascript
app.get('/todos', (req, res) => {
  collection.find().toArray()
    .then(results => {
      res.json(results);
    })
    .catch(error => {
      console.error(error);
      res.status(500).send('Internal Server Error');
    });
});
// This code defines a GET route at /todos. When a GET request is made to this endpoint, it queries the MongoDB collection to retrieve all todos and sends the results as a JSON response.
```
{: pre}
{: javascript}

```python
@app.route('/todos', methods=['GET'])
def get_todos():
    todos = list(collection.find())
    return jsonify(todos)
```
{: pre}
{: python}

Define a route for creating todos:
{: javascript}
{: python}

```javascript
app.post('/todos', (req, res) => {
  collection.insertOne(req.body)
    .then(result => {
      res.json(result.ops[0]);
    })
    .catch(error => {
      console.error(error);
      res.status(500).send('Internal Server Error');
    });
});
// This code defines a POST route at /todos. When a POST request is made to this endpoint, it takes the request body (assuming it contains a JSON payload with a task property) and inserts it as a new todo document into the MongoDB collection. It then responds with the inserted todo document.
```
{: pre}
{: javascript}

```python
@app.route('/todos', methods=['POST'])
def create_todo():
    new_todo = request.json
    result = collection.insert_one(new_todo)
    return jsonify(new_todo)
# This code defines a route for handling POST requests to the /todos endpoint. When a POST request is made to this endpoint, it expects a JSON payload containing a new todo. It inserts the new todo into the MongoDB collection and returns the inserted todo as a JSON response.
```
{: pre}
{: python}

Now, start the Express server, listening on the specified port.
{: javascript}

Now, start the Flask server, listening on port `3000`.
{: python}

```javascript
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
// This line of code starts the Express server, listening on the specified port.
```
{: pre}
{: javascript}

## Create a ToDo App with MongoDB Compass
{: #create-todo-app-mongodb-compass}
{: step}

Use MongoDB Compass to perform similar tasks as the code examples.

### Connect with MongoDB Compass
{: #connecting-mongodb-compass-new}

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

### Create and retrieve with MongoDB Compass
{: #connecting-mongodb-compass-create-retrieve-todos}

By following these steps, you can use MongoDB Compass to create a database, define a collection, and insert documents representing todos. However, to build a complete todo app, you would typically use a programming language (such as JavaScript, Python, or Go) and a framework or library to create an application that interacts with the database, handles CRUD operations, and provides a user interface for managing and displaying todos.

Create a new database and collection:

- Once connected, you'll see a list of databases on the left-hand side of the Compass interface.
- Right-click on the "Databases" section and choose "Create Database".
- Enter a name for the database (e.g., "todoapp") and click "Create".
- With the newly created database selected, right-click on the "Collections" section and choose "Create Collection".
- Enter a name for the collection (e.g., "todos") and click "Create".

Add documents to the collection:

- Select the "todos" collection in the left-hand sidebar.
- Click on the "Insert Document" button in the top-right corner of the Compass interface.
- Enter the details of the todo, such as "task", "priority", and any other relevant fields, in the document editor.
- Click "Insert" to add the document to the collection.
- Repeat this step to add more todo documents.
