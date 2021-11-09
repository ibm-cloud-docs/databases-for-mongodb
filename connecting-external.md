---
copyright:
  years: 2017, 2020
lastupdated: "2021-11-09"

keywords: mongodb, databases, connecting, pymongo, java driver, self-signed certificate, mongodbee

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:term: .term}

# Connecting an external application
{: #mongodb-external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-mongodb_full}}. Each deployment has connection strings specifically for drivers and applications. Connection strings are displayed in the *Endpoints* panel of your deployment's *Overview*, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections) and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The connection strings can be used by any of the users you have created on your deployment. While you can use the admin user for all of your connections and applications, it might be better to create users specifically for your applications to connect with. Documentation on generating credentials is on the [Creating Users and Getting Connection Strings](/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) page.

## Using Connection Information
{: #mongodb-using-connection-info}

All the information a driver needs to make a connection to your deployment is in the "MongoDB" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Type`||Type of connection - for MongoDB, it is "URI"
`Scheme`||Scheme for a URI - for MongoDB, it is "mongodb"
`Path`||Path for a URI - for MongoDB, it is the database name. When provisioning a MongoDB instance for the first time, the default database for the user to connect to is `admin`.
`Authentication`|`Username`|The username that you use to connect.
`Authentication`|`Password`|A password for the user - might be shown as `$PASSWORD`
`Authentication`|`Method`|How authentication takes place; "direct" authentication is handled by the driver. Mongo 3.6 uses SCRAM SHA 1, whereas Mongo 4.2 uses SHA 256
`Hosts`|`0...`|A hostname and port to connect to
`Composed`|`0...`|A URI combining Scheme, Authentication, Host, Path, and Replica Set name.
`Certificate`|`Name`|The allocated name for the self-signed certificate for database deployment
`Certificate`|Base64|A base64 encoded version of the certificate.
{: caption="Table 1. mongodb/URI connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

Many MongoDB drivers are able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example,

```shell
mongodb://admin:$PASSWORD@d5eeee66-5bc4-498a-b73b-1307848f1eac.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:30484/<database name>?authSource=admin&replicaSet=replset
```

The `replicaSet` query parameter contains the replica set name for your deployment. It is probably `replset`. Some drivers and applications need it passed in separately.
{: .tip}

The following example uses the information from your connection string and the [MongoDB Java Driver](http://mongodb.github.io/mongo-java-driver/?jmp=docs) to connect to your database.

```java
public class MongodbConnect {
    private static Logger log = LoggerFactory.getLogger(LoggerFactory.class);

    public static void main(String[] args) {

        System.setProperty("javax.net.ssl.trustStore", "path/to/keystore");
        System.setProperty("javax.net.ssl.trustStorePassword", "store_password");

        // make sure you append ssl=true to the connection URI
        final String mongoURI = "mongodb://user:password@host:port,host:port/?authSource=admin&replicaSet=replset&ssl=true";

        MongoClient mongoClient = MongoClients.create(mongoURI);
        boolean testDB = false;
        
        // this loop will continue attempting to connect to the database until the admin database is found
        while (!testDB) {
            try {
                // check if you can connect to the database by checking for
                // the presence of the admin database. If the admin databases isn't found
                // then you're not connected.
                MongoIterable<String> databases = mongoClient.listDatabaseNames();
                for (String name: databases) {
                    if (name.contains("admin")) {
                        System.out.println("admin found...");
                        testDB = true;
                    }
                }
            } catch (Exception e) {
                log.info(e.getMessage());
            }
        }

        // close connection
        mongoClient.close();

    }
}
```
{: codeblock}

This next example uses information from your connection string and the Python driver [pymongo](https://docs.mongodb.com/drivers/pymongo/) to connect to your database. This is just a simple connection example, without error handling or retry logic and may not be suitable for production.

```python
import pymongo
from pymongo import MongoClient
from pymongo.errors import ConnectionFailure


client = MongoClient(
    "mongodb://admin:$PASSWORD@host.databases.appdomain.cloud:30484/<database name>?authSource=adminreplicaSet=replset",
    ssl=True,
    ssl_ca_certs="/path/to/cert/ca-certificate.crt"
)

try:
    db_list = client.list_database_names()
    print("List of databases:")
    print(db_list)

except ConnectionFailure as err:
    print("Unable to connect to database")
```
{: codeblock}

This final example uses the [MongoDB Node.js driver](http://mongodb.github.io/node-mongodb-native/3.5/)

```javascript
const MongoClient = require("mongodb").MongoClient;

let connectionString = "mongodb://<username>:<password>@<host>:<port>,<host>:<port>/<database>?authSource=admin&replicaSet=replset";

let options = {
    tls: true,
    tlsCAFile: `/path/to/cert`,
    useUnifiedTopology: true 
};

// connects to a MongoDB database
MongoClient.connect(connectionString, options, function (err, db) {
    if (err) {
        console.log(err);
    } else {
       // lists the databases that exist in the deployment
        db.db('example').admin().listDatabases(function(err, dbs) {
            console.log(dbs.databases);
            db.close();
        });
    }
});
```
{: codeblock}

## Driver TLS and self-signed certificate support
{: #mongodb-tls-certificate-support}

All connections to {{site.data.keyword.databases-for-mongodb}} are TLS 1.2 enabled, so the driver you use to connect needs to be able to support encryption. Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection.

### Using the self-signed certificate
{: #mongodb-using-cert}

1. Copy the certificate information from the _Endpoints_ panel or the Base64 field of the connection information.
2. If needed, decode the Base64 string into text.
3. Save the certificate to a file. (You can use the Name that is provided or your own file name). *
4. Provide the path to the certificate to the driver or client.

 *For MacOS, ensure sure you have the certificate imported into your trust store, and mark the certificate as `trust always`.
{: .tip}

### CLI plug-in support for the self-signed certificate
{: #mongodb-cli-plugin}

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver.

## Other Language Drivers
{: #mongodb-other-lang-drivers}

MongoDB has a vast array of language drivers. The table covers a few of the most common. If you're looking for more languages, try the [MongoDB.org Driver List](http://www.mongodb.org/display/DOCS/Drivers).
