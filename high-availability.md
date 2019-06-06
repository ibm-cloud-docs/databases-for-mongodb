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

{{site.data.keyword.databases-for-mongodb}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. A deployment consists of two data nodes, one a primary node and the other as a secondary node. They are configured as a replication set without sharding, so both contain a copy of your data. An arbiter node is present to handle failover. If the primary node fails or becomes unavailable, the arbiter helps break ties when promoting a secondary to be the new primary. The failed node can then rejoin as secondary and your data remains intact.

Connections to a MongoDB replica are made by supplying the driver or binary the replica set name and a seed list for the hosts in the replica set. This configuration allows drivers and applications the ability to change which host they are connecting to if one of the hosts become unavailable. Your deployment has a [connection string](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) in the [DNS Seedlist Connection format](https://docs.mongodb.com/manual/reference/connection-string/#dns-seedlist-connection-format), so your applications can take advantage of this feature.
 
## Application-level High-Availability

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-mongodb}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruptions are not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## Performance

{{site.data.keyword.databases-for-mongodb}} deployments can be [scaled to your usage](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-resources-scaling), but they do not auto-scale. There are a few factors to consider if you are concerned about the performance of your deployment.

### Disk IOPS

The number of Input-Output Operations per second (IOPS) is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-mongodb}} deployments are provisioned on [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance). Hitting IOPS limits can cause your databases to respond slowly or appear unresponsive. Things like unoptimized queries, [index building](https://docs.mongodb.com/manual/core/index-creation/), and creating new indexes can cause spikes in IOPS, but it's also possible that normal work loads for your applications can exceed the available IOPS for your deployment. You can increase the number IOPS available to your deployment by increasing disk space.

### WiredTiger Memory Cache

{{site.data.keyword.databases-for-mongodb}} uses the [WiredTiger storage engine, which uses both the filesystem memory cache and an internal memory cache](https://docs.mongodb.com/manual/core/wiredtiger/#memory-use). MongoDB is most performant when it serves your data from its internal cache, a little less performant when the data is in the filesystem cache, and least performant when it has to grab your data from disk.

The default size of the internal cache is `50% of (total RAM - 1 GB)` or `256 MB`, whichever is larger.

For example, the minimum memory size of a {{site.data.keyword.databases-for-mongodb}} deployment is 1024 MB per data member, so the internal cache is 256 MB (because `1024 MB - 1 GB = 0`).

The remaining allocated memory is used for all other host processes and the filesystem cache. The internal/filesystem cache ratio is not user-configurable on your deployment, but you can scale the total amount of memory to adjust the internal cache to make your database more performant.

For example, if you scale the memory to 2048 MB per member the internal cache size becomes 1024 MB. `2048 MB - 1 GB = 1024 MB.`

### Monitoring your deployment

You can use the [monitoring integration](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-monitoring) to estimate typical resource usage, and scale your deployment accordingly.

If you are planning on running operations that might put a spike in the usual RAM usage, increase the size of your data, or an increase in IOPS, you can scale your deployment's resources to avoid hitting limits that affect deployment operations.

## SLA

{{site.data.keyword.databases-for-mongodb}} deployments conform to the {{site.data.keyword.cloud_notm}} [SLA terms](/docs/overview?topic=overview-SLAs#SLAs).


