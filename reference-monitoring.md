---
copyright:
  years: 2020
lastupdated: "2020-12-17"

keywords: mongodb, sysdig, monitoring, metrics, iops, disk usage, memory usage, page faults

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:important: .important}

# Sysdig Monitoring Integration
{: #sysdig-monitoring}

Monitoring for {{site.data.keyword.databases-for-mongodb_full}} deployments is provided through integration with the Sysdig Monitoring service. Your deployments forward selected information so you can monitor deployment health and resource usage. To see your {{site.data.keyword.databases-for-mongodb}} dashboards in Sysdig, you have to [Enable Platform Metrics](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-platform_metrics_enabling) in the same region as your deployment. If you have deployments in more than one region, you have to provision Sysdig and enable platform metrics in each region.

To access Sysdig from your deployment, use the _Monitoring_ link from the right menu. (If you do not already have a Sysdig service in the same region as your deployment it says _Add monitoring_.)

![The Monitoring link in a deployment](images/monitoring-ui-link.png)

To access your deployment's Sysdig dashboard from Sysdig, it's in the sidebar, under _IBM_.

![Cloud databases dashboard in Sysdig](images/monitoring-ibm-list.png)

## Sysdig Availability

Sysdig monitoring is available for deployments in every region. Deployments in Multi-zone Regions (MZRs) - `eu-gb`, `eu-de`, `us-east`, `us-south`, `jp-tok`, `au-syd` - have their metrics in the corresponding Sysdig region.

If you have deployments that are in a Single-zone Region (SZR) - `osl01`, `che01`, or `seo01` - then your logs are forwarded to a Sysdig instance in another region. You need to provision Sysdig instances in the region where your metrics are forwarded to. Metrics for deployments in `osl01` go to `eu-gb`. Metrics for deployments in `seo01` and `che01` go to `jp-tok`. 

## Available Metrics
{: metrics-by-plan}

| Metric Name |
|-----------|
| [Average time spent acquiring locks in microseconds](#ibm_databases_for_mongodb_locks_time_acquiring_microseconds_total_average) | 
| [Average time spent acquiring locks in microseconds](#ibm_databases_for_mongodb_locks_time_acquiring_microseconds_W_average) | 
| [Connections](#ibm_databases_for_mongodb_connections) | 
| [IO utilization as a percent - 5 minute average](#ibm_databases_for_mongodb_disk_io_utilization_percent_average_5m) |
| [IO utilization as a percent - 15 minute average](#ibm_databases_for_mongodb_disk_io_utilization_percent_average_15m) | 
| [IO utilization as a percent - 30 minute average](#ibm_databases_for_mongodb_disk_io_utilization_percent_average_30m) | 
| [IO utilization as a percent - 60 minute average](#ibm_databases_for_mongodb_disk_io_utilization_percent_average_60m) | 
| [IOPS read & write total count for an instance.](#ibm_databases_for_mongodb_disk_iops_read_write_total) | 
| [Max allowed memory for an instance.](#ibm_databases_for_mongodb_memory_limit_bytes) | 
| [Oplog gigabyte per hour](#ibm_databases_for_mongodb_oplog_gb_per_hour) | 
| [Oplog used bytes](#ibm_databases_for_mongodb_oplog_used_bytes) | 
| [Oplog used bytes percent of total](#ibm_databases_for_mongodb_oplog_used_bytes_percent) | 
| [Oplog window hours](#ibm_databases_for_mongodb_oplog_window_hours) | 
| [Page faults](#ibm_databases_for_mongodb_page_faults) | 
| [Process resident memory in bytes](#ibm_databases_for_mongodb_process_resident_memory_bytes) |
| [Process virtual memory in bytes](#ibm_databases_for_mongodb_process_virtual_memory_bytes) |
| [Replica set member state](#ibm_databases_for_mongodb_status) |
| [Replication lag](#ibm_databases_for_mongodb_replica_lag) |
| [Status](#ibm_databases_for_mongodb_status) | 
| [Total disk space for an instance.](#ibm_databases_for_mongodb_disk_total_bytes) | 
| [Used CPU for an instance.](#ibm_databases_for_mongodb_cpu_used_percent) | 
| [Used disk space for an instance.](#ibm_databases_for_mongodb_disk_used_bytes) | 
| [Used memory for an instance.](#ibm_databases_for_mongodb_memory_used_bytes) | 
{: caption="Table 1. Available Metrics" caption-side="top"}


### Average time spent acquiring locks in microseconds
{: #ibm_databases_for_mongodb_locks_time_acquiring_microseconds_total_average}

Average time spent acquiring locks in microseconds

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_locks_time_acquiring_microseconds_total_average`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance` |
{: caption="Table 2: Average time spent acquiring locks in microseconds metric metadata" caption-side="top"}

### Average time spent acquiring locks in microseconds
{: #ibm_databases_for_mongodb_locks_time_acquiring_microseconds_W_average}

Average time spent acquiring locks in microseconds

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_locks_time_acquiring_microseconds_W_average`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance` |
{: caption="Table 3: Average time spent acquiring locks in microseconds metric metadata" caption-side="top"}
### Connections
{: #ibm_databases_for_mongodb_connections}

The number of connections to the database.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_connections`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 2. Connections metric metadata" caption-side="top"}

### IO utilization in percent 5 minute average
{: #ibm_databases_for_mongodb_disk_io_utilization_percent_average_5m}

How much disk I/O has been used over 5 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_io_utilization_percent_average_5m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 3. IO utilization in percent 5 minute average metric metadata" caption-side="top"}

### IO utilization in percent 15 minute average
{: #ibm_databases_for_mongodb_disk_io_utilization_percent_average_15m}

How much disk I/O has been used over 15 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_io_utilization_percent_average_15m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 4. IO utilization in percent 15 minute average metric metadata" caption-side="top"}

### IO utilization in percent 30 minute average
{: #ibm_databases_for_mongodb_disk_io_utilization_percent_average_30m}

How much disk I/O has been used over 30 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_io_utilization_percent_average_30m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 5. IO utilization in percent 30 minute average metric metadata" caption-side="top"}

### IO utilization in percent 60 minute average
{: #ibm_databases_for_mongodb_disk_io_utilization_percent_average_60m}

How much disk I/O has been used over 60 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_io_utilization_percent_average_60m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 6. IO utilization in percent 60 minute average metric metadata" caption-side="top"}

### IOPS read & write total count for an instance
{: #ibm_databases_for_mongodb_disk_iops_read_write_total}

How many input/output operations per second your deployment is performing.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_iops_read_write_total`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 7. IOPS read & write total count for an instance metric metadata" caption-side="top"}

### Max allowed memory for an instance
{: #ibm_databases_for_mongodb_memory_limit_bytes}

The maximum amount of memory available to your deployment.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_memory_limit_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 8. Max allowed memory for an instance metric metadata" caption-side="top"}

### Oplog gigabyte per hour
{: #ibm_databases_for_mongodb_oplog_gb_per_hour}

The gigabytes of oplog per hour the primary generates

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_oplog_gb_per_hour`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 59: Oplog gigabyte per hour metric metadata" caption-side="top"}

### Oplog used bytes
{: #ibm_databases_for_mongodb_oplog_used_bytes}

The total amount of space used by the oplog in bytes.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_oplog_used_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 60: Oplog used bytes metric metadata" caption-side="top"}

### Oplog used bytes percent of total
{: #ibm_databases_for_mongodb_oplog_used_bytes_percent}

The total used oplog space in percent

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_oplog_used_bytes_percent`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 61: Oplog used bytes percent of total metric metadata" caption-side="top"}

### Oplog window hours
{: #ibm_databases_for_mongodb_oplog_window_hours}

The approximate number of hours available in the oplog.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_oplog_window_hours`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 62: Oplog window hours metric metadata" caption-side="top"}

### Page faults
{: #ibm_databases_for_mongodb_page_faults}

The number of times per second that MongoDB had to request data from disk. Scale RAM to reduce the number of disk requests.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_page_faults`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 9. Page faults metric metadata" caption-side="top"}

### Process resident memory in bytes
{: #ibm_databases_for_mongodb_process_resident_memory_bytes}

Amount of actual physical memory used by the MongoDB process. Resident memory (the number of megabytes resident): In a standard deployment resident is the amount of memory used by the WiredTiger cache plus the memory dedicated to other in-memory structures used by the `mongod` process. By default, `mongod` with WiredTiger reserves 50% of the total physical memory on the server for the cache and at a steady state, WiredTiger tries to limit cache usage to 80% of that total. For example, if a server has 16GB of memory, WiredTiger will assume it can use 8GB for cache and at steady state should use about 6.5GB. To learn more about the WiredTiger Storage Engine, reference the [MongoDB documentation](/docs/databases-for-mongodb?topic=databases-for-mongodb-performance#wiredtiger-cache-and-memory).

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_process_resident_memory_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 64: Process resident memory in bytes metric metadata" caption-side="top"}

### Process virtual memory in bytes
{: #ibm_databases_for_mongodb_process_virtual_memory_bytes}

Amount of virtual memory used by the `mongod` process. Generally, virtual memory should be slightly larger than mapped memory. If virtual memory is many gigabytes larger, it indicates that excessive memory is being used by other aspects than the memory mapping of files and is sub-optimal. The most common case of usage of a high amount of memory for non-mapped is excessive connections to the database. Each connection has a thread stack and the memory for those stacks can add up considerably.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_process_virtual_memory_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 65: Process virtual memory in bytes metric metadata" caption-side="top"}

### Replica set member state
{: #ibm_databases_for_mongodb_status}

An integer between 0 and 10 that represents the replica state of the current member.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_status`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 68: Replica set member state metric metadata" caption-side="top"}

### Replication lag
{: #ibm_databases_for_mongodb_replica_lag}

The replication lag in seconds

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_replica_lag`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance` |
{: caption="Table 69: Replication lag metric metadata" caption-side="top"}

### Status
{: #ibm_databases_for_mongodb_status}

An integer between 0 and 10 that represents the replica state of the current member.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_status`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 10. Status metric metadata" caption-side="top"}

### Total disk space for an instance
{: #ibm_databases_for_mongodb_disk_total_bytes}

Represents the total amount of disk available to your deployment

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_total_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 11. Total disk space for an instance metric metadata" caption-side="top"}

### Used CPU for an instance
{: #ibm_databases_for_mongodb_cpu_used_percent}

How much CPU is used as a percentage of total CPU available. Only for deployments that have dedicated CPU.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_cpu_used_percent`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 12. Used CPU for an instance metric metadata" caption-side="top"}

### Used disk space for an instance
{: #ibm_databases_for_mongodb_disk_used_bytes}

How much disk your deployment is using.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_used_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 13. Used disk space for an instance metric metadata" caption-side="top"}

### Used memory for an instance
{: #ibm_databases_for_mongodb_memory_used_bytes}

How much memory your deployment is using.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_memory_used_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 14. Used memory for an instance metric metadata" caption-side="top"}

## Attributes for Segmentation
{: attributes}

### Global Attributes
{: global-attributes}

The following attributes are available for segmenting all of the metrics listed above

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud Type` | `ibm_ctype` | The cloud type is a value of public, dedicated, or local. |
| `Location` | `ibm_location` | The location of the monitored resource - this may be a region, data center, or global. |
| `Resource` | `ibm_resource` | The resource being measured by the service - typically a identifying name or GUID. |
| `Resource Type` | `ibm_resource_type` | The type of the resource being measured by the service. |
| `Scope` | `ibm_scope` | The scope is the account, organization, or space GUID associated with this metric. |

{: caption="Table 15. Global Attributes Metadata" caption-side="top"}

### Additional Attributes
{: additional-attributes}

The following attributes are available for segmenting one or more attributes as described in the reference above.  Please see the individual metrics for segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Service instance` | `ibm_service_instance` | The service instance segment identifies the instance the metric is associated with. |
{: caption="Table 14. Additional Attributes Metadata" caption-side="top"}

