---

copyright:
  years: 2026
lastupdated: "2026-04-02"

keywords: mongodb, databases, monitoring, scaling, autoscaling, resources, troubleshooting, flowchart

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Best practices for performance
{: #best-practices}

Use this information to apply best practices to your {{site.data.keyword.databases-for-mongodb}} deployment running on {{site.data.keyword.cloud_notm}}.

## Performance troubleshooting flowchart
{: #flowchart}

Use the flowchart to determine how to troubleshoot performance and steps to take next.

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
| **CPU utilization** | &amp;gt; 75% | &amp;gt; 90% | Scale CPU cores |
| **Memory utilization** | &amp;gt; 80% | &amp;gt; 95% | Scale memory allocation |
| **Disk utilization** | &amp;gt; 80% | &amp;gt; 90% | Scale disk space |
| **Disk IOPS** | &amp;gt; 80% of limit | &amp;gt; 95% of limit | Increase disk size for more IOPS |
| **Active connections** | &amp;gt; 80% of limit | &amp;gt; 95% of limit | Scale plan or optimize connection pooling |
| **Replication lag** | &amp;gt; 5 seconds | &amp;gt; 30 seconds | Investigate and scale if needed |
| **Cache hit ratio** | < 95% | < 90% | Scale memory or optimize queries |
| **Query execution time** | &amp;gt; 100ms (avg) | &amp;gt; 1000ms (avg) | Optimize queries and indexes |
| **Lock wait time** | &amp;gt; 100ms | &amp;gt; 1000ms | Optimize operations and kill long-running queries |
| **Page faults** | &amp;gt; 100/sec | &amp;gt; 1000/sec | Scale memory |
| **Network latency** | &amp;gt; 10ms | &amp;gt; 50ms | Check network configuration |
| **Backup duration** | &amp;gt; 1 hour | &amp;gt; 4 hours | Consider scaling or optimization |
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
* [IBM Cloud Activity Tracker documentation](/docs/observability-ibm?topic=observability-ibm-getting-started-at)
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
