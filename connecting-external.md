---
copyright:
  years: 2017,2019
lastupdated: "2019-04-10"

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an external application
{: #external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-mongodb_full}}. Each deployment has connection strings specifically for drivers and applications. Connection strings are displayed in the _Connections_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The connection strings can be used by any of the credentials you have created on your deployment. While you can use the admin user for all of your connections and applications, it might be better to generate credentials specifically for your applications to connect with. Documentation on generating credentials is on the [Getting Credentials and Connection Strings](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) page.

## Using Connection Information

All the information a driver needs to make a connection to your deployment is in the "MongoDB" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Type`||Type of connection - for MongoDB, it is "URI"
`Scheme`||Scheme for a URI - for MongoDB, it is "mongodb"
`Path`||Path for a URI - for MongoDB, it is the database name. The default is `ibmclouddb`.
`Authentication`|`Username`|The username that you use to connect.
`Authentication`|`Password`|A password for the user - might be shown as `$PASSWORD`
`Authentication`|`Method`|How authentication takes place; "direct" authentication is handled by the driver.
`Hosts`|`0...`|A hostname and port to connect to
`Composed`|`0...`|A URI combining Scheme, Authentication, Host, and Path
`Certificate`|`Name`|The allocated name for the self-signed certificate for database deployment
`Certificate`|Base64|A base64 encoded version of the certificate.
{: caption="Table 1. `mongodb`/`URI` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

Many MongoDB drivers are able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example,
```
mongodb://admin:$PASSWORD@d5eeee66-5bc4-498a-b73b-1307848f1eac.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:30484/ibmclouddb?authSource=admin
```

## Language Drivers

MongoDB has a vast array of language drivers. The table covers a few of the most common.

Language|Driver|Documentation
----------|-----------
Node.js|`mongodb`|[Link](http://mongodb.github.io/node-mongodb-native/3.1/)
Go|`mongo`|[Link](https://godoc.org/github.com/mongodb/mongo-go-driver/mongo)
Python|`pymongo`|[Link](http://api.mongodb.com/python/current/index.html)
Java|`mongodb-driver-sync`|[Link](http://mongodb.github.io/mongo-java-driver/?jmp=docs)
{: caption="Table 2. Common MongoDB drivers" caption-side="top"}

If you're looking for languages that are not in the table, try the [MongoDB.org Driver List](http://www.mongodb.org/display/DOCS/Drivers).
{: tip}

## Driver TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-mongodb}} are TLS 1.2 enabled, so the driver you use to connect needs to be able to support encryption. Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection. 

### Using the self-signed certificate

1. Copy the certificate information from the _Connections_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.

### CLI plug-in support for the self-signed certificate

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver.






 
