---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-05"

keywords: troubleshooting for MongoDB

subcollection: databases-for-mongodb

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can’t I connect to my MongoDB deployment?
{: #troubleshoot-connect}
{: troubleshoot}
{: support}

If you encounter errors when connecting to your {{site.data.keyword.databases-for-mongodb_full}} deployment, review these common causes and resolutions.
{: shortdesc}

You receive an error message or fail to connect to a {{site.data.keyword.databases-for-mongodb}} deployment. If reviewing application logs, you might see errors that mention intermittent connection timeouts or unable to connect.
{: tsSymptoms}

* An unsecured connection is a common cause of connectivity errors.  All {{site.data.keyword.databases-for-mongodb}} connections use TLS/SSL encryption; {{site.data.keyword.databases-for-mongodb}} rejects unsecured connections.  To avoid errors, make sure you configured a secure connection. Refer to [Getting Started](/docs/databases-for-mongodb?topic=databases-for-mongodb-getting-started-new) for an example of a secure connection.
* If you are using a private endpoint, make sure that you specify connection strings that contain the private endpoint (see [Credentials for Private Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints&interface=ui#private-endpoints-credentials)) and that you follow the steps in [Connecting through Private Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints#private-endpoint-connections).
* If your application log captures a short connection interruption, that behavior is expected as a normal part of operations for this managed service. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud}}. However, if you experience several minutes of connection interruption check the Cloud Status for the service. For more information, see [Application-level high availability](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-elasticsearch-ha-dr#application-level-ha).
{: #tsResolve}
