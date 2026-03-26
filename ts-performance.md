---

copyright:
  years: 2026
lastupdated: "2026-03-26"

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

* Scale your deployment to a higher plan.
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
* Blocking sort stages

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

For managed deployments, schedule maintenance activities appropriately.


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
* Leave sufficient memory for OS and other processes.
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

{{site.data.keyword.databases-for-mongodb}} automatically backs up. Check your backup schedule in the {{site.data.keyword.cloud_notm}} console under **Backups**.

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

#### Point-in-Time Recovery (PITR) considerations
{: #troubleshooting-step11-pitr}

* PITR maintains continuous backup capability
* Minimal performance impact under normal conditions
* Might increase disk I/O slightly

#### Recommended actions:
{: #troubleshooting-step11-actions}

* Schedule application maintenance during backup windows.
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



## {{site.data.keyword.cloud_notm}}-specific features and tools to help troubleshoot performance
{: #features-tools}

### Using IBM Cloud Monitoring (Sysdig)
{: #sysdig}

{{site.data.keyword.databases-for-mongodb}} integrates with IBM Cloud Monitoring powered by Sysdig for comprehensive observability.

#### Accessing monitoring dashboards
{: #dashboards}

1. Navigate to your MongoDB deployment in {{site.data.keyword.cloud_notm}} console.
2. Click **Monitoring** in the left navigation.
3. Click **Launch Monitoring** to open the Sysdig dashboard.

#### Key metrics to track:
{: #key-metrics}

* **Platform metrics:**
    * **CPU utilization** - target: < 75% sustained
    * **Memory utilization** - target: < 80% sustained
    * **Disk utilization** - target: < 80%
    * **Disk IOPS** - monitor for saturation
    * **Network throughput** - identify bandwidth constraints

* **MongoDB-specific metrics:**
    * **Operations per second** - track workload patterns
    * **Active connections** - monitor against plan limits
    * **Replication lag** - target: < 1 second
    * **Query execution time** - identify slow queries
    * **Cache hit ratio** - target: > 95%

#### Setting up alerts
{: #alert-setup}

Create alerts for critical thresholds:

```sh
Alert: High CPU Usage
Condition: CPU > 80% for 10 minutes
Action: Notify operations team
```
{: codeblock}

```sh
Alert: Replication Lag
Condition: Replication lag > 5 seconds
Action: Page on-call engineer
```
{: codeblock}

```sh
Alert: Disk Space
Condition: Disk usage > 85%
Action: Trigger scaling workflow
```
{: codeblock}

#### Creating custom dashboards
{: #custom-dashboard}

1. In Sysdig, click **Dashboards** > **Create Dashboard**.
2. Add panels for key metrics.
3. Use filters to focus on your MongoDB deployment.
4. Save and share with your team.

#### Example dashboard layout
{: #dashboard-layout}

* **Row 1**: CPU, memory, disk utilization
* **Row 2**: Operations per second, active connections
* **Row 3**: Replication lag, query performance
* **Row 4**: Cache statistics, lock contention

#### Historical analysis
{: #analysis}

* Use the time range selector for historical data
* Compare current metrics with baseline
* Identify trends and patterns
* Correlate events with performance changes

#### Recommended actions:
{: #actions}

* Set up alerts before issues occur.
* Review dashboards daily.
* Establish baseline metrics.
* Document normal patterns compared to abnormal patterns.
* Use metrics for capacity planning.


### IBM Cloud Activity Tracker integration
{: #activity-tracker-integration}

IBM Cloud Activity Tracker helps you track configuration changes and administrative actions that can impact performance.

#### Accessing Activity Tracker
{: #activity-tracker-access}


1. Navigate to **Observability** > **Activity Tracker** in the {{site.data.keyword.cloud_notm}} console.
2. Select your region.
3. Filter events by your MongoDB instance.

#### Key events to monitor:
{: #key-events}


* **Configuration changes:**
    * Scaling operations (CPU, memory, disk)
    * Backup configuration changes
    * Network configuration updates
    * User access modifications

* **Performance-impacting events**
    * Database restarts
    * Failover events
    * Maintenance operations
    * Index creation and deletion

#### Correlating events with performance issues
{: #events-performance}

1. Note the timestamp of performance degradation.
2. Search Activity Tracker for events around that time.
3. Look for configuration changes or administrative actions.
4. Correlate with monitoring metrics.

#### Example event analysis
{: #example-analysis}


```sh
Event: Database scaled from 2GB to 4GB RAM
Time: 2024-01-15 14:30:00 UTC
Impact: Temporary connection disruption (30 seconds)
Result: Improved performance after scaling
```
{: codeblock}

#### Audit trail for compliance
{: #audit-compliance}

* Track who made changes and when
* Maintain compliance with security policies
* Review access patterns
* Identify unauthorized changes

#### Recommended actions:
{: #activity-tracker-actions}

* Review Activity Tracker logs regularly.
* Set up alerts for critical events.
* Document change management procedures.
* Correlate events with performance metrics.
* Use for post-incident analysis.


### {{site.data.keyword.cloud_notm}} scaling options
{: #scaling-options}

{{site.data.keyword.databases-for-mongodb}} offers flexible scaling options to match your performance needs.

#### Vertical scaling (compute and memory)
{: #vertical-scaling}

Scale CPU and memory resources to handle increased workload.

* **Using the {{site.data.keyword.cloud_notm}} console:**
    1. Navigate to your MongoDB deployment.
    2. Click **Resources** in the left navigation.
    3. Adjust **Memory** and **CPU** sliders.
    4. Review cost impact.
    5. Click **Scale**.

* **Using the {{site.data.keyword.cloud_notm}} CLI:**

    ```bash
    # Scale memory to 8GB and CPU to 4 cores
    ibmcloud cdb deployment-groups-set <deployment-id> member \
      --memory 8192 \
      --cpu-allocation 4
    ```
    {: codeblock}

#### Considerations
{: #scaling-considerations}

* Brief connection disruption during scaling.
* Plan for 5-10 minutes downtime.
* Scale proactively before saturation.
* Monitor metrics after scaling.

#### Horizontal scaling (replica set members)
{: #scaling-horizontal}

Add replica set members for read scaling and high availability.

* **Using the {{site.data.keyword.cloud_notm}} console:**
    1. Navigate to **Resources**.
    2. Adjust **Members** slider.
    3. Review configuration.
    4. Click **Scale**.

* **Using the {{site.data.keyword.cloud_notm}} CLI:**

    ```bash
    # Add a replica set member
    ibmcloud cdb deployment-groups-set <deployment-id> member \
      --members 4
    ```
    {: codeblock}

#### Benefits
{: #horizontal-benefits}

* Distribute read load across secondaries
* Improved fault tolerance
* Better geographic distribution
* No downtime for adding members

#### Storage scaling
{: #storage-scaling}

Increase disk space and IOPS for better performance.

* **Using the {{site.data.keyword.cloud_notm}} console:**

    1. Navigate to **Resources**.
    2. Adjust **Disk** slider.
    3. Review IOPS allocation.
    4. Click **Scale**.

* **Using the {{site.data.keyword.cloud_notm}} CLI:**

    ```bash
    # Scale disk to 100GB
    ibmcloud cdb deployment-groups-set <deployment-id> member \
      --disk-allocation 102400
    ```
    {: codeblock}

#### Important information:
{: #storage-scaling-notes}

* Storage can only be increased, not decreased
* IOPS scale with disk size
* No downtime for storage scaling
* Monitor disk usage trends

#### Scaling best practices
{: #storage-scaling-best}

| Scenario | Recommended action |
|----------|-------------------|
| High CPU (>80%) | Scale CPU cores |
| High memory (>80%) | Scale memory allocation |
| Disk latency | Increase disk size for more IOPS |
| Connection limits | Scale to higher tier |
| Read-heavy workload | Add replica members |
| Write-heavy workload | Scale CPU and memory |
{: caption="Scaling best practices" caption-side="top"}

#### Cost optimization
{: #storage-scaling-cost}

* Right size your deployment.
* Use monitoring to identify actual needs.
* Scale down during low-traffic periods (if supported).
* Consider reserved capacity for predictable workloads.

#### Automation
{: #storage-scaling-automation}

```bash
# Example: Auto-scale based on CPU threshold
if [ $(ibmcloud cdb deployment-metrics <deployment-id> --metric cpu) -gt 80 ]; then
  ibmcloud cdb deployment-groups-set <deployment-id> member --cpu-allocation 6
fi
```
{: codeblock}

### {{site.data.keyword.cloud_notm}} CLI and API for diagnostics
{: #cli-api-diagnostics}

Use {{site.data.keyword.cloud_notm}} CLI and API for automated diagnostics and monitoring.

#### Installing {{site.data.keyword.cloud_notm}} CLI
{: #install-cli}

```bash
# Install IBM Cloud CLI
curl -fsSL https://clis.cloud.ibm.com/install/linux | sh

# Install databases plugin
ibmcloud plugin install cloud-databases
```
{: codeblock}

#### Essential diagnostic commands
{: #diagnostic-commands}

* **Get deployment information:**


    ```bash
    # List all MongoDB deployments
    ibmcloud cdb deployments --type mongodb

    # Get specific deployment details
    ibmcloud cdb deployment <deployment-id>
    ```
    {: codeblock}

* **Check deployment status:**

    ```bash
    # Get deployment status
    ibmcloud cdb deployment-status <deployment-id>

    # Get connection strings
    ibmcloud cdb deployment-connections <deployment-id>
    ```
    {: codeblock}

* **Monitor metrics:**

    ```bash
    # Get CPU metrics
    ibmcloud cdb deployment-metrics <deployment-id> --metric cpu

    # Get memory metrics
    ibmcloud cdb deployment-metrics <deployment-id> --metric memory

    # Get disk metrics
    ibmcloud cdb deployment-metrics <deployment-id> --metric disk
    ```
    {: codeblock}

* **Scaling operations:**

    ```bash
    # Scale memory
    ibmcloud cdb deployment-groups-set <deployment-id> member \
      --memory 16384

    # Scale CPU
    ibmcloud cdb deployment-groups-set <deployment-id> member \
      --cpu-allocation 8

    # Scale disk
    ibmcloud cdb deployment-groups-set <deployment-id> member \
      --disk-allocation 204800
    ```
    {: codeblock}

* **Backup operations:**

    ```bash
    # List backups
    ibmcloud cdb backups <deployment-id>

    # Get backup information
    ibmcloud cdb backup <backup-id>
    ```
    {: codeblock}

#### Using the {{site.data.keyword.cloud_notm}} API
{: #cloud-api}

* **Authentication:**

    ```bash
    # Get IAM token
    export IAM_TOKEN=$(ibmcloud iam oauth-tokens --output json | jq -r '.iam_token')
    ```
    {: codeblock}

* **Get deployment metrics using the API:**

    ```bash
    # Get metrics
    curl -X GET \
      "https://api.{region}.databases.cloud.ibm.com/v5/deployments/{deployment-id}/metrics" \
      -H "Authorization: ${IAM_TOKEN}"
    ```
    {: codeblock}

* **Scale deployment using the API:**

    ```bash
    # Scale resources
    curl -X PATCH \
      "https://api.{region}.databases.cloud.ibm.com/v5/deployments/{deployment-id}/groups/member" \
      -H "Authorization: ${IAM_TOKEN}" \
      -H "Content-Type: application/json" \
      -d '{
        "memory": {
          "allocation_mb": 16384
        },
        "cpu": {
          "allocation_count": 8
        }
      }'
    ```
    {: codeblock}

* **Sample diagnostic script:**

    ```bash
    #!/bin/bash
    # MongoDB Performance Check Script

    DEPLOYMENT_ID="your-deployment-id"

    echo "=== MongoDB Performance Diagnostics ==="
    echo ""

    # Check CPU
    CPU=$(ibmcloud cdb deployment-metrics $DEPLOYMENT_ID --metric cpu --output json | jq -r '.metrics[0].value')
    echo "CPU Usage: ${CPU}%"
    if [ $(echo "$CPU > 80" | bc) -eq 1 ]; then
      echo "⚠️  WARNING: High CPU usage detected"
    fi

    # Check Memory
    MEMORY=$(ibmcloud cdb deployment-metrics $DEPLOYMENT_ID --metric memory --output json | jq -r '.metrics[0].value')
    echo "Memory Usage: ${MEMORY}%"
    if [ $(echo "$MEMORY > 80" | bc) -eq 1 ]; then
      echo "⚠️  WARNING: High memory usage detected"
    fi

    # Check Disk
    DISK=$(ibmcloud cdb deployment-metrics $DEPLOYMENT_ID --metric disk --output json | jq -r '.metrics[0].value')
    echo "Disk Usage: ${DISK}%"
    if [ $(echo "$DISK > 80" | bc) -eq 1 ]; then
      echo "⚠️  WARNING: High disk usage detected"
    fi

    # Check Status
    STATUS=$(ibmcloud cdb deployment-status $DEPLOYMENT_ID --output json | jq -r '.status')
    echo "Deployment Status: ${STATUS}"

    echo ""
    echo "=== Diagnostics Complete ==="
    ```

* **Automation recommendations**

    * Schedule regular health checks.
    * Integrate with monitoring systems.
    * Automate scaling based on thresholds.
    * Create alerts for critical metrics.
    * Log all operations for audit trail.


### {{site.data.keyword.cloud_notm}} network optimization
{: #network}

Network configuration significantly impacts MongoDB performance, especially for distributed applications.

#### Private endpoints compared to public endpoints
{: #endpoints}

#### Private endpoints (recommended)
{: #private-endpoints}

**Benefits:**
* Lower latency
* Enhanced security
* No internet egress charges
* Better performance for {{site.data.keyword.cloud_notm}} workloads

**Setup:**
1. Navigate to **Settings** > **Endpoints**.
2. Enable **Private endpoint**.
3. Update connection strings in applications.

**Connection string example:**

```sh
mongodb://user:pass@host.private.databases.appdomain.cloud:port/database?authSource=admin&replicaSet=replset
```
{: codeblock}

#### Public endpoints
{: #public-endpoints}

**Use cases:**
* External applications
* Development and testing
* Hybrid cloud scenarios

**Security considerations:**
* Use IP allowlisting.
* Enforce TLS/SSL.
* Rotate credentials regularly.

#### Service endpoints
{: #service-endpoints}

{{site.data.keyword.cloud_notm}} service endpoints provide optimized connectivity within {{site.data.keyword.cloud_notm}}.

#### Benefits
{: #service-endpoints-benefits}

* Reduced latency
* No public internet traversal
* Improved security posture
* Cost savings on bandwidth

#### Configuration
{: #service-endpoints-config}

```bash
# Enable service endpoint
ibmcloud cdb deployment-service-endpoint-enable <deployment-id>
```
{: codeblock}

#### Multi-zone deployment considerations
{: #multizone}

{{site.data.keyword.databases-for-mongodb}} can span multiple availability zones.

#### Performance implications
{: #multizone-implications}

* **Intra-zone latency**: < 1ms
* **Inter-zone latency**: 1-5ms
* **Cross-region latency**: 50-200ms

#### Best practices
{: #multizone-best}

* Deploy applications in the same region.
* Use read preferences to minimize latency.
* Consider `nearest` read preference for multi-zone apps.
* Monitor replication lag between zones.

#### Network latency troubleshooting
{: #latency}

* **Measure latency from application**

    ```bash
    # Test connection latency
    time mongo "mongodb://host:port/database" --eval "db.runCommand({ping: 1})"
    ```
    {: codeblock}

* **Check from {{site.data.keyword.cloud_notm}} shell**

    ```bash
    # Ping test (if ICMP allowed)
    ping -c 10 your-mongodb-host.databases.appdomain.cloud

    # TCP connection test
    nc -zv your-mongodb-host.databases.appdomain.cloud 27017
    ```
    {: codeblock}

#### MongoDB connection diagnostics
{: #measure-latency-connection}

```js
// Check network latency
db.runCommand({ ping: 1 })

// Check connection pool stats
db.serverStatus().connections
```
{: codeblock}

#### Geographic distribution
{: #geographic}

For globally distributed applications:

#### Strategies
{: #latency-strategies}

* **Single region**: Lowest latency, single point of failure
* **Multi-region with read replicas**: Read scaling, eventual consistency
* **Cross-region replication**: Disaster recovery, higher latency

#### Recommendations
{: #latency-recommendations}

* Place database close to primary user base.
* Use CDN for static content.
* Implement application-level caching.
* Consider data residency requirements.

#### Bandwidth optimization
{: #latency-bandwidth}

* Use projections to limit data transfer.
* Implement pagination for large result sets.
* Compress data at application level.
* Use bulk operations to reduce round trips.

#### Connection pooling best practices
{: #latency-pooling}

```javascript
// Node.js example
const client = new MongoClient(uri, {
  maxPoolSize: 50,
  minPoolSize: 10,
  maxIdleTimeMS: 30000,
  serverSelectionTimeoutMS: 5000,
  socketTimeoutMS: 45000
});
```
{: codeblock}


### {{site.data.keyword.cloud_notm}} Support integration
{: #support-integration}

Know when and how to engage {{site.data.keyword.cloud_notm}} Support for performance issues.

#### When to contact IBM Support
{: #contact-IBM}

Contact support if the following apply:

* Performance issues persist after following this guide.
* Replication lag continues despite optimization.
* Disk latency remains high without workload spikes.
* Suspected infrastructure-level issues.
* Unexpected behavior after scaling.
* Deployment health issues.
* Backup or restore problems.

#### Before contacting support
{: #before-contact}

Gather the following information:

* **Deployment details**

    * Deployment ID (CRN)
    * Region and availability zones
    * Current plan and resources
    * MongoDB version

* **Issue details**

    * Time window of the issue (with timezone)
    * Symptoms observed
    * Impact on applications
    * Recent changes (code, configuration, scaling)

* **Performance data**

    * Monitoring screenshots from Sysdig
    * Query examples causing issues
    * Output from diagnostic commands
    * Activity Tracker events during the issue window

* **MongoDB diagnostics**

    ```bash
    # Collect diagnostic data
    mongo "your-connection-string" --eval "
      printjson(db.serverStatus());
      printjson(db.currentOp());
      printjson(rs.status());
    " > mongodb-diagnostics.json
    ```
    {: codeblock}

#### Opening a support ticket
{: #open-ticket}

#### Using the {{site.data.keyword.cloud_notm}} console
{: #ticket-console}

1. Navigate to **Support** in the top menu.
2. Click **Create a case**.
3. Select **Databases for MongoDB**.
4. Choose severity level.
5. Provide detailed description.
6. Attach diagnostic files.

#### Using the {{site.data.keyword.cloud_notm}} CLI
{: #ticket-cli}

```bash
# Create support case
ibmcloud support case-create \
  --subject "MongoDB Performance Issue" \
  --description "Detailed description of issue" \
  --severity 2 \
  --offering databases-for-mongodb
```
{: codeblock}

#### Severity levels
{: #ticket-severity}

| Severity | Description | Response time |
|----------|-------------|---------------|
| 1 (Critical) | Production down, data loss | 1 hour |
| 2 (High) | Significant performance degradation | 2 hours |
| 3 (Medium) | Moderate impact, workaround available | 4 hours |
| 4 (Low) | General questions, feature requests | 8 hours |
{: caption="Severity levels" caption-side="top"}

#### Escalation procedures
{: #escalation}

If the issue is not resolved within the expected timeframe:

1. Update the support case with urgency.
2. Request escalation to a senior engineer.
3. Contact your IBM account team.
4. For critical issues, request management escalation.

#### SLA considerations
{: #sla}

* Review your service level agreement.
* Understand uptime guarantees.
* Know your support entitlements.
* Document all outages for SLA credits.

#### Support best practices
{: #support-best}

* Provide complete information upfront.
* Respond promptly to support requests.
* Test suggested solutions in non-production first.
* Document resolution for future reference.
* Provide feedback on support experience.

#### Self-service resources
{: #self-service}

Before opening a ticket, check the following:

* [{{site.data.keyword.cloud_notm}} Databases documentation](https://cloud.ibm.com/docs/databases-for-mongodb)
* [MongoDB documentation](https://docs.mongodb.com/)
* [{{site.data.keyword.cloud_notm}} Status page](https://cloud.ibm.com/status)
* Community forums and Stack Overflow

## Quick reference: diagnostic commands
{: #quick-reference-commands}

Essential MongoDB commands for performance troubleshooting.

| Command | Purpose | Key metrics | Normal values |
|---------|---------|-------------|---------------|
| `db.serverStatus()` | Overall server statistics | CPU, memory, connections | Varies by workload |
| `db.serverStatus().connections` | Connection statistics | current, available | < 80% of available |
| `db.serverStatus().opcounters` | Operation counters | insert, query, update, delete | Baseline dependent |
| `db.serverStatus().locks` | Lock statistics | Global lock time | < 10% of total time |
| `db.serverStatus().wiredTiger.cache` | Cache statistics | Cache hit ratio | > 95% |
| `db.currentOp()` | Current operations | Active queries, locks | Few long-running ops |
| `db.currentOp({ waitingForLock: true })` | Operations waiting for locks | Lock contention | Should be empty |
| `rs.status()` | Replica set status | Replication lag, member health | Lag < 1 second |
| `rs.printSecondaryReplicationInfo()` | Replication lag details | Lag per secondary | Lag < 1 second |
| `db.collection.stats()` | Collection statistics | Size, index size, document count | Monitor growth |
| `db.collection.find().explain("executionStats")` | Query execution plan | Execution time, docs examined | Use indexes |
| `db.system.profile.find()` | Slow query log | Slow operations | Review regularly |
| `sh.status()` | Sharding status (if applicable) | Chunk distribution | Even distribution |
| `db.adminCommand({ top: 1 })` | Collection usage statistics | Hot collections | Identify optimization targets |
| `db.printReplicationInfo()` | Oplog information | Oplog size, time range | Sufficient for recovery |
{: caption="Diagnostic commands" caption-side="top"}

### Quick diagnostic workflow
{: #workflow}

```js
// 1. Check overall health
db.serverStatus().ok  // Should return 1

// 2. Check connections
var conn = db.serverStatus().connections;
print("Connections: " + conn.current + "/" + conn.available);

// 3. Check replication (if replica set)
rs.status().ok  // Should return 1

// 4. Check for slow operations
db.currentOp({ "secs_running": { $gt: 5 } })

// 5. Check cache efficiency
var cache = db.serverStatus().wiredTiger.cache;
var hitRatio = 1 - (cache["pages read into cache"] /
  (cache["pages read into cache"] + cache["pages requested from the cache"]));
print("Cache hit ratio: " + (hitRatio * 100).toFixed(2) + "%");

// 6. Check for lock contention
db.currentOp({ waitingForLock: true })
```
{: codeblock}



## Performance troubleshooting flowchart
{: #flowchart}

```sh
┌─────────────────────────────────┐
│   Performance issue detected    │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  Check IBM Cloud Monitoring     │
│  - CPU > 80%?                   │
│  - Memory > 80%?                │
│  - Disk latency high?           │
└────────────┬────────────────────┘
             │
        ┌────┴────┐
        │  YES    │
        ▼         │
┌──────────────┐  │
│ Scale        │  │
│ resources    │  │
└──────────────┘  │
                  │ NO
                  ▼
        ┌─────────────────────┐
        │ Check slow queries  │
        │ db.system.profile   │
        └─────────┬───────────┘
                  │
             ┌────┴────┐
             │  Found? │
             ▼         │
        ┌─────────┐    │
        │ Optimize│    │
        │ queries │    │
        │ & indexes│   │
        └─────────┘    │
                       │ NO
                       ▼
              ┌────────────────┐
              │ Check Locks    │
              │ currentOp()    │
              └────────┬───────┘
                       │
                  ┌────┴────┐
                  │ Locked? │
                  ▼         │
             ┌─────────┐    │
             │ Kill or │    │
             │ optimize│    │
             └─────────┘    │
                            │ NO
                            ▼
                   ┌────────────────┐
                   │ Check cache    │
                   │ hit ratio      │
                   └────────┬───────┘
                            │
                       ┌────┴────┐
                       │ < 95%?  │
                       ▼         │
                  ┌─────────┐    │
                  │ Scale   │    │
                  │ memory  │    │
                  └─────────┘    │
                                 │ NO
                                 ▼
                        ┌────────────────┐
                        │ Check          │
                        │ replication    │
                        └────────┬───────┘
                                 │
                            ┌────┴────┐
                            │ Lagging?│
                            ▼         │
                       ┌─────────┐    │
                       │ Scale   │    │
                       │ or fix  │    │
                       └─────────┘    │
                                      │ NO
                                      ▼
                             ┌────────────────┐
                             │ Contact IBM    │
                             │ Support        │
                             └────────────────┘
```
{: caption="Troubleshooting flowchart" caption-side="bottom"}


## Common anti-patterns
{: #anti-patterns}

Avoid these common mistakes that lead to performance issues.

### Query anti-patterns
{: #query-anti-patterns}

#### 1. Missing indexes
{: #missing-indexes}

**Problem:**
```js
// No index on 'email' field
db.users.find({ email: "user@example.com" })
```

**Solution:**
```js
// Create index
db.users.createIndex({ email: 1 })
```

#### 2. Inefficient regex queries
{: #regex}

**Problem:**
```js
// Case-insensitive regex without index
db.users.find({ name: /john/i })
```

**Solution:**
```js
// Use text index or exact match
db.users.createIndex({ name: "text" })
db.users.find({ $text: { $search: "john" } })
```

#### 3. Large skip() operations
{: #skip}

**Problem:**
```js
// Skipping thousands of documents
db.collection.find().skip(10000).limit(10)
```

**Solution:**
```js
// Use range queries with indexed field
db.collection.find({ _id: { $gt: lastSeenId } }).limit(10)
```

#### 4. Selecting unnecessary fields
{: #fields}

**Problem:**
```js
// Fetching entire documents
db.users.find({ status: "active" })
```

**Solution:**
```js
// Use projection
db.users.find({ status: "active" }, { name: 1, email: 1 })
```

#### 5. Inefficient aggregation pipelines
{: #pipelines}

**Problem:**
```js
// $match after $lookup
db.orders.aggregate([
  { $lookup: { ... } },
  { $match: { status: "completed" } }
])
```

**Solution:**
```js
// $match first to reduce documents
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $lookup: { ... } }
])
```

### Schema design issues
{: #schema-design}

#### 1. Unbounded arrays
{: #arrays}

**Problem:**
```js
// Array grows indefinitely
{
  userId: 123,
  activities: [/* thousands of items */]
}
```

**Solution:**
```js
// Use separate collection or bucketing
{
  userId: 123,
  month: "2024-01",
  activities: [/* limited items */]
}
```

#### 2. Excessive embedding
{: #embedding}

**Problem:**
```js
// Deeply nested documents
{
  user: {
    profile: {
      settings: {
        preferences: {
          // many levels deep
        }
      }
    }
  }
}
```

**Solution:**
```js
// Flatten or use references
{
  userId: 123,
  profileId: 456
}
```

#### 3. Large documents
{: #documents}

**Problem:**
```js
// Documents approaching 16MB limit
{
  data: "very large string...",
  attachments: [/* large binary data */]
}
```

**Solution:**
```js
// Store large data separately (GridFS or object storage)
{
  dataRef: "s3://bucket/key",
  attachments: [{ ref: "gridfs://id" }]
}
```

### Connection management mistakes
{: #management}

#### 1. Not using connection pooling
{: #connection-pooling}

**Problem:**
```js
// Creating new connection per request
app.get('/api/users', async (req, res) => {
  const client = await MongoClient.connect(uri);
  // ...
  await client.close();
});
```

**Solution:**
```js
// Reuse connection pool
const client = new MongoClient(uri, { maxPoolSize: 50 });
await client.connect();

app.get('/api/users', async (req, res) => {
  const db = client.db();
  // ...
});
```

#### 2. Not closing cursors
{: #cursors}

**Problem:**
```js
// Cursor left open
const cursor = db.collection.find();
// Never closed
```

**Solution:**
```js
// Always close cursors
const cursor = db.collection.find();
try {
  await cursor.forEach(doc => { /* process */ });
} finally {
  await cursor.close();
}
```

#### 3. Too many connections
{: #connections}

**Problem:**
```js
// One connection per user session
const connections = new Map();
users.forEach(user => {
  connections.set(user.id, new MongoClient(uri));
});
```

**Solution:**
```js
// Share connection pool across application
const client = new MongoClient(uri);
// All users share the same pool
```

### Indexing pitfalls
{: #indexing-pitfalls}

#### 1. Too many indexes
{: #too-many-indexes}

**Problem:**
```js
// Index on every field
db.collection.createIndex({ field1: 1 })
db.collection.createIndex({ field2: 1 })
db.collection.createIndex({ field3: 1 })
// ... 20+ indexes
```

**Impact:** Slows down writes and increases storage.

**Solution:** Keep only necessary indexes and use compound indexes.

#### 2. Wrong index order in compound indexes
{: #wrong-order}

**Problem:**
```js
// Query: { status: "active", createdAt: { $gt: date } }
// Index: { createdAt: 1, status: 1 }  // Wrong order
```

**Solution:**
```js
// Correct order: equality first, range second
db.collection.createIndex({ status: 1, createdAt: 1 })
```

#### 3. Not using covered queries
{: #covered-queries}

**Problem:**
```js
// Index exists but query not covered
db.users.createIndex({ email: 1 })
db.users.find({ email: "user@example.com" }, { name: 1, email: 1 })
// Still fetches documents
```

**Solution:**
```js
// Include all projected fields in index
db.users.createIndex({ email: 1, name: 1 })
db.users.find({ email: "user@example.com" }, { name: 1, email: 1, _id: 0 })
```

## Appendix: metrics thresholds
{: #metrics-thresholds}

Recommended thresholds for key performance metrics.

| Metric | Warning threshold | Critical threshold | Recommended action |
|--------|------------------|-------------------|-------------------|
| **CPU utilization** | > 75% | > 90% | Scale CPU cores |
| **Memory utilization** | > 80% | > 95% | Scale memory allocation |
| **Disk utilization** | > 80% | > 90% | Scale disk space |
| **Disk IOPS** | > 80% of limit | > 95% of limit | Increase disk size for more IOPS |
| **Active connections** | > 80% of limit | > 95% of limit | Scale plan or optimize connection pooling |
| **Replication lag** | > 5 seconds | > 30 seconds | Investigate and scale if needed |
| **Cache hit ratio** | < 95% | < 90% | Scale memory or optimize queries |
| **Query execution time** | > 100ms (avg) | > 1000ms (avg) | Optimize queries and indexes |
| **Lock wait time** | > 100ms | > 1000ms | Optimize operations and kill long-running queries |
| **Page faults** | > 100/sec | > 1000/sec | Scale memory |
| **Network latency** | > 10ms | > 50ms | Check network configuration |
| **Backup duration** | > 1 hour | > 4 hours | Consider scaling or optimization |
{: caption="Metrics thresholds" caption-side="top"}

### Monitoring frequency recommendations
{: #frequency}

| Metric category | Check frequency | Retention period |
|----------------|----------------|------------------|
| Resource utilization | Every 1 minute | 30 days |
| Query performance | Every 5 minutes | 14 days |
| Replication status | Every 1 minute | 30 days |
| Connection statistics | Every 5 minutes | 14 days |
| Backup status | Every 1 hour | 90 days |
| Disk growth | Every 1 hour | 90 days |
{: caption="Monitoring frequency" caption-side="top"}

### Alert configuration examples
{: #config-examples}

#### CPU alert
{: #cpu}

```sh
Condition: CPU > 80% for 10 consecutive minutes
Action: Send notification to ops team
Escalation: Page on-call if > 90% for 15 minutes
```

#### Memory alert
{: #memory}

```sh
Condition: Memory > 85% for 15 consecutive minutes
Action: Send notification to ops team
Escalation: Auto-scale if > 95% for 10 minutes
```

#### Replication lag alert
{: #lag}

```sh
Condition: Lag > 10 seconds
Action: Send notification immediately
Escalation: Page on-call if > 60 seconds
```

#### Disk space alert
{: #disk-space}

```sh
Condition: Disk > 80%
Action: Send notification to ops team
Escalation: Create incident if > 90%
```

## Best practices summary
{: #summary}

| Area | Recommendation |
|------|---------------|
| **Indexing** | Regularly review and remove unused indexes |
| **Monitoring** | Configure alerts for CPU, memory, disk, and replication lag |
| **Capacity planning** | Keep disk usage below 80% and scale proactively |
| **Query design** | Use explain plans during development |
| **Scaling** | Scale proactively before saturation |
| **Connection pooling** | Use connection pools and avoid per-request connections |
| **Read preferences** | Use secondaries for read-heavy workloads |
| **Write concern** | Balance durability with performance needs |
| **Schema design** | Avoid unbounded arrays and excessive embedding |
| **Backup planning** | Schedule during low-traffic periods |
| **Network** | Use private endpoints for {{site.data.keyword.cloud_notm}} workloads |
| **Security** | Rotate credentials regularly and use IP allowlisting |
| **Documentation** | Document baseline metrics and normal patterns |
| **Testing** | Test performance changes in non-production first |
| **Support** | Gather diagnostics before contacting support |
{: caption="Best practices" caption-side="top"}


## Additional resources
{: #resources}

### {{site.data.keyword.cloud_notm}} documentation
{: #docs}

* [{{site.data.keyword.cloud_notm}} Databases for MongoDB documentation](/docs/databases-for-mongodb)
* [IBM Cloud Monitoring documentation](/docs/monitoring)
* [IBM Cloud Activity Tracker documentation](/docs/activity-tracker)
* [{{site.data.keyword.cloud_notm}} CLI reference](/docs/cli)

### MongoDB documentation
{: #mongodb-docs}

* [MongoDB performance best practices](https://docs.mongodb.com/manual/administration/analyzing-mongodb-performance/)
* [MongoDB indexing strategies](https://docs.mongodb.com/manual/applications/indexes/)
* [MongoDB query optimization](https://docs.mongodb.com/manual/core/query-optimization/)
* [WiredTiger Storage Engine](https://docs.mongodb.com/manual/core/wiredtiger/)

### Community resources
{: #community-resources}

* [MongoDB community forums](https://www.mongodb.com/community/forums/)
* [Stack Overflow - MongoDB Tag](https://stackoverflow.com/questions/tagged/mongodb)
* [IBM Cloud community](https://community.ibm.com/community/user/cloud/home)
