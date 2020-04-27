---

Copyright:
  years: 2019
lastupdated: "2019-11-25"

keywords: mongodb, databases

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability
{: #high-availability}

{{site.data.keyword.databases-for-mongodb_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-mongodb}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. A deployment consists of two data nodes, one a primary node and the other as a secondary node. They are configured as a replication set without sharding, so both contain a copy of your data. An arbiter node is present to handle failover. If the primary node fails or becomes unavailable, the arbiter helps break ties when promoting a secondary to be the new primary. The failed node can then rejoin as secondary and your data remains intact.

Connections to a MongoDB replica are made by supplying the driver or binary the replica set name and a seed list for the hosts in the replica set. This configuration allows drivers and applications the ability to change which host they are connecting to if one of the hosts becomes unavailable. Your deployment has a [connection string](/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) in the [DNS Seedlist Connection format](https://docs.mongodb.com/manual/reference/connection-string/#dns-seedlist-connection-format), so your applications can take advantage of this feature.
 
## Application-level High-Availability

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-mongodb}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruptions are not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## SLA

{{site.data.keyword.databases-for-mongodb}} deployments conform to the {{site.data.keyword.cloud_notm}} [SLA terms](/docs/overview?topic=overview-slas).


