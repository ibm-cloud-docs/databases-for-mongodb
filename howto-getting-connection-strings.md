---

copyright:
  years: 2018, 2023
lastupdated: "2023-06-15"

keywords: mongodb, databases, connection strings

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}


# Getting Connection Strings
{: #connection-strings}

Connection strings allow you to establish a connection between your application and your {{site.data.keyword.databases-for-mongodb}} instance.

## Getting Connection Strings in the UI
{: #connection-strings-ui}
{: ui}

Follow these steps to retrieve your {{site.data.keyword.databases-for-mongodb}} instance connection strings:

1. In your deployment's **Overview**, scroll down to the *Endpoints* section. 
1. Within the *Endpoints* section, you find: 
   - **Connect using a CLI** - This section contains information for connecting to your deployment through the [{{site.data.keyword.IBM_notm}} CLI](https://www.ibm.com/cloud/cli){: external}.
   - **Connect using a MongoDB Enterprise Client** - This section allows you to get a TLS certificate and connect to your deployment.

Connection Strings for your deployment are displayed on the *Dashboard Overview*, in the *Endpoints* section. These strings can be used with any set of credentials that you generate.

## Getting Connection Strings in the CLI
{: #connection-strings-cli}
{: cli}

You can also grab connection strings from the [CLI](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections).
```sh
ibmcloud cdb deployment-connections example-deployment -u <newusername> [--endpoint-type <endpoint type>]
```
{: pre}

Full connection information is returned by the `ibmcloud cdb deployment-connections` command with the `--all` flag. To retrieve all the connection information for a deployment named "example-deployment", use the following command.
```sh
ibmcloud cdb deployment-connections example-deployment -u <newusername> --all [--endpoint-type <endpoint type>]
```
{: pre}

If you don't specify a user, the `deployment-connections` commands return information for the admin user by default. If you don't specify an endpoint type, the connection string returns the public endpoint by default. If your deployment only has a private endpoint, you must specify `--endpoint-type private` or the commands return an error. The user and endpoint type is not enforced. You can use any user on your deployment with either endpoint (if both exist on your deployment).

To use the `ibmcloud cdb` CLI commands, you must [install the {{site.data.keyword.databases-for}} plug-in](/docs/databases-for-mongodb?topic=databases-cli-plugin-cdb-reference#installing-the-cloud-databases-cli-plug-in).
{: .tip}

## Getting Connection Strings in the API
{: #connection-strings-api}
{: api}

To retrieve user's connection strings from the API, use the [`/users/{userid}/connections`](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026) endpoint. You must specify in the path which user and which type of endpoint (public or private) should be used in the returned connection strings. The user and endpoint type is not enforced. You can use any user on your deployment with either endpoint (if both exist on your deployment).
```sh
curl -X GET -H "Authorization: Bearer $APIKEY" 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/users/{userid}/connections/{endpoint_type}'
```
{: pre}

## Additional Users and Connection Strings
{: #connection-strings-additional-users-strings}

Access to your {{site.data.keyword.databases-for-mongodb}} deployment is not limited to the admin user. You can create more users and retrieve connection strings specific to them by using the *Service Credentials* section, the {{site.data.keyword.IBM_notm}} CLI, or through the {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} API. 

All users on your deployment can use the connection strings, including connection strings for either public or private endpoints.

When you create a user, it is assigned certain database roles and privileges. For more information, see the [Managing Users and Roles](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management) page.

## Connection String Breakdown
{: #connection-string-breakdown}

### The CLI Section
{: #connection-strings-cli-section}
{: cli}

The "CLI" section contains information that is suited for connecting with the [`mongo` Shell](https://www.mongodb.com/docs/v4.4/mongo/){: external}.

| Field Name | Index | Description |
| ---------- | ----- | ----------- |
| `Bin` | | The recommended binary to create a connection; in this case it is `mongo`. |
| `Composed` | | A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses  |`Arguments` as command-line parameters.
| `Environment` | | A list of key/values you set as environment variables. |
| `Arguments` | `0...` | The information that is passed as arguments to the command shown in the Bin field. |
| `Certificate` | `Base64` | A self-signed certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded. |
| `Certificate` | `Name` | The allocated name for the self-signed certificate. |
| `Type` | | The type of package that uses this connection information; in this case `cli`.  |
{: caption="Table 1. mongo/cli connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.
