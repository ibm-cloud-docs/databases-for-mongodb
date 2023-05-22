---
copyright:
  years: 2019, 2023
lastupdated: "2023-05-22"

keywords: mongodb, databases, admin user, service credentials, ops manager, mongodb managing users, roles, root account

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Managing Users and Roles
{: #user-management}

{{site.data.keyword.databases-for-mongodb_full}} deployments come with authentication enabled and use MongoDB's
[role-based access control](https://docs.mongodb.com/manual/core/authorization/){: external}.

When you provision a new deployment in {{site.data.keyword.cloud_notm}}, you are automatically given access to a MongoDB admin user. You can add users in the UI in _Service Credentials_, with the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin), or the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction). 

MongoDB centralizes user data in the `admin` database. You can list all users and their roles and database permissions [in the mongo shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongo-shell) by using the `show users` command.
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

## The admin User
{: #user-management-admin}

The admin user is intended for use as an administrative user. It is granted the MongoDB built-in roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite){: external}, [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin){: external}, and [`userAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#userAdminAnyDatabase){: external}.

All three roles provide privileges on all databases except local and config.

`userAdminAnyDatabase` is the role that provides the administrative power to the admin user. It provides the `listDatabases` action on the cluster as a whole. With `userAdminAnyDatabase`, [create and grant roles](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/){: external} to any other user on your deployment, including any of the MongoDB built-in roles. For example, to monitor your deployment, use `admin` to log in to the mongo shell and grant the [`clusterMonitor`](https://docs.mongodb.com/manual/reference/built-in-roles/#clusterMonitor){: external} role to any user (including itself).
```sh
db.grantRolesToUser(
    "admin",
    [
      { role: "clusterMonitor", db: "admin" }
    ]
)
```

## _Service Credential_ Users
{: #user-management-service-cred-users}

Users that you [create through _Service Credentials_](/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) are given the roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite){: external} and [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin){: external}.

If you need users that are created from _Service Credentials_ to have a different role, you can use the admin user to change their role.

## Creating users through the CLI and API
{: #user-management-cli-api}

Users that are created in the CLI and API are given the roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite){: external} and [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin){: external}.

If you need users to have a different role, you can use the admin user to change their role.

Users that are created directly from the API and CLI do not appear in _Service Credentials_, but you can add them if you choose.

## MongoDB created users and roles
{: #user-management-mongodb-users}

If the built-in users and roles do not suit your environment, you can [create users and roles](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#create-a-user-defined-role){: external} directly in MongoDB. The admin user for your deployment has the power to create any role or set of privileges for use on your deployment.

Users and roles that are created directly in MongoDB do not appear in _Service Credentials_ and are not integrated with your {{site.data.keyword.cloud_notm}} account or [IAM](/docs/databases-for-mongodb?topic=cloud-databases-iam).

## The `ibm` User
{: #user-management-ibm}

If you use the mongo shell to list the users on your deployment, you might notice a user that is named `ibm`. The `ibm` user is the internal root account that manages replication, cluster operations, and other functions that ensure the stability of your deployment. Changing or deleting to the `ibm` user is not advised and disrupts the stability of your deployment.

## The `ops_manager` users for MongoDB Enterprise Edition
{: #user-management-ops-manager}

The Ops Manager is only available in {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployments. The `ops_manager` user type has limited permissions. For more information, see the [Ops Manager documentation](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager). 