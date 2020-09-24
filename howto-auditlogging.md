---

Copyright:
  years: 2020
lastupdated: "2020-08-26"

keywords: mongodb, databases, logs, audit, logdna, enterprise

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Audit logging with MongoDB Enterprise Edition
{: #auditlogging}

Audit logging capabilities for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition are provided with {{site.data.keyword.la_full}}.

## Event Types

{{site.data.keyword.databases-for-mongodb}} Enterprise Edition provides audit logging capabilities based on the following event types: 

* `authenticate`
* `createCollection`
* `createDatabase`
* `createIndex`
* `renameCollection`
* `dropCollection`
* `dropDatabase`
* `dropIndex`
* `createUser`
* `dropUser`
* `dropAllUsersFromDatabase`
* `updateUser`
* `grantRolesToUser`
* `revokeRolesFromUser`
* `createRole`
* `updateRole`
* `dropRole`
* `dropAllRolesFromDatabase`
* `grantRolesToRole`
* `revokeRolesFromRole`
* `grantPrivilegesToRole`
* `revokePrivilegesFromRole`

The event types for {{site.data.keyword.databases-for-mongodb}} Enterprise Edition are fixed and not configurable. 
{: .note}

## Audit Logs

Audit events appear in {{site.data.keyword.la_full}}.

For more information on {{site.data.keyword.databases-for-mongodb}} Enterprise Edition audit logging, review the [Log Analysis Integration documentation](/docs/databases-for-mongodb?topic=cloud-databases-logging) along with the [{{site.data.keyword.la_full}}](/docs/Log-Analysis-with-LogDNA) getting started tutorial.

