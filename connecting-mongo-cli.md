---
copyright:
  years: 2019
lastupdated: "2019-04-10"

keywords: mongodb, databases, mongo shell

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Connecting with the `mongo` Shell
{: #mongo-shell}

You can access your MongoDB database from a command-line client, which allows for direct interaction and monitoring of the data structures that are created within the database. You can use it to query and update data as well as performing administrative operations and monitoring performance. 

## Installing 

The `mongo` shell is available as part of the MongoDB distribution, which can be downloaded from [the MongoDB Download Center](https://www.mongodb.com/download-center/community?jmp=docs).

## Connecting

Connection strings are displayed in the _Endpoints_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

![CLI Endpoints panel](images/cli-endpoints-pane.png)

The information the `mongo` shell needs to make a connection to your deployment is in the "cli" section the connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Bin`||The recommended binary to create a connection; in this case it is `mongo`.
`Composed`||A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses `Arguments` as command-line parameters.
`Environment`||A list of key/values you set as environment variables.
`Arguments`|`0...`|The information that is passed as arguments to the command shown in the Bin field.
`Certificate`|`Base64`|A self-signed certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded.
`Certificate`|`Name`|The allocated name for the self-signed certificate.
`Type`||The type of package that uses this connection information; in this case `cli`. 
{: caption="Table 1. `mongo`/`cli` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## `mongo` example

```
mongo -u admin -p $PASSWORD --ssl --sslCAFile c5f07836-d94c-11e8-a2e9-62ec2ed68f84 --authenticationDatabase admin --host replset/bd574ce4-7b36-4274-9976-96db98a3ac10-0.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:30484,bd574ce4-7b36-4274-9976-96db98a3ac10-1.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:30484
```

* `mongo` - The command itself. 
* `--ssl --sslCAFile` - The path and name of the self-signed certificate for your deployment.
* `-u` - The parameter for the username.
* `-p` - The parameter for the password. 
* `--authenticationDatabase` - The database where the user and its credentials are created and stored.
* `--host` - The replica set name, followed by a `/`, and the hosts of the replica set members. 

## Starting `mongo` from the IBM Cloud CLI

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating the client connection. For example, to connect to a deployment named  "example-mongo" with an "example-user", use the following command.

```
ibmcloud cdb deployment-connections --start -u example-user example-mongo
```

The command prompts for the user's password and then runs the `mongo` command-line client to connect to the database.

The option `--start` must come before the parameters, otherwise connection information is just returned and the `mongo` shell is not started.
{: .tip}

## Using the self-signed certificate

1. Copy the certificate information from the _Endpoints_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the `--sslCAFile` parameter.

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the `--sslCAFile` parameter.

