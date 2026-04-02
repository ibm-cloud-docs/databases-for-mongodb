---

copyright:
  years: 2026
lastupdated: "2026-04-02"

keywords: mongodb, databases, monitoring, scaling, autoscaling, resources, troubleshooting

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.cloud_notm}} Support integration
{: #support-integration}

Know when and how to engage {{site.data.keyword.cloud_notm}} Support for performance issues.

## Self-service resources
{: #self-service}

Before opening a ticket, check the following:

* [{{site.data.keyword.cloud_notm}} Databases documentation](https://cloud.ibm.com/docs/databases-for-mongodb)
* [MongoDB documentation](https://docs.mongodb.com/)
* [{{site.data.keyword.cloud_notm}} Status page](https://cloud.ibm.com/status)
* Community forums and Stack Overflow

## When to contact IBM Support
{: #contact-IBM}

Contact support if the following apply:

* Performance issues persist after following this guide.
* Replication lag continues despite optimization.
* Disk latency remains high without workload spikes.
* Suspected infrastructure-level issues.
* Unexpected behavior after scaling.
* Deployment health issues.
* Backup or restore problems.

## Before contacting support
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

## Opening a support ticket
{: #open-ticket}

### Using the {{site.data.keyword.cloud_notm}} console
{: #ticket-console}

1. Navigate to **Support** in the top menu.
2. Click **Create a case**.
3. Select **Databases for MongoDB**.
4. Choose severity level.
5. Provide detailed description.
6. Attach diagnostic files.

### Using the {{site.data.keyword.cloud_notm}} CLI
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

### Severity levels
{: #ticket-severity}

| Severity | Description | Response time |
|----------|-------------|---------------|
| 1 (Critical) | Production down, data loss | 1 hour |
| 2 (High) | Significant performance degradation | 2 hours |
| 3 (Medium) | Moderate impact, workaround available | 4 hours |
| 4 (Low) | General questions, feature requests | 8 hours |
{: caption="Severity levels" caption-side="top"}

### Escalation procedures
{: #escalation}

If the issue is not resolved within the expected timeframe:

1. Update the support case with urgency.
2. Contact your IBM account team.
3. For critical issues, request management escalation.

### Support best practices
{: #support-best}

* Provide complete information upfront.
* Respond promptly to support requests.
* Test suggested solutions in non-production first.
* Document resolution for future reference.

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
