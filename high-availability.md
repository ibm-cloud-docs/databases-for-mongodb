---

Copyright:
  years: 2019
lastupdated: "2019-02-19"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability
{: #high-availability}

{{site.data.keyword.databases-for-mongodb_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-mongodb}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments have one node that is the primary and the other node as the secondary. Both nodes store a copy of your data.

By contrast, application resilience and connection error handling are the responsibility of the application developer. 

## Application-level High-Availability

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-mongodb}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption is not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## Resource Scaling

{{site.data.keyword.databases-for-mongodb}} deployments do not auto-scale. Deployment owners can [monitor](/docs/services/databases-for-mongodb?topic=databases-for-mongodb-monitoring) the state of the deployment, estimate typical resource usage, and scale the deployment accordingly.

If you are planning on running operations that might put a spike in the usual RAM usage, or any data operations that could overflow your allotted storage, you should manually scale your service's resources up first to avoid hitting resource limits that would affect deployment operations.

## SLA

{{site.data.keyword.databases-for-mongodb}} deployments conform to the {{site.data.keyword.cloud_notm}} [SLA terms](/docs/overview?topic=overview-SLAs#SLAs).


