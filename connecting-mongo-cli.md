---
copyright:
  years: 2019
lastupdated: "2019-01-28"
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

The information the `mongo` shell needs to make a connection to your deployment is in the "cli" section of your [connection strings](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-connection-strings). The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Bin`||The recommended binary to create a connection; in this case it is `mongo`.
`Composed`||A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses `Arguments` as command line parameters.
`Environment`||A list of key/values you set as environment variables.
`Arguments`|`0...`|The information that is passed as arguments to the command shown in the Bin field.
`Certificate`|`Base64`|A self-signed certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded.
`Certificate`|`Name`|The allocated name for the self-signed certificate.
`Type`||The type of package that uses this connection information; in this case `cli`. 
{: caption="Table 2. `mongo`/`cli` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## `mongo` example



## Starting `mongo` from the IBM Cloud CLI

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating the client connection. For example, to connect to a deployment named  "example-mongo" with an "example-user", use the following command.

```
ibmcloud cdb deployment-connections -u example-user admin example-mongo --start
```
Or
```
ibmcloud cdb cxn -u example-user example-mongo -s
```

The command prompts for the user's password and then runs the `mongo` command-line client to connect to the database.

If you don't specify a user, the `cdb deployment-connections` commands return information for the admin user by default.
{: .tip}