---

copyright:
  years: 2026
lastupdated: "2026-04-02"

keywords: mongodb, databases, monitoring, scaling, autoscaling, resources, troubleshooting

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloud_notm}} specific tools and diagnostic commands for troubleshooting performance
{: #features-tools}


## {{site.data.keyword.cloud_notm}} specific tools and features
{: #tools}

You can use various tools and features of {{site.data.keyword.cloud_notm}} to aid performance troubleshooting.

### Using IBM Cloud Monitoring (Sysdig)
{: #sysdig}

MongoDB integrates with IBM Cloud Monitoring powered by Sysdig for comprehensive observability.

#### Accessing monitoring dashboards
{: #dashboards}

1. Navigate to your MongoDB deployment in {{site.data.keyword.cloud_notm}} console.
2. Click **Monitoring** in the left navigation.
3. Click **Launch Monitoring** to open the Sysdig dashboard.

#### Key metrics to track:
{: #key-metrics}

* **Platform metrics:**
    * **CPU utilization** - target: < 75% sustained
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

### Audit trail for compliance
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

## {{site.data.keyword.cloud_notm}} CLI and API for diagnostics
{: #cli-api-diagnostics}

Use {{site.data.keyword.cloud_notm}} CLI and API for automated diagnostics and monitoring.

### Installing {{site.data.keyword.cloud_notm}} CLI
{: #install-cli}

```bash
# Install IBM Cloud CLI
curl -fsSL https://clis.cloud.ibm.com/install/linux | sh

# Install databases plugin
ibmcloud plugin install cloud-databases
```
{: codeblock}

### Essential diagnostic commands
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

### Using the {{site.data.keyword.cloud_notm}} API
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


## {{site.data.keyword.cloud_notm}} network optimization
{: #network}

Network configuration significantly impacts MongoDB performance, especially for distributed applications. Compare private endpoints with public endpoints:

### Private endpoints (recommended)
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

### Public endpoints
{: #public-endpoints}

**Use cases:**
* External applications
* Development and testing
* Hybrid cloud scenarios

**Security considerations:**
* Use IP allowlisting.
* Enforce TLS/SSL.
* Rotate credentials regularly.

### Service endpoints
{: #service-endpoints}

{{site.data.keyword.cloud_notm}} service endpoints provide optimized connectivity within {{site.data.keyword.cloud_notm}}.

### Benefits
{: #service-endpoints-benefits}

* Reduced latency
* No public internet traversal
* Improved security posture
* Cost savings on bandwidth

### Configuration
{: #service-endpoints-config}

```bash
# Enable service endpoint
ibmcloud cdb deployment-service-endpoint-enable <deployment-id>
```
{: codeblock}

### Multi-zone deployment considerations
{: #multizone}

{{site.data.keyword.databases-for-mongodb}} can span multiple availability zones.



### Best practices
{: #multizone-best}

* Deploy applications in the same region.
* Use read preferences to minimize latency.
* Consider `nearest` read preference for multi-zone apps.
* Monitor replication lag between zones.

### Network latency troubleshooting
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

### MongoDB connection diagnostics
{: #measure-latency-connection}

```js
// Check network latency
db.runCommand({ ping: 1 })

// Check connection pool stats
db.serverStatus().connections
```
{: codeblock}

### Geographic distribution
{: #geographic}

For globally distributed applications:

### Strategies
{: #latency-strategies}

* **Single region**: Lowest latency, single point of failure
* **Multi-region with read replicas**: Read scaling, eventual consistency
* **Cross-region replication**: Disaster recovery, higher latency

### Recommendations
{: #latency-recommendations}

* Place database close to primary user base.
* Use CDN for static content.
* Implement application-level caching.
* Consider data residency requirements.

### Bandwidth optimization
{: #latency-bandwidth}

* Use projections to limit data transfer.
* Implement pagination for large result sets.
* Compress data at application level.
* Use bulk operations to reduce round trips.

### Connection pooling best practices
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
