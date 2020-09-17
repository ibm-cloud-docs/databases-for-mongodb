---

Copyright:
  years: 2019, 2020
lastupdated: "2020-01-09"

keywords: mongodb, databases, monitoring, scaling, autoscaling, resources, WiredTiger

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Performance
{: #performance}

{{site.data.keyword.databases-for-mongodb_full}} deployments can be both manually [scaled to your usage](/docs/databases-for-mongodb?topic=databases-for-mongodb-resources-scaling), or configured to [autoscale](/docs/databases-for-mongodb?topic=databases-for-mongodb-autoscaling) under certain resource conditions. There are a few factors to consider if you are tuning the performance of your deployment.

## Monitoring your deployment

{{site.data.keyword.databases-for-mongodb}} deployments offer an integration with the [Sysdig Monitoring service](/docs/databases-for-mongodb?topic=databases-for-mongodb-sysdig-monitoring) for basic monitoring of resource usage on your deployment. Many of the available metrics, like disk usage and IOPS, are presented to help you configure [autoscaling](/docs/databases-for-mongodb?topic=databases-for-mongodb-autoscaling) on your deployment. Observing trends in your usage and configuring the autoscaling to respond to them can help alleviate performance problems before your databases become unstable due to resource exhaustion.

## Disk Usage

If you are concerned about how much space MongoDB is using to store your data, you can run some native MongoDB [data storage diagnostics](https://docs.mongodb.com/manual/faq/storage/#data-storage-diagnostics) to find the sizes of things like databases, collections, and indexes. If the approximate size of your data set is known and fixed, you can manually scale your disk to accommodate your data. If your data set grows at predictable rate over time, you can configure autoscaling to increase disk size when your disk usage hits a certain threshold.

## Disk I/O

The number of Input-Output Operations per second (IOPS) on {{site.data.keyword.databases-for-mongodb}} deployments is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-mongodb}} deployments are [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/BlockStorage?topic=BlockStorage-About#provendurance). Hitting I/O utilization limits can cause your databases to respond slowly or appear unresponsive. Things like unoptimized queries, [index building](https://docs.mongodb.com/manual/core/index-creation/), and creating new indexes can cause spikes in IOPS, but it's also possible that normal work loads for your applications can exceed the available IOPS for your deployment. 

You can increase the number IOPS available to your deployment by increasing disk space. You can also configure autoscaling to increase disk size automatically if your deployment's I/O utilization hits a certain saturation point for an extended period of time.

More information on disk writes in MongoDB is in the [MongoDB documentation](https://docs.mongodb.com/manual/faq/storage/#how-frequently-does-wiredtiger-write-to-disk)

## WiredTiger Cache and Memory

{{site.data.keyword.databases-for-mongodb}} uses the [WiredTiger storage engine, which uses both the file system memory cache and an internal memory cache](https://docs.mongodb.com/manual/core/wiredtiger/#memory-use). MongoDB is most performant when it serves your data from its internal cache, a little less performant when the data is in the file system cache, and least performant when it has to grab your data from disk.

The default size of the internal cache is `50% of (total RAM - 1 GB)` or `256 MB`, whichever is larger. For example, the minimum memory size of a {{site.data.keyword.databases-for-mongodb}} deployment is 1024 MB per data member, so the internal cache is 256 MB (because `1024 MB - 1 GB = 0`).

The internal/file system cache ratio is not user-configurable on your deployment, but you can scale the total amount of memory to adjust the internal cache to make your database more performant. For example, if you scale the memory to 2048 MB per member the internal cache size becomes 1024 MB. `2048 MB - 1 GB = 1024 MB.`

Another way to use autoscaling is to set memory to scale when disk I/O utilization hits a certain threshold. Increasing memory decreases the amount that MongoDB reads or writes to disk, so additional memory might alleviate pressure on disk I/O by supporting more caching.

More information about the WiredTiger cache is in the [MongoDB documentation](https://docs.mongodb.com/manual/faq/storage/#to-what-size-should-i-set-the-wiredtiger-internal-cache).

## Query Performance

The MongoDB documentation has multiple resources on query performance, including a how-to on [analyzing query performance](https://docs.mongodb.com/manual/tutorial/analyze-query-plan/). Once you have a general idea on how your queries perform, they also have tips on [optimizing your queries](https://docs.mongodb.com/manual/core/query-optimization/).

As a more advanced topic, you can learn how MongoDB [manages query plans](https://docs.mongodb.com/manual/core/query-plans/).

## Other MongoDB Monitoring Tools

You can also take advantage of some of the native MongoDB monitoring functions. For example, you can use both [`mongotop`](https://docs.mongodb.com/manual/reference/program/mongotop/#bin.mongotop) and [`mongostat`](https://docs.mongodb.com/manual/reference/program/mongostat/#bin.mongostat). 

```
mongotop 30 --username admin --password $PASSWORD --ssl --sslCAFile $CERTFILE --authenticationDatabase admin --host host1.databases.appdomain.cloud:31712, host2.databases.appdomain.cloud:31712

mongostat -n 20 1 --username admin --password $PASSWORD --ssl --sslCAFile $CERTFILE --authenticationDatabase admin --host host1.databases.appdomain.cloud:31712,host2.databases.appdomain.cloud:31712 --json
```

You can also run any of the [documented commands](https://docs.mongodb.com/manual/administration/monitoring/#commands) that report on the status of your MongoDB database.

Many of the MongoDB utilities and commands need the [Cluster Monitor](https://docs.mongodb.com/manual/reference/built-in-roles/#clusterMonitor) role for in order to execute them. It is not part of the `admin` default role set. You can [grant the Cluster Monitor role](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management#the-admin-user) to the `admin` user on your deployment.
{: .tip}
