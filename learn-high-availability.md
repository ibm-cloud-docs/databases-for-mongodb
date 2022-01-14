---

copyright:
  years: 2019, 2020
lastupdated: "2021-01-14"

keywords: mongodb, databases

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:important: .important}

# High-Availability
{: #high-availability}

{{site.data.keyword.databases-for-mongodb_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} environment. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

Both {{site.data.keyword.databases-for-mongodb}} Standard and Enterprise Edition deployments consist of [three data nodes](https://docs.mongodb.com/manual/core/replica-set-architecture-three-members/#primary-with-two-secondary-members-p-s-s), one a primary node and the other two as secondary nodes. They are configured as a replication set without sharding, so they provide two complete copies of the data set at all times in addition to the primary. If the primary is unavailable, the replica set elects a secondary to be primary and continues normal operation. The old primary rejoins the set when available. 

Connections to a MongoDB replica are made by supplying the driver or binary the replica set name and a seed list for the hosts in the replica set. This configuration allows drivers and applications the ability to change which host they are connecting to if one of the hosts becomes unavailable. Your deployment has a [connection string](/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings) in the [DNS Seedlist Connection format](https://docs.mongodb.com/manual/reference/connection-string/#dns-seedlist-connection-format), so your applications can take advantage of this feature.
 
## Application-level High-Availability
{: #high-availability-app-level}

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-mongodb}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruptions are not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## High availability, disaster recovery, and SLA resources
{: #high-availability-da-sla}

Issuing commands that break replication or force node shut down, such as `db.shutdown()`, will break the high availability of your database, void your SLA, and will require restoring from a backup.
{: .important}

{{site.data.keyword.databases-for-mongodb}} deployments conform to the {{site.data.keyword.cloud_notm}} Databases [HA, DR, and SLA](/docs/cloud-databases?topic=cloud-databases-ha-dr) information and terms.

You can find more information about best practices when using your MongoDB deployment on the IBM Cloud in this [Best Practices blog post.](https://www.ibm.com/cloud/blog/best-practices-for-mongodb-on-the-ibm-cloud)
{: .tip}
