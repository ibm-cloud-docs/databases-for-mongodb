---

copyright:
  years: 2026
lastupdated: "2026-03-30"

keywords: mongodb, databases, monitoring, scaling, autoscaling, resources, troubleshooting

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}


# Troubleshooting performance for {{site.data.keyword.databases-for-mongodb}}
{: #troubleshooting-performance}

Use this guide to help you identify and resolve performance issues in your {{site.data.keyword.databases-for-mongodb}} deployment running on {{site.data.keyword.cloud_notm}} and powered by MongoDB.

If your applications are experiencing slow responses, timeouts, or inconsistent database performance, consider the following steps and information.


## Symptoms of performance issues
{: #troubleshooting-symptoms}

You might observe some of the following symptoms that indicate problems with performance:

* Increased application latency
* Slow query log entries
* High CPU or memory utilization
* Increased disk latency
* Replication lag
* Connection timeouts

Complete the following steps to determine the cause of the issues:

### Step 1: Check resource utilization
{: #troubleshooting-step1}

1. Log in to the {{site.data.keyword.cloud_notm}} console and navigate to your MongoDB deployment.

2. Review the **Monitoring** section for:

    * CPU utilization
    * Memory usage
    * Disk IOPS and latency
    * Active connections

#### What to look for:
{: #troubleshooting-step1-symptoms}

* CPU consistently above 75%
* Memory consistently above 80%
* Disk latency increasing over time
* Connections approaching plan limits

#### Recommended actions:
{: #troubleshooting-step1-actions}

* Increase storage or IOPS if disk latency is high.
* Review workload spikes in your application.

If resource usage remains elevated for sustained periods, scaling is recommended.




### Step 2: Identify slow queries
{: #troubleshooting-step2}

Slow queries are one of the most common causes of degraded performance.

1. Enable profiling:

    ```js
    db.setProfilingLevel(1, { slowms: 100 })
    ```
    {: codeblock}

2. Review recent slow operations:

    ```js
    db.system.profile.find().sort({ ts: -1 }).limit(20)
    ```
   {: codeblock}

3. Analyze query execution:

    ```js
    db.collection.find({ ... }).explain("executionStats")
    ```
    {: codeblock}

#### What to look for:
{: #troubleshooting-step2-symptoms}

* `COLLSCAN` (collection scan instead of index usage)
* High `totalDocsExamined` compared to `nReturned`
* Blocking sort stages (Liam: extra info needed)

#### Recommended actions:
{: #troubleshooting-step2-actions}

* Create appropriate indexes.
* Use compound indexes for multi-field queries.
* Ensure aggregation pipelines begin with `$match`.
* Avoid large `skip()` pagination.



### Step 3: Review connection usage
{: #troubleshooting-step3}

High or poorly managed connections can impact performance.

#### Check connection statistics:
{: #troubleshooting-step3-statistics}

```js
db.serverStatus().connections
```
{: codeblock}

#### Recommended actions:
{: #troubleshooting-step3-actions}

* Use connection pooling in your application.
* Avoid opening a new connection for each request.
* Close unused cursors.

Connection limits are determined by your deployment plan.

### Step 4: Check replication health
{: #troubleshooting-step4}

Replication lag can affect read performance and data freshness.

#### Check replication status:
{: #troubleshooting-step4-status}

```js
rs.printSecondaryReplicationInfo()
```
{: codeblock}

#### Common causes of lag:
{: #troubleshooting-step4-lag}

* High write throughput
* Disk bottlenecks
* Network latency

#### Recommended actions:
{: #troubleshooting-step4-actions}

* Scale storage performance.
* Review write concern settings.
* Scale to a higher plan if lag is persistent.

### Step 5: Sharded cluster considerations (if applicable)
{: #troubleshooting-step5}

You might need sharding when:
* Working set > RAM
* Single-node IOPS maxed out even after scaling
* Horizontal write scaling required
* Collections exceed 1–2 TB

For more information, see [performance tuning](https://www.mongodb.com/docs/manual/administration/performance-tuning/) and [sharding](https://www.mongodb.com/docs/manual/sharding/).

If your deployment uses sharding, run:

```js
sh.status()
```
{: codeblock}

#### Check for:
{: #troubleshooting-step5-check}

* Uneven chunk distribution
* Jumbo chunks
* Traffic concentrated on a single shard

#### Recommended actions:
{: #troubleshooting-step5-actions}

* Review shard key selection.
* Avoid monotonically increasing shard keys.
* Consider hashed shard keys.

Improper shard key selection can significantly affect performance at scale.

### Step 6: After large data deletions
{: #troubleshooting-step6}

Deleting a significant percentage of data does not immediately reduce disk usage at the operating system level.

#### Possible impacts:
{: #troubleshooting-step6-impacts}

* Internal fragmentation
* High disk utilization
* Reduced performance

#### Recommended actions:
{: #troubleshooting-step6-actions}

* Plan compaction operations carefully.
* Consider dump and restore for severe fragmentation.
* Keep disk utilization below 80–85%.

Schedule maintenance activities appropriately.


### Step 7: Check for lock contention
{: #troubleshooting-step7}

Lock contention can severely impact concurrent operations and overall throughput.

* Check global lock statistics:

    ```js
    db.serverStatus().locks
    ```
   {: codeblock}

* Check current operations for locks:

    ```js
    db.currentOp({
      $or: [
        { waitingForLock: true },
        { "locks.Global": "w" }
      ]
    })
    ```
    {: codeblock}

* Analyze lock wait time:

  ```js
  db.serverStatus().globalLock
  ```
  {: codeblock}

#### What to look for:
{: #troubleshooting-step7-symptoms}

* High `currentQueue` values (readers or writers).
* Operations with `waitingForLock: true`.
* Long-running operations holding locks.
* Index builds that block operations.

#### Common causes:
{: #troubleshooting-step7-causes}

* Long-running queries without proper indexes.
* Large write operations.
* Index builds on large collections.
* Administrative commands (compact, repairDatabase).

#### Recommended actions:
{: #troubleshooting-step7-actions}

* Kill long-running operations if necessary:
  ```js
  db.killOp(opid)
  ```
  {: codeblock}
* Build indexes in the background:
  ```js
  db.collection.createIndex({ field: 1 }, { background: true })
  ```
  {: codeblock}
* Break large operations into smaller batches.
* Schedule maintenance operations during low-traffic periods.
* Use read concern and write concern appropriately.


### Step 8: Analyze workload patterns
{: #troubleshooting-step8}

Understanding your workload patterns helps identify optimization opportunities.

* Check operation counters:


    ```js
    db.serverStatus().opcounters
    ```
    {: codeblock}

* Analyze operations over time:

    ```js
    db.serverStatus().opcountersRepl
    ```
    {: codeblock}

* Identify hot collections:

    ```js
    db.adminCommand({ top: 1 })
    ```
    {: codeblock}

* Check the read ratio compared to the write ratio:

    ```js
    var stats = db.serverStatus().opcounters;
    print("Read ratio: " + (stats.query + stats.getmore) / (stats.query + stats.getmore + stats.insert + stats.update + stats.delete));
    ```
    {: codeblock}

#### What to look for:
{: #troubleshooting-step8-symptoms}

* Disproportionate operations on specific collections
* High read-to-write or write-to-read ratios
* Sudden spikes in operation counts
* Time-based patterns (peak hours)

#### Recommended actions:
{: #troubleshooting-step8-actions}

* Optimize frequently accessed collections first.
* Consider read replicas for read-heavy workloads.
* Use appropriate read preferences.
* Implement caching for frequently read data.
* Review indexing strategy for hot collections.
* Consider sharding for write-heavy collections.


### Step 9: Investigate memory pressure and cache efficiency
{: #troubleshooting-step9}

MongoDB's WiredTiger storage engine relies heavily on cache efficiency.

* Check WiredTiger cache statistics:

    ```js
    db.serverStatus().wiredTiger.cache
    ```
    {: codeblock}

* Review key metrics:

    ```js
    var cache = db.serverStatus().wiredTiger.cache;
    print("Cache size: " + cache["bytes currently in the cache"]);
    print("Max cache size: " + cache["maximum bytes configured"]);
    print("Pages read into cache: " + cache["pages read into cache"]);
    print("Pages written from cache: " + cache["pages written from cache"]);
    print("Cache hit ratio: " + (1 - cache["pages read into cache"] / (cache["pages read into cache"] + cache["pages requested from the cache"])));
    ```
    {: codeblock}

* Check for eviction pressure:

    ```js
    db.serverStatus().wiredTiger.cache["pages evicted by application threads"]
    ```
    {: codeblock}

#### What to look for:
{: #troubleshooting-step9-symptoms}

* Cache hit ratio below 95%
* High eviction rates
* Cache size consistently at maximum
* Application threads performing evictions

#### Estimate working set size:
{: #troubleshooting-step9-size}

```js
db.serverStatus().wiredTiger.cache["tracked dirty bytes in the cache"]
```
{: codeblock}

#### Recommended actions:
{: #troubleshooting-step9-actions}

* Scale to a plan with more memory if the cache is consistently full.
* Review and optimize indexes (remove unused indexes).
* Limit result set sizes in queries.
* Use projections to reduce document size.
* Consider archiving old data.
* Monitor working set size trends.

#### Memory allocation best practices
{: #troubleshooting-step9-best}

* The WiredTiger cache should be 50% of available RAM (default).
* Leave sufficient memory for other processes.
* Monitor swap usage, which should be minimal.


### Step 10: Review write concern and read preference settings
{: #troubleshooting-step10}

Write concern and read preference settings significantly impact performance and consistency.

* Check current write concern:

    ```js
    db.getWriteConcern()
    ```
    {: codeblock}


* Check replica set configuration:

    ```js
    rs.conf()
    ```
    {: codeblock}

* Write concern options:

    | Write concern | Durability | Performance | Use case |
    |---------------|------------|-------------|----------|
    | `w: 1` | Low | High | Non-critical data, high throughput |
    | `w: "majority"` | High | Medium | Default, balanced approach |
    | `w: <number>` | Medium-High | Medium-Low | Specific replica count |
    | `j: true` | Highest | Lowest | Critical data requiring journal sync |
    {: caption="Write concern options" caption-side="top"}


* Read preference options:

    | Read preference | Consistency | Performance | Use case |
    |-----------------|-------------|-------------|----------|
    | `primary` | Highest | Medium | Default, strong consistency |
    | `primaryPreferred` | High | Medium-High | Fallback to secondary |
    | `secondary` | Eventual | High | Analytics, reporting |
    | `secondaryPreferred` | Eventual | High | Read scaling |
    | `nearest` | Eventual | Highest | Lowest latency |
    {: caption="Read preference options" caption-side="top"}


* Check read preference in your application:

    ```js
    // Example in Node.js driver
    db.collection('users').find({}).readPreference('secondary')
    ```
    {: codeblock}

#### What to look for:
{: #troubleshooting-step10-symptoms}

* Overly strict write concerns for non-critical data
* Using `primary` read preference when eventual consistency is acceptable
* Not leveraging secondaries for read-heavy workloads

#### Recommended actions:
{: #troubleshooting-step10-actions}

* Use `w: 1` for high-throughput, non-critical writes.
* Use `w: "majority"` for important data (default).
* Use `secondary` or `secondaryPreferred` for analytics queries.
* Consider `nearest` for geographically distributed applications.
* Balance consistency requirements with performance needs.
* Test different configurations under load.


### Step 11: Monitor backup and maintenance impact
{: #troubleshooting-step11}

Backup operations and maintenance tasks can temporarily affect performance.

#### {{site.data.keyword.cloud_notm}} backup schedule
{: #troubleshooting-step11-schedule}

{{site.data.keyword.databases-for-mongodb}} automatically does a backup. Check your backup schedule in the {{site.data.keyword.cloud_notm}} console under **Backups**.

#### Check for ongoing backup operations:
{: #troubleshooting-step11-backup}

```js
db.currentOp({
  $or: [
    { op: "command", "command.backup": { $exists: true } },
    { desc: /^conn/ }
  ]
})
```
{: codeblock}

#### What to look for:
{: #troubleshooting-step11-symptoms}

* Performance degradation during backup windows
* Increased disk I/O during backups
* Replication lag during backups


#### Recommended actions:
{: #troubleshooting-step11-actions}

* Monitor performance metrics during backup times.
* Consider scaling if backups consistently impact performance.
* Review backup retention policies.
* Plan for increased resource usage during restore operations.

#### Maintenance operation best practices
{: #troubleshooting-step11-best}

* Schedule index builds during low-traffic periods.
* Use background index builds when possible.
* Monitor replication lag during maintenance.
* Test maintenance operations in non-production first.
* Coordinate with {{site.data.keyword.cloud_notm}} maintenance windows.
