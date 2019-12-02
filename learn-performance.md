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

# Performance
{: #performance}

{{site.data.keyword.databases-for-mongodb_full}} deployments can be [scaled to your usage](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-resources-scaling), but they do not auto-scale. There are a few factors to consider if you are concerned about the performance of your deployment.

## Disk

If you are concerned about how much space MongoDB is using to store your data, you can run some native MongoDB [data storage diagnostics](https://docs.mongodb.com/manual/faq/storage/#data-storage-diagnostics) to find the sizes of things like databases, collections, and indexes.

### Disk IOPS

The number of Input-Output Operations per second (IOPS) is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-mongodb}} deployments are provisioned on [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance). Hitting IOPS limits can cause your databases to respond slowly or appear unresponsive. Things like unoptimized queries, [index building](https://docs.mongodb.com/manual/core/index-creation/), and creating new indexes can cause spikes in IOPS, but it's also possible that normal work loads for your applications can exceed the available IOPS for your deployment. You can increase the number IOPS available to your deployment by increasing disk space.

More information on disk writes in MongoDB is in the [MongoDB documentation](https://docs.mongodb.com/manual/faq/storage/#how-frequently-does-wiredtiger-write-to-disk)

## WiredTiger Cache

{{site.data.keyword.databases-for-mongodb}} uses the [WiredTiger storage engine, which uses both the file system memory cache and an internal memory cache](https://docs.mongodb.com/manual/core/wiredtiger/#memory-use). MongoDB is most performant when it serves your data from its internal cache, a little less performant when the data is in the file system cache, and least performant when it has to grab your data from disk.

The default size of the internal cache is `50% of (total RAM - 1 GB)` or `256 MB`, whichever is larger. For example, the minimum memory size of a {{site.data.keyword.databases-for-mongodb}} deployment is 1024 MB per data member, so the internal cache is 256 MB (because `1024 MB - 1 GB = 0`).

The internal/file system cache ratio is not user-configurable on your deployment, but you can scale the total amount of memory to adjust the internal cache to make your database more performant. For example, if you scale the memory to 2048 MB per member the internal cache size becomes 1024 MB. `2048 MB - 1 GB = 1024 MB.`

More information about the WiredTiger cache is in the [MongoDB documentation](https://docs.mongodb.com/manual/faq/storage/#to-what-size-should-i-set-the-wiredtiger-internal-cache
).

## Query Performance

The MongoDB documentation has multiple resources on query performance, including a how-to on [analyzing query performance](https://docs.mongodb.com/manual/tutorial/analyze-query-plan/). Once you have a general idea on how your queries perform, they also have tips on [optimizing your queries](https://docs.mongodb.com/manual/core/query-optimization/).

As a more advanced topic, you can learn how MongoDB [manages query plans](https://docs.mongodb.com/manual/core/query-plans/).

## Monitoring your deployment

{{site.data.keyword.databases-for-mongodb}} deployments offer an integration with the [{{site.data.keyword.cloud_notm}} Monitoring service](/docs/services/databases-for-mongodb?topic=cloud-databases-monitoring) for basic monitoring of memory and disk usage on your deployment. You can also take advantage of some of the native MongoDB monitoring functions.

Many of the MongoDB utilities and commands need the [Cluster Monitor](https://docs.mongodb.com/manual/reference/built-in-roles/#clusterMonitor) role for access. It is not part of the `admin` default role set. You can [grant the Cluster Monitor role](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-user-management#the-admin-user) to the `admin` user on your deployment.
{: .tip}

For example, you can use both [`mongotop`](https://docs.mongodb.com/manual/reference/program/mongotop/#bin.mongotop) and [`mongostat`](https://docs.mongodb.com/manual/reference/program/mongostat/#bin.mongostat). 

```
mongotop 30 --username admin --password $PASSWORD --ssl --sslCAFile $CERTFILE --authenticationDatabase admin --host host1.databases.appdomain.cloud:31712, host2.databases.appdomain.cloud:31712

mongostat -n 20 1 --username admin --password $PASSWORD --ssl --sslCAFile $CERTFILE --authenticationDatabase admin --host host1.databases.appdomain.cloud:31712,host2.databases.appdomain.cloud:31712 --json
```

You can also run any of the [documented commands](https://docs.mongodb.com/manual/administration/monitoring/#commands) that report on the status of your MongoDB database.
