---

copyright:
  years: 2020, 2024
lastupdated: "2024-08-12"

keywords: mongodb, databases, logs, audit, enterprise

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# MongoDB Enterprise audit logging
{: #auditlogging}

Audit logging capabilities for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition are provided with {{site.data.keyword.la_full}}.

## Event types
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

## Audit logs
{: #audit-logs}

Audit events appear in {{site.data.keyword.logs_full}}.

For more information on {{site.data.keyword.databases-for-mongodb}} Enterprise Edition audit logging, see the [Cloud Logs Integration documentation](/docs/databases-for-mongodb?topic=databases-for-mongodb-logging) along with the [{{site.data.keyword.logs_full}} getting started tutorial](/docs/cloud-logs){: external}.

Audit events had been appearing in {{site.data.keyword.la_full}}, which is being deprecated. For older deployments you can still use [{{site.data.keyword.la_full}}](/docs/log-analysis) until March 2025. You may want to check that and make sure you are migrating to the new logging solution.
{: .note}
