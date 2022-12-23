---

copyright:
  years: 2019, 2022
lastupdated: "2022-12-23"

keywords: mongodb, databases, monitoring, scaling, autoscaling, resources, WiredTiger

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Performance
{: #performance}

{{site.data.keyword.databases-for-mongodb_full}} deployments can be both manually [scaled to your usage](/docs/databases-for-mongodb?topic=databases-for-mongodb-resources-scaling), or configured to [autoscale](/docs/databases-for-mongodb?topic=databases-for-mongodb-autoscaling) under certain resource conditions. If you are tuning the performance of your deployment, consider a few factors first.

## Monitoring your deployment
{: #monitor-deployment}

{{site.data.keyword.databases-for-mongodb}} deployments offer an integration with the [{{site.data.keyword.monitoringfull}} service](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring) for basic monitoring of resource usage on your deployment. Many of the available metrics, like disk usage and IOPS, are presented to help you configure [autoscaling](/docs/databases-for-mongodb?topic=databases-for-mongodb-autoscaling) on your deployment. Observing trends in your usage and configuring the autoscaling to respond to them can help alleviate performance problems before your databases become unstable due to resource exhaustion.

## Disk Usage
{: #disk-usage}

If you are concerned about how much space MongoDB is using to store your data, you can run some native MongoDB [data storage diagnostics](https://docs.mongodb.com/manual/faq/storage/#data-storage-diagnostics){: .external} to find the sizes of things like databases, collections, and indexes. If the approximate size of your data set is known and fixed, you can manually scale your disk to accommodate your data. If your data set grows at predictable rate over time, you can configure autoscaling to increase disk size when your disk usage hits a certain threshold.

## Disk I/O
{: #disk-iops}

The number of Input-Output Operations per second (IOPS) on {{site.data.keyword.databases-for-mongodb}} deployments is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-mongodb}} deployments are [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/BlockStorage?topic=BlockStorage-orderingthroughConsole#orderingthroughConsoleEndurance). Hitting I/O utilization limits can cause your databases to respond slowly or appear unresponsive. Things like unoptimized queries, [index building](https://docs.mongodb.com/manual/core/index-creation/){: .external}, and creating new indexes can cause spikes in IOPS, but it's also possible that normal work loads for your applications can exceed the available IOPS for your deployment. 

You can increase the number IOPS available to your deployment by increasing disk space. You can also configure autoscaling to increase disk size automatically if your deployment's I/O utilization hits a certain saturation point for an extended period of time.

For more information, see the [MongoDB documentation](https://docs.mongodb.com/manual/faq/storage/#how-frequently-does-wiredtiger-write-to-disk){: .external}.

## WiredTiger Cache and Memory
{: #wiredtiger-cache-memory}

{{site.data.keyword.databases-for-mongodb}} uses the [WiredTiger storage engine, which uses both the file system memory cache and an internal memory cache](https://docs.mongodb.com/manual/core/wiredtiger/#memory-use){: .external}. MongoDB is most performant when it serves your data from its internal cache, a little less performant when the data is in the file system cache, and least performant when it has to grab your data from disk.

The default size of the internal cache is `50% of (total RAM - 1 GB)` or `256 MB`, whichever is larger. For example, the minimum memory size of a {{site.data.keyword.databases-for-mongodb}} deployment is 1024 MB per data member, so the internal cache is 512 MB (because `(3072 MB / 2) - 1 GB = 512`.

The internal/file system cache ratio is not user-configurable on your deployment, but you can scale the total amount of memory to adjust the internal cache to make your database more performant. For example, if you scale the memory to 4096 MB per member the internal cache size becomes 5120 MB. `(12288 MB / 2) - 1 GB = 5120 MB.`

Another way to use autoscaling is to set memory to scale when disk I/O utilization hits a certain threshold. Increasing memory decreases the amount that MongoDB reads or writes to disk, so additional memory might alleviate pressure on disk I/O by supporting more caching.

More information about the WiredTiger cache is in the [MongoDB documentation](https://docs.mongodb.com/manual/faq/storage/#to-what-size-should-i-set-the-wiredtiger-internal-cache){: .external}.

## Query Performance
{: #query-performance}

The MongoDB documentation has multiple resources on query performance, including a how-to on [analyzing query performance](https://docs.mongodb.com/manual/tutorial/analyze-query-plan/){: .external}. Once you have a general idea on how your queries perform, they also have tips on [optimizing your queries](https://docs.mongodb.com/manual/core/query-optimization/){: .external}.

As a more advanced topic, you can learn how MongoDB [manages query plans](https://docs.mongodb.com/manual/core/query-plans/){: .external}.

## Other MongoDB Monitoring Tools
{: #other-monitor}

You can also take advantage of some of the native MongoDB monitoring functions. For example, you can use both [`mongotop`](https://docs.mongodb.com/manual/reference/program/mongotop/#bin.mongotop){: .external} and [`mongostat`](https://docs.mongodb.com/manual/reference/program/mongostat/#bin.mongostat){: .external}. 

```sh
mongotop 30 --username admin --password $PASSWORD --ssl --sslCAFile $CERTFILE --authenticationDatabase admin --host host1.databases.appdomain.cloud:31712, host2.databases.appdomain.cloud:31712

mongostat -n 20 1 --username admin --password $PASSWORD --ssl --sslCAFile $CERTFILE --authenticationDatabase admin --host host1.databases.appdomain.cloud:31712,host2.databases.appdomain.cloud:31712 --json
```

Run any of the [documented commands](https://docs.mongodb.com/manual/administration/monitoring/#commands){: .external} that report on the status of your MongoDB database.

Many of the MongoDB utilities and commands need the [Cluster Monitor](https://docs.mongodb.com/manual/reference/built-in-roles/#clusterMonitor){: .external} role to execute. It is not part of the `admin` default role set. [Grant the Cluster Monitor role](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management#the-admin-user) to the `admin` user on your deployment.
{: .tip}

For more information, see the [Best Practices blog post](https://www.ibm.com/cloud/blog/best-practices-for-mongodb-on-the-ibm-cloud).
