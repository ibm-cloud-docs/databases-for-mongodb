---

copyright:
  years: 2020
lastupdated: "2021-12-21"

keywords: mongodb, databases, logs, audit, enterprise

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# MongoDB Enterprise Audit logging
{: #auditlogging}

Audit logging capabilities for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition are provided with {{site.data.keyword.la_full}}.

## Event Types
{: #event-types}

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
{: #audit-logs}

Audit events appear in {{site.data.keyword.la_full}}.

For more information on {{site.data.keyword.databases-for-mongodb}} Enterprise Edition audit logging, review the [Log Analysis Integration documentation](/docs/databases-for-mongodb?topic=cloud-databases-logging) along with the [{{site.data.keyword.la_full}}](/docs/log-analysis) getting started tutorial.
