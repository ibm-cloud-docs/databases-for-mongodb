---
copyright:
  years: 2019, 2023
lastupdated: "2023-07-05"

keywords: mongodb, databases, admin user, service credentials, ops manager, mongodb managing users, roles, root account

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Managing Users and Roles
{: #user-management-new}

{{site.data.keyword.databases-for-mongodb}} deployments come with authentication enabled and use MongoDB's [role-based access control](https://docs.mongodb.com/manual/core/authorization/){: external}.

Add users in the UI in _Service Credentials_, with the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin), or the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction).

## The admin user
{: #user-management-admin-user}
{: ui}
{: cli}
{: api}

When you provision a {{site.data.keyword.databases-for-mongodb}} deployment, an `admin` user is automatically created. 

Set the admin password before using it to connect.
{: important}

The `admin` user has the following permissions:

- [`userAdminAnyDatabase`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-userAdminAnyDatabase){: external} provides the same privileges as [`userAdmin`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-userAdmin){: external} on all databases except `local` and `config`. `userAdminAnyDatabase` provides the administrative power to the admin user. It provides the `listDatabases` action on the cluster as a whole. With `userAdminAnyDatabase`, [create and grant roles](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/){: external} to any other user on your deployment, including any of the MongoDB built-in roles. For example, to monitor your deployment, use `admin` to log in to the mongo shell and grant the [`clusterMonitor`](https://docs.mongodb.com/manual/reference/built-in-roles/#clusterMonitor){: external} role to any user (including itself).
   Use a command like:

   ```sh
   db.grantRolesToUser(
    "admin",
    [
      { role: "clusterMonitor", db: "admin" }
    ]
   )
   ```
   {: pre}

- [`readWriteAnyDatabase`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-readWriteAnyDatabase){: external} provides the same privileges as [`readWrite`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-readWrite){: external} on all databases except `local` and `config`. 
- [`dbAdminAnyDatabase`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-dbAdminAnyDatabase){: external} provides the same privileges as [`dbAdmin`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-dbAdmin){: external} on all databases except `local` and `config`.

### Setting the Admin Password in the UI
{: #user-management-set-admin-password-ui}
{: ui}

To set the password through the {{site.data.keyword.cloud_notm}} dashboard, select __Manage__ from the service dashboard. Open the _Settings_ tab, and use the _Change Database Admin Password_ to set a new admin password.

### Setting the Admin Password in the CLI
{: #user-management-set-admin-password-cli}
{: cli}

Use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI {{site.data.keyword.databases-for}} plug-in to set the admin password.

For example, to set the admin password for a deployment named `example-deployment`, use the following command:

```sh
ibmcloud cdb user-password example-deployment admin <newpassword>
```
{: pre}

### Setting the Admin Password in the API
{: #user-management-set-admin-password-api}
{: api}

The Foundation Endpoint that is shown on the Overview panel Deployment Details section of your service provides the base URL to access this deployment through the API. Use it with the [Set specified user's password](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#changeuserpassword){: external} endpoint to set the admin password.

```sh
curl -X PATCH `https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/users/admin` \
-H `Authorization: Bearer <>` \
-H `Content-Type: application/json` \ 
-d `{"password":"newrootpasswordsupersecure21"}` \
```
{: pre}

## Managing Users and Roles through the UI
{: #user-management-ui}
{: ui}

1. Go to the service dashboard for your service.
2. Click _Service Credentials_ to open the _Service Credentials_ section.
3. Click __New Credential__.
4. Choose a descriptive name for your new credential. 
5. (Optional) Specify whether the new credentials use a public or private endpoint. Use either `{ "service-endpoints": "public" }` / `{ "service-endpoints": "private" }` in the _Add Inline Configuration Parameters_ field to generate connection strings that use the specified endpoint. Use of the endpoint is not enforced. It just controls which hostnames are in the connection strings. Public endpoints are generated by default.
6. Click _Add_ to provision the new credentials. A username and password, and an associated MongoDB user is auto-generated.

The new credentials appear in the table, and the connection strings are available as JSON in a click-to-copy field under _View Credentials_.

Creating a user from the CLI or API doesn't automatically populate that user's connection strings into _Service Credentials_. If you want to add them there, you can create a new credential with the existing user information. Enter the username and password in the JSON field under _Add Inline Configuration Parameters_. For example, {"existing_credentials":{"username":"Robert","password":"supersecure"}}. Basically, you send in the username and password, and _Service Credentials_ generates the connection strings with the credentials filled in.

Generating credentials from an existing user does not check for or create that user. 
{: .tip}

Users that you [create through _Service Credentials_](/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) are given the roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite){: external} and [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin){: external}.

If you need users that are created from _Service Credentials_ to have a different role, you can use the admin user to change their role.


## Managing Users and Roles through the CLI
{: #user-management-cli}
{: cli}

Users that are created in the CLI are given the roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite){: external} and [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin){: external}.

If you need users to have a different role, you can use the `admin` user to change their role.

Users that are created directly from the CLI do not appear in _Service Credentials_, but you can add them.

If you manage your service through the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/cli?topic=cli-install-ibmcloud-cli), create a new user with `cdb user-create`. For example, to create a new user for an "example-deployment", use the following command:

```sh
ibmcloud cdb user-create example-deployment <newusername> <newpassword>
```
{: pre}

When the task finishes, retrieve the new user's connection strings with the `ibmcloud cdb deployment-connections` command.

MongoDB centralizes user data in the `admin` database. List all users and their roles and database permissions [in the mongo shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongo-shell) by using the `show users` command.

```sh
ibmcloud cdb deployment-connections --start -u admin mongodb-production
Database Password>>
MongoDB shell version v4.0.3
connecting to: mongodb://....
....
replset:PRIMARY> use admin
switched to db admin
replset:PRIMARY> show users
```
{: pre}


## Managing Users and Roles through the API
{: #user-management-api}
{: api}

Users that are created in the API are given the roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite){: external} and [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin){: external}.

If you need users to have a different role, use the admin user to change their role.

Users that are created directly from the API do not appear in _Service Credentials_, but you can add them.

The _Foundation Endpoint_ that is shown on the _Overview_ section of your service provides the base URL to access this deployment through the API. To create and manage users, use the base URL with the [`/users` endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api#creates-a-database-level-user).

The command looks like: 

```sh
curl -X POST 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/users' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"username":"jane_smith", "password":"newsupersecurepassword"}'
```
{: pre}

To retrieve a user's connection strings, use the base URL with the `/users/{userid}/connections` endpoint. 

## MongoDB created users and roles
{: #user-management-mongodb-users}

If the built-in users and roles do not suit your environment, [create users and roles](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#create-a-user-defined-role){: external} directly in MongoDB. The admin user for your deployment has the power to create any role or set of privileges for use on your deployment.

Users and roles that are created directly in MongoDB do not appear in _Service Credentials_ and are not integrated with your {{site.data.keyword.cloud_notm}} account or [IAM](/docs/databases-for-mongodb?topic=cloud-databases-iam).

## The `ibm` User
{: #user-management-ibm}

If you use the mongo shell to list the users on your deployment, you might notice a user that is named `ibm`. The `ibm` user is the internal root account that manages replication, cluster operations, and other functions that ensure the stability of your deployment. Changing or deleting to the `ibm` user is not advised and disrupts the stability of your deployment.

## The `ops_manager` users for MongoDB Enterprise Edition
{: #user-management-ops-manager}

The Ops Manager is only available in {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployments. The `ops_manager` user type has limited permissions. For more information, see the [Ops Manager documentation](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager).
