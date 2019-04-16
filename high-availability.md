---

Copyright:
  years: 2019
lastupdated: "2019-02-19"

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability and Performance
{: #high-availability}

{{site.data.keyword.databases-for-mongodb_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-mongodb}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. A deployment consists of two data nodes, one a primary node and the other as a secondary node. They are configured as a replication set without sharding, so both contain a copy of your data. An arbiter node is present for failover, and to break ties when electing a new primary. Should the primary node fail or become unavailable, the arbiter helps promote the secondary to be the new primary. The failed node can then rejoin as secondary and your data remains intact.

Connections to a MongoDB replica are made by supplying the driver or binary the replica set name and a seed list for the hosts in the replica set. This allows drivers and applications tha ability to change which host they are connecting to should one of the hosts become unavailable. Your deployment has a [connection string](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) in the [DNS Seedlist Connection format](https://docs.mongodb.com/manual/reference/connection-string/#dns-seedlist-connection-format), so your applications can take advantage of this feature.
 
## Application-level High-Availability

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-mongodb}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption is not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## Performance

{{site.data.keyword.databases-for-mongodb}} deployments can be [scaled to your usage](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-resources-scaling), but they do not auto-scale. There are a few factors to consider if you are concerned about the performance of your deployment.

### Disk IOPS

The number of Input-Output Operations per second (IOPS) is limited by the type of storage volume being used. Storage volumes for {{site.data.keyword.databases-for-mongodb}} deployments are provisioned on [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance). Hitting IOPS limits can cause your databases to respond slowly or appear unresponsive. Things like un-optimized queries, [index building](https://docs.mongodb.com/manual/core/index-creation/), and creating new indexes can cause spikes in IOPS, but it's also possible that normal work loads for your applications can exceed the available IOPS for your deployment. If you are exceeding the IOPS limit, you can increase the number IOPS available to your deployment by increasing disk space.

### Monitoring 

You can use the [monitoring integration](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-monitoring), to estimate typical resource usage, and scale your deployment accordingly.

If you are planning on running operations that might put a spike in the usual RAM usage, increase the size of your data, or an increase in IOPS, you can manually scale your service's resources up to avoid hitting limits that can affect deployment operations.

## SLA

{{site.data.keyword.databases-for-mongodb}} deployments conform to the {{site.data.keyword.cloud_notm}} [SLA terms](/docs/overview?topic=overview-SLAs#SLAs).


