---
copyright:
  years: 2019, 2020
lastupdated: "2020-10-23"

keywords: mongodb, databases

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Managing Users and Roles
{: #user-management}

{{site.data.keyword.databases-for-mongodb_full}} deployments come with authentication enabled and use MongoDB's
[role-based access control](https://docs.mongodb.com/manual/core/authorization/).

When you provision a new deployment in {{site.data.keyword.cloud_notm}}, you are automatically given access to a MongoDB admin user. You can also add users in the _Service Credentials_ panel, the cloud databases CLI plug-in, or the cloud databases API. 

MongoDB centralizes user data in the `admin` database. You can list all users and their roles and database permissions [in the mongo shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongo-shell) by using the `show users` command.
```
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

The admin user is intended for use as an administrative user. It is granted the MongoDB built-in roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite), [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin), and [`userAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#userAdminAnyDatabase).

All three roles provide privileges on all databases except local and config.

`userAdminAnyDatabase` is the role that provides the administrative power to the admin user. It provides the `listDatabases` action on the cluster as a whole. It also provides you the ability to [create and grant roles](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/) to any other user on your deployment. This includes any of the MongoDB built-in roles. For example, if you need to set up monitoring for your deployment, you can use admin to log into the mongo shell and grant the [`clusterMonitor`](https://docs.mongodb.com/manual/reference/built-in-roles/#clusterMonitor) role to any user (including itself).
```
db.grantRolesToUser(
    "admin",
    [
      { role: "clusterMonitor", db: "admin" }
    ]
)
```

## _Service Credential_ Users

Users that you [create through the _Service Credentials_ panel](/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) are given the roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite) and [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin).

If you need users that are created from _Service Credentials_ to have a different role, you can use the admin user to change their role.

## Users that are created through the CLI and the API

Users that are created in the CLI and API are given the roles [`readWriteAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#readWrite) and [`dbAdminAnyDatabase`](https://docs.mongodb.com/manual/reference/built-in-roles/#dbAdmin).

If you need users to have a different role, you can use the admin user to change their role.

Users that are created directly from the API and CLI do not appear in _Service Credentials_, but you can add them if you choose.

## MongoDB created users and roles

If the built-in users and roles do not suit your environment, you can [create users and roles](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#create-a-user-defined-role) directly in MongoDB. The admin user for your deployment has the power to create any role or set of privileges for use on your deployment.

Users and roles that are created directly in MongoDB do not appear in _Service Credentials_ and are not integrated with your {{site.data.keyword.cloud_notm}} account or [IAM](/docs/databases-for-mongodb?topic=cloud-databases-iam).

## The `ibm` User

If you use the mongo shell to list the users on your deployment, you might have noticed a user that is named `ibm`. The `ibm` user is the internal root account that manages replication, cluster operations, and other functions that ensure the stability of your deployment. Changing or deleting to the `ibm` user is not advised and will disrupt the stability of your deployment.

## The `ops_manager` users for MongoDB Enterprise Edition

The Ops Manager is only available in {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployments. The `ops_manager` user type has limited permissions. For details on creating an Ops Manager user, please see the [Ops Manager documentation](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager). 