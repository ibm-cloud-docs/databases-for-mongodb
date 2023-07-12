---

copyright:
  years: 2023
lastupdated: "2023-07-12"

keywords: troubleshooting for MongoDB, workload, logging, latency mean, disk latency

subcollection: databases-for-mongodb

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How can I measure a deployment's workload?
{: #troubleshoot-workload}
{: troubleshoot}
{: support}

You'd like to upgrade your {{site.data.keyword.databases-for-mongodb}}, but you want check if it's being used. 
{: shortdesc}

You're ready to upgrade to a new major version, but you'd like to ensure that your deployment isn't under heavy workload. 
{: tsSymptoms}

Check your [platform metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling){: external} within {{site.data.keyword.monitoringfull}}. 

Specifically, [Disk read latency mean (`ibm_databases_for_mongodb_disk_read_latency_mean`)](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring#ibm_databases_for_mongodb_disk_read_latency_mean){: external} indicates your deployment's current workload. Disk read latency mean is the time that it takes for MongoDB to read data from disk. It is measured in milliseconds. A low disk read latency indicates that MongoDB is able to read data from disk quickly. A high disk read latency indicates that MongoDB is taking a long time to read data from disk.

For more information, see [Monitoring Integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring).
{: tsResolve}
