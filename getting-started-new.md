---
copyright:
  years: 2019, 2023
lastupdated: "2023-05-30"

keywords: mongodb, databases, mongodb compass, mongodbee, mongodb enterprise, mongodb ee provision, mongodb compass, mongodb ops manager

subcollection: databases-for-mongodb

content-type: tutorial
services:
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting Started with {{site.data.keyword.databases-for-mongodb}}
{: #getting-started-new}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

This tutorial guides you through the steps to quickly start using {{site.data.keyword.databases-for-mongodb}} by provisioning an instance, creating a database and a credential, and then uploading some data. Additionally, you'll learn how to connect {{site.data.keyword.mon_full}} and {{site.data.keyword.at_full}}. Finally, you also find out how to get help with {{site.data.keyword.databases-for-mongodb}}.
{: shortdesc}

Follow these steps to complete the tutorial: {: ui}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision an {{site.data.keyword.databases-for-mongodb}} instance using the console](#provision_instance_ui)
* [Step 3: Set your Admin password using the console](#admin_password_ui)
* [Step 4: Download and install MongoDB Compass](#mongodb_compass)
* [Step 5: Configure private endpoint access](#config_priv_endpoints)
* [Step 6: Connect {{site.data.keyword.monitoringshort}}](#connect_monitoring_ui)
* [Step 7: Connect Activity Tracker](#activity_tracker_ui)
* [Step 8: If you need more help](#getting_help)
{: ui}

Follow these steps to complete the tutorial: {: cli}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision an {{site.data.keyword.databases-for-mongodb}} instance using the CLI](#provision_instance_cli)
* [Step 3: Set your Admin password](#admin_password_cli)
* [Step 4: Download and install MongoDB Compass](#mongodb_compass)
* [Step 5: Configure private endpoint access](#config_priv_endpoints_cli)
* [Step 6: Connect {{site.data.keyword.monitoringshort}}](#connect_monitoring_cli)
* [Step 7: Connect Activity Tracker](#activity_tracker_cli)
* [Step 8: If you need more help](#getting_help)
{: cli}

Follow these steps to complete the tutorial: {: api}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision an {{site.data.keyword.databases-for-mongodb}} instance using an API](#provision_instance_api)
* [Step 3: Set your Admin password](#admin_password_api)
* [Step 4: Download and install MongoDB Compass](#mongodb_compass)
* [Step 5: Configure private endpoint access](#config_priv_endpoints_api)
* [Step 6: Connect IBM Cloud Monitoring](#connect_monitoring_api)
* [Step 7: Connect Activity Tracker](#activity_tracker_api)
* [Step 8: If you need more help](#getting_help)
{: api}


## Before you begin
{: #prereqs}

* You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.

## Step 1: Choose your plan
{: #choose_plan}

{{site.data.keyword.databases-for-mongodb}} offers two different plans. For more information, see [{{site.data.keyword.databases-for-mongodb}} Plans](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-plans){: external}.

* [{{site.data.keyword.databases-for-mongodb}}] is a fully managed NoSQL database service based on the MongoDB Community Edition.

* [{{site.data.keyword.databases-for-mongodb}} Enterprise](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-plans#mongodb-plans-ee){: external} offers advanced features, such as the [MongoDB Ops Manager](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-plans#ops-manager){: external}, the [Analytics Add-on](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-plans#analytics-add-on){: external}, and [point-in-time recovery](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-plans#point-in-time-recovery){: external}.

### Using APIs
{: #using_apis}
{: api}

Use the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction){: external} to work with your {{site.data.keyword.databases-for-mongodb}} instance. The resource controller API is used to [provision an instance](#provision_instance_api).


## Step 2: Provision an {{site.data.keyword.databases-for-mongodb}} instance through the console
{: #provision_instance_ui}
{: ui}

1. Log in to the {{site.data.keyword.cloud_notm}} console.
1. Click the [**{{site.data.keyword.databases-for-mongodb}} service**](https://cloud.ibm.com/databases/databases-for-mongodb/create){: external} in the **Catalog**.

1. In **Service Details**, configure the following:
    - **Service name** - The name can be any string and is the name that is used on the web and in the CLI to identify the new deployment.
    - **The Resource group** - If you are organizing your services into [resource groups](/docs/account?topic=account-account_setup), specify the resource group in this field. Otherwise, you can leave it at default. For more information, see [Managing resource groups](/docs/account?topic=account-rgs).
    - **Location** - The deployment's public cloud region or Satellite location.
1. **Resource allocation** - Specify initial RAM, disk, and cores for your databases. The minimum sizes of memory and disk are selected by default. With dedicated cores, your resource group is given a single-tenant host with a minimum reserve of CPU shares. Your deployments are then allocated the number of cores you specify. *Once provisioned, disk cannot be scaled down.*

1. In **Service Configuration**, configure the following:
    - **Database Version** [Set only at deployment]{: tag-red} - The deployment version of your database. To ensure optimal performance, run the preferred version. The latest minor version is used automatically. For more information, see [Database Versioning Policy](/docs/cloud-databases?topic=cloud-databases-versioning-policy){: external}.
    - **Database Edition** [Set only at deployment]{: tag-red} - Select the edition you would like to provision. For more information, see [{{site.data.keyword.databases-for-mongodb}} Plans](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-plans){: external}.
    - **Encryption** - If you use [Key Protect](/docs/cloud-databases?topic=cloud-databases-key-protect&interface=ui), an instance and key can be selected to encrypt the deployment's disk. If you do not use your own key, the deployment automatically creates and manages its own disk encryption key.
    - **Endpoints** [Set only at deployment]{: tag-red} - Configure the [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) on your deployment. *A {{site.data.keyword.databases-for-mongodb}} instance cannot have both public and private endpoints simultaneously*.

    After you configure the appropriate settings, click **Create** to start the provisioning process. The {{site.data.keyword.databases-for-mongodb}} **Resource list** page opens.

1. Click **Create**. The {{site.data.keyword.databases-for}} **Resource list** page opens.

1. When your instance has been provisioned, click on the instance name to view more information.

## Step 2: Provision a {{site.data.keyword.databases-for-mongodb}} instance through the CLI
{: #provision_instance_cli}
{: cli}

You can provision a {{site.data.keyword.databases-for-mongodb}} instance using the CLI. If you don't already have it, you will need to install the [{{site.data.keyword.cloud_notm}} CLI](https://www.ibm.com/cloud/cli){: external}.

1. Log in to {{site.data.keyword.cloud_notm}} with the following command:
{: #step2_login_qsg}

    ```sh
    ibmcloud login
    ```
    {: pre}

    If you use a federated user ID, it's important that you switch to a one-time passcode (`ibmcloud login --sso`), or use an API key (`ibmcloud --apikey key` or `@key_file`) to authenticate. For more information about how to log in using the CLI, see [General CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login) under `ibmcloud login`.

1. Create a {{site.data.keyword.databases-for-mongodb}} instance.
{: #step3_es_instance}

    Select one of the following methods:

    * To create an instance from the CLI on the Enterprise plan, run the following command:

        ```sh
        ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE_NAME> <SERVICE_PLAN_NAME> <LOCATION> <SERVICE_ENDPOINTS_TYPE> <RESOURCE_GROUP>
        ```
        {: codeblock}

   The fields in the command are described in the table that follows.
   | Field | Description | Flag |
   |-------|------------|------------|
   | `NAME` [Required]{: tag-red} | The instance name can be any string and is the name that is used on the web and in the CLI to identify the new deployment. |  |
   | `SERVICE_NAME` [Required]{: tag-red} | Name or ID of the service. For {{site.data.keyword.databases-for-mongodb}}, use `databases-for-mongodb`. |  |
   | `SERVICE_PLAN_NAME` [Required]{: tag-red} | Standard plan (`standard`) or Enterprise plan (`enterprise`) |  |
   | `LOCATION` [Required]{: tag-red} | The location where you want to deploy. To retrieve a list of regions, use the `ibmcloud regions` command. |  |
   | `SERVICE_ENDPOINTS_TYPE` | Configure the [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) of your deployment, either `public` or `private`. The default value is `public`. *A MongoDB deployment cannot have both public and private endpoints simultaneously. This parameter cannot be changed after provisioning.* |  |
   | `RESOURCE_GROUP` | The Resource group name. The default value is `default`. | -g |
   | `--parameters` | JSON file or JSON string of parameters to create service instance | -p |
   {: caption="Table 1. Basic command format fields" caption-side="top"}

   You will see a response like:

   ```text
   Creating service instance INSTANCE_NAME in resource group default of account    USER...
   OK
   Service instance INSTANCE_NAME was created.

   Name:                INSTANCE_NAME
   ID:                  crn:v1:bluemix:public:databases-for-mongodb:us-east:a/   40ddc34a846383BGB5b60e:dd13152c-fe15-4bb6-af94-fde0af5303f4::
   GUID:                dd13152c-fe15-4bb6-af94-fde0af56897
   Location:            LOCATION
   State:               provisioning
   Type:                service_instance
   Sub Type:            Public
   Service Endpoints:   public
   Allow Cleanup:       false
   Locked:              false
   Created at:          2023-06-26T19:42:07Z
   Updated at:          2023-06-26T19:42:07Z
   Last Operation:
                        Status    create in progress
                        Message   Started create instance operation
   ```
   {: codeblock}

1. To check provisioning status, use the following command:

   ```sh
   ibmcloud resource service-instance <INSTANCE_NAME>
   ```
   {: pre}

   When complete, you will see a response like:

   ```text
   Retrieving service instance INSTANCE_NAME in resource group default under account USER's Account as USER...
   OK

   Name:                  INSTANCE_NAME
   ID:                    crn:v1:bluemix:public:databases-for-mongodb:us-east:a/40ddc34a953a8c02f109835656860e:dd13152c-fe15-4bb6-af94-fde0af5303f4::
   GUID:                  dd13152c-fe15-4bb6-af94-fde5654765
   Location:              <LOCATION>
   Service Name:          databases-for-mongodb
   Service Plan Name:     standard
   Resource Group Name:   default
   State:                 active
   Type:                  service_instance
   Sub Type:              Public
   Locked:                false
   Service Endpoints:     public
   Created at:            2023-06-26T19:42:07Z
   Created by:            USER
   Updated at:            2023-06-26T19:53:25Z
   Last Operation:
                          Status    create succeeded
                          Message   Provisioning mongodb with version 4.4 (100%)
   ```
   {: codeblck}

1. (Optional) Deleting a service instance
   Delete an instance by running a command like this one:

   ```sh
   ibmcloud resource service-instance-delete <INSTANCE_NAME>
   ```
   {: pre}

### The `--parameters` parameter
{: #flags-params-service-endpoints}
{: cli}

The `service-instance-create` command supports a `-p` flag, which allows JSON-formatted parameters to be passed to the provisioning process. Some parameter values are Cloud Resource Names (CRNs), which uniquely identify a resource in the cloud. All parameter names and values are passed as strings.

For example, if a database is being provisioned from a particular backup and the new database deployment needs a total of 9 GB of memory across three members, then the command to provision 3 GBs per member looks like:

```sh
ibmcloud resource service-instance-create databases-for-mongodb <SERVICE_NAME> standard us-south \
-p \ '{
  "backup_id": "crn:v1:blue:public:databases-for-mongodb:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "members_memory_allocation_mb": "3072"
}'
```
{: .pre}

## Step 2: Provision an {{site.data.keyword.databases-for-mongodb}} instance by using the resource controller API
{: #provision_instance_api}
{: api}

The preferred method to provision an instance is to use the [CLI](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=cli#provision_instance_cli) but if you want use the [resource controller API](/apidocs/resource-controller/resource-controller#create-resource-instance){: external}, run a command like the following to create an Enterprise instance in US South:

```sh
curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_instances -H "Authorization: ${token}" -H "Content-Type: application/json" \
-d '{ "name": "JG-test-curl", "target": "us-south", "resource_group":"9eba3cff1b0540b9ab7fb93829911da0", "resource_plan_id": "ibm.message.hub.enterprise.3nodes.2tb", "parameters":{"service-endpoints":"public","throughput":"150"}}'
```
{: codeblock}

## Step 3: Create a topic and select number of partitions by using the console
{: #create_topic_ui}
{: ui}

For guidance about the settings that you can modify when creating topics, see [topic configuration](/docs/EventStreams?topic=EventStreams-kafka_java_api){: external}.

1. From your newly provisioned instance, navigate to **Topics** using the menu on the left.
2. Click the **Create topic** button and an enter a topic name. Click **Next**. Topic names are restricted to a maximum of 200 characters.
3. Select the number of partitions.

    One or more partitions make up a topic. A partition is an ordered list of messages. 1 partition is sufficient for getting started, but production systems often have more.

    Partitions are distributed across the brokers to increase the scalability of your topic. You can also use them to distribute messages across the members of a consumer group.

    Click **Next**.


4. Set the message retention period. This is how long messages are retained before they are deleted. If your messages are not read by a consumer within this time, they will be missed.

    Click **Create topic**.

### Working with topics using the console
{: #work_topic_ui}
{: ui}

After you create topics, you can use the console to [list topics](#list_topic_ui) and [delete topics](#delete_topic_ui).


#### List topics
{: #list_topic_ui}
{: ui}

From your {{site.data.keyword.databases-for-mongodb}} instance, navigate to **Topics** from the menu on the left.

From the **Topics page**, you can view the following information about your topics:
**Name**, **Partitions**, **Retention time**, **Retention size**, **Cleanup policy**, and **Stream landing**.

#### Delete a topic
{: #delete_topic_ui}
{: ui}

From your {{site.data.keyword.databases-for-mongodb}} instance, navigate to **Topics** from the menu on the left.

From the **Topics page**, click the three dots to the right of the topic name and click **Delete this topic**.


## Step 3: Create a topic and select number of partitions by using the CLI
{: #create_topic_cli}
{: cli}

For guidance about the settings that you can modify when creating topics, see [topic configuration](/docs/EventStreams?topic=EventStreams-kafka_java_api){: external}.

Run the following [**ibmcloud es topic-create**](/docs/EventStreams?topic=EventStreams-cli_reference#ibmcloud_es){: external} command to create a new topic with one partition:

```bash
ibmcloud es topic-create [--name] topic1 [--partitions 1]
```
{: codeblock}

**Prerequisites**: None

**Command options**:

--name value
:   Topic name. Topic names are restricted to a maximum of 200 characters.

--partitions value
:   Set the number of partitions for the topic.

    One or more partitions make up a topic. A partition is an ordered list of messages. 1 partition is sufficient for getting started, but production systems often have more.

    Partitions are distributed across the brokers to increase the scalability of your topic. You can also use them to distribute messages across the members of a consumer group.

### Working with topics
{: #work_topic_cli}
{: cli}

After you create topics, you can use the CLI to [list topics](#ibmcloud_es_topic_list_cli), [delete topics](#ibmcloud_es_topic_delete_cli), and [update the configuration of topics](#ibmcloud_es_topic_update_cli). You can also [view details about your cluster](#ibmcloud_es_cluster_cli) using the CLI.


#### List a topic using the **ibmcloud es topics** command
{: #ibmcloud_es_topic_list_cli}

Run the **ibmcloud es topics** command to list your topics.

```bash
ibmcloud es topics [--filter FILTER] [--json]
```
{: codeblock}

**Prerequisites**: None

**Command options**:

--filter value, -f value (optional)
:   Topic name.

--json (optional)
:   Format output in JSON. Up to 1000 topics are returned.

#### Delete a topic using the **ibmcloud es topic-delete** command
{: #ibmcloud_es_topic_delete_cli}

Run the **ibmcloud es topic-delete** command to delete a topic.

```bash
ibmcloud es topic-delete [--name] TOPIC_NAME [--force]
```
{: codeblock}

**Prerequisites**: None

**Command options**:

--name value, -n value
:   Topic name.

--force, -f (optional)
:   Delete without confirmation.



#### Update the configuration of a topic using the **ibmcloud es topic-update** command
{: #ibmcloud_es_topic_update_cli}

Run the **ibmcloud es topic-update** command to update the configuration of a topic.

```bash
ibmcloud es topic-update [--name] TOPIC_NAME --config KEY[=VALUE][;KEY[=VALUE]]* [--default]
```
{: codeblock}

**Prerequisites**: None

**Command options**:

--name value, -n value
:   Topic name.

--config KEY[=VALUE], -c KEY[=VALUE]
:   Set a configuration option for the topic as a KEY[=VALUE] pair.
:   If VALUE is not given, the '--default' flag is to be specified to indicate resetting the configuration value back to the default. Multiple '--config' options can be specified. Each '--config' option can specify a semicolon-delimited list of assignments. The following list shows valid configuration keys:

    - cleanup.policy
    - retention.ms
    - retention.bytes
    - segment.bytes
    - segment.ms
    - segment.index.bytes

--default, -d  (optional)
:   Reset each configuration parameter that is specified by using '--config' to its default value.

#### Display cluster details using the **ibmcloud es cluster** command
{: #ibmcloud_es_cluster_cli}

Run the **ibmcloud es cluster** command to display the details of the cluster, including the Kafka version.

```bash
ibmcloud es cluster [--json]
```
{: codeblock}

**Prerequisites**: None

**Command options**:

--json (optional)
:   Output format in JSON.

## Step 3: Create a topic and select number of partitions by using the Admin REST API
{: #create_topic_api}
{: api}

{{site.data.keyword.databases-for-mongodb}} provides a REST API for administration that you can use to create, delete, list, and update topics.

You can create a Kafka topic by issuing a POST request to the `/admin/topics` path. The body of the request must contain a JSON document. For example:

```json
{
    "name": "topicname",
    "partitions": 1,
    "configs": {
        "retentionMs": 86400000,
        "cleanupPolicy": "delete"
    }
}
```
{: codeblock}


The JSON document must contain a `name` attribute, specifying the name of the Kafka topic to create. Topic names are restricted to a maximum of 200 characters.The JSON can also specify the number of partitions to assign to the topic (using the `partitions` property). If the number of partitions is not specified, the topic is created with a single partition.

One or more partitions make up a topic. A partition is an ordered list of messages. 1 partition is sufficient for getting started, but production systems often have more.

Partitions are distributed across the brokers to increase the scalability of your topic. You can also use them to distribute messages across the members of a consumer group.

You can also specify an optional `configs` object within the request. This allows you to specify the `retentionMs` property, which controls how long (in milliseconds) Kafka retains messages published to the topic. After this time elapses the messages are automatically deleted to free space. You must specify the value of the `retentionMs` property in a whole number of hours (for example, multiples of 3600000).

For guidance about the settings that you can modify when creating topics, see [topic configuration](/docs/EventStreams?topic=EventStreams-kafka_java_api{: external}).

The expected HTTP status codes are as follows:

* 202: Topic creation request was accepted.
* 400: Invalid request JSON.
* 403: Not authorized to create topic.
* 422: Semantically invalid request.

If the request to create a Kafka topic succeeds, HTTP status code 202 (Accepted) is returned. If the operation fails, an HTTP status code of 422 (Unprocessable Entity) is returned, and a JSON object containing additional information about the failure is returned as the body of the response.

### Example
{: #create_topic_api_example}

You can exercise the REST endpoint for creating a Kafka topic using the following snippet of curl. You'll need to supply your own API key or token and specify the correct endpoint for ADMIN API.

```sh
curl -i -X POST -H 'Accept: application/json' -H 'Content-Type: application/json' -H 'Authorization: Bearer ${TOKEN}' --data '{ "name": "newtopic", "partitions": 1}' ${ADMIN_URL}/admin/topics
```
{: codeblock}


### Working with topics using the Admin REST API
{: #work_topic_api}
{: api}

After you create topics, you can use the Admin REST API to [list topics](#topic_list_api) and [delete topics](#topic_delete_api) and [update topic configuration](#topic_update_api).

#### List topics
{: #topic_list_api}

You can list all your Kafka topics by issuing a GET request to the
`/admin/topics` path.

The expected status code is:

* 200: The topic list is returned as JSON in the following format:

```json
[
  {
    "name": "topic1",
    "partitions": 1,
    "retentionMs": 86400000,
    "cleanupPolicy": "delete"
  },
  { "name": "topic2",
    "partitions": 2,
    "retentionMs": 86400000,
    "cleanupPolicy": "delete"
  }
]
```
{: codeblock}

A successful response will have HTTP status code 200 (OK) and contain an
array of JSON objects, where each object represents a Kafka topic and has the following properties:

| Property name     | Description                                             |
|-------------------|---------------------------------------------------------|
| name              | The name of the Kafka topic.                            |
| partitions        | The number of partitions assigned to the Kafka topic.   |
| retentionsMs      | The retention period for messages on the topic (in ms). |
| cleanupPolicy     | The cleanup policy of the Kafka topic.        |
{: caption="Table 1. {{site.data.keyword.databases-for-mongodb}} topic properties" caption-side="top"}

##### List topics example
{: #topic_list_example_api}

You can use the following curl command to list all your Kafka topics:

```sh
curl -i -X GET -H 'Accept: application/json' -H 'Authorization: Bearer ${TOKEN}' ${ADMIN_URL}/admin/topics
```
{: codeblock}

#### Delete a topic
{: #topic_delete_api}

To delete a topic, issue a DELETE request to the `/admin/topics/TOPICNAME`path (where TOPICNAME is the name of the Kafka topic that you want to delete).

The expected return codes are as follows:

- 202: Topic deletion request was accepted.
- 403: Not authorized to delete topic.
- 404: Topic does not exist.

A 202 (Accepted) status code is returned if the REST API accepts the delete
request. A status code 422 (Unprocessable Entity) is returned if the delete request is rejected. If a delete request is rejected, the body of the HTTP response contains a JSON object, which
provides additional information about why the request was rejected.

Kafka deletes topics asynchronously. Deleted topics can still appear in the
response to a list topics request for a short period of time after the completion of a REST request to delete the topic.

##### Delete topic example
{: #topic_delete_example_api}

The following curl command deletes a topic called `MYTOPIC`:

```sh
curl -i -H 'Content-Type: application/json' -X DELETE -H 'Authorization: Bearer ${TOKEN}' ${ADMIN_URL}/admin/topics/MYTOPIC
```
{: codeblock}

#### Update a topic's configuration
{: #topic_update_api}

To increase a topic's partition number or to update a topic's configuration, issue a`PATCH` request to `/admin/topics/{topic}` with the following body:

```json
{
  "new_total_partition_count": 4,
  "configs": [
    {
      "name": "cleanup.policy",
      "value": "compact"
    }
  ]
}
```
{: codeblock}

The following configuration keys are supported: 'cleanup.policy', 'retention.ms', 'retention.bytes', 'segment.bytes', 'segment.ms', and 'segment.index.bytes'.

You can only increase the partition number, not decrease it.

The expected status codes are as follows:
* 202: Update topic request was accepted.
* 400: Invalid request JSON or number of partitions is invalid.
* 404: Topic specified does not exist.
* 422: Semantically invalid request.

##### Update topic configuration example
{: #topic_update_example_api}

The following curl command updates a topic called `MYTOPIC`, set its `partitions` to 4 and its `cleanup.policy` to be `compact`.

```sh
curl -i -X PATCH -H 'Content-Type: application/json' -H 'Authorization: Bearer ${TOKEN}' --data '{"new_total_partition_count": 4,"configs":[{"name":"cleanup.policy","value":"compact"}]}' ${ADMIN_URL}/admin/topics/MYTOPIC
```
{: codeblock}


## Step 4: Create a service credential by using the console
{: #create_credential_ui}
{: ui}


To allow you to connect to your {{site.data.keyword.databases-for-mongodb}} instance, create a service key by using the {{site.data.keyword.Bluemix_notm}} console:

1. Locate your {{site.data.keyword.databases-for-mongodb}} service in the **Resource list**.
2. Click your service tile.
3. Click **Service credentials**.
4. Click **New credential**.
5. Complete the details for your new credential like a name and role and click **Add**. A new credential appears in the credentials list.
6. Click the chevron next to the new credential to reveal the details in JSON format.

## Step 4: Create a service credential by using the CLI
{: #create_credential_cli}
{: cli}

Create a service key by using the {{site.data.keyword.Bluemix_notm}} CLI, so that you can connect to your {{site.data.keyword.databases-for-mongodb}} instance:

1. Locate your service:
    ```bash
    ibmcloud resource service-instances
    ```
    {: codeblock}

2. Create a service key:
    ```bash
    ibmcloud resource service-key-create <key_name> <key_role> --instance-name <your_service_name>
    ```
    {: codeblock}

3. Print the service key:
    ```bash
    ibmcloud resource service-key <key_name>
    ```
    {: codeblock}

    A single set of endpoint details are contained in each service key. For service instances configured to be connected to a single network type, either the {{site.data.keyword.Bluemix_notm}} Public network (the default) or the {{site.data.keyword.Bluemix_notm}} Private network, the service key contains the details relevant to that network type. For instances configured to support both the private and public networks, details for the public network are returned. If you want details for the private network, you must add the `--service-endpoint private` parameter to the previous **service-key-create** CLI command. For example:
    {: note}

    ```bash
    ibmcloud resource service-key-create <private-key-name> <role> --instance-name <instance-name> --service-endpoint private
    ```
    {: codeblock}

## Step 4: Create a service credential by using the CLI and the REST producer API
{: #create_credential_api}
{: api}

To connect to your {{site.data.keyword.databases-for-mongodb}} instance, the supported authentication mechanism is using a bearer token. To obtain your token by using the {{site.data.keyword.Bluemix_notm}} CLI, first log in to {{site.data.keyword.Bluemix_notm}} and then run the following command:

```sh
ibmcloud iam oauth-tokens
```
{: codeblock}

Place this token in the Authorization header of the HTTP request in the form `Bearer<token>`. Both API key or JWT tokens are supported.

## Step 5: Produce data using the console
{: #produce_data_ui}
{: ui}

You cannot produce data by using the console. You can produce data using the [CLI](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=cli#produce_data_cli), the [REST Producer API](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=api#produce_data_api), or the [Kafka API](https://kafka.apache.org/documentation/#producerapi).


## Step 5: Produce data using the CLI
{: #produce_data_cli}
{: cli}

You can use the {{site.data.keyword.databases-for-mongodb}} Kafka console producer tool to produce data. The console tools are in the `bin` directory of your Kafka client download, which you can download from [Apache Kafka downloads](http://kafka.apache.org/downloads){: external}.

You must provide a list of brokers (using the BOOTSTRAP_ENDPOINTS property) and SASL credentials. To provide the SASL credentials to this tool, create a properties file based on the following example:

```config
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

Replace USER and PASSWORD with the values from your {{site.data.keyword.databases-for-mongodb}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.

{{site.data.keyword.databases-for-mongodb}} provides example `producer.properties` and `consumer.properties` files for the [Java client](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample/resources){: external}.

After you create the properties file, you can run the console producer in a terminal as follows:

```bash
   kafka-console-producer.sh --broker-list BOOTSTRAP_ENDPOINTS --producer.config CONFIG_FILE --topic TOPIC_NAME
```
{: codeblock}

Replace the following variables in the example with your own values:

- BOOTSTRAP_ENDPOINTS with the value from your {{site.data.keyword.databases-for-mongodb}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.
- CONFIG_FILE with the path of the configuration file.

You can use many of the other options of this tool, except for those that require access to ZooKeeper. For more information, see [Using Kafka console tools with Event Streams](/docs/EventStreams?topic=EventStreams-kafka_console_tools){: external}.



### Producer configuration settings
{: #producer_config_cli}
{: cli}

For details of some of the most important settings that you can configure for the producer, see the following information:

* [General configuration settings](/docs/EventStreams?topic=EventStreams-producing_messages#config_settings){: external}
* [Delivery semantics](/docs/EventStreams?topic=EventStreams-producing_messages#delivery_semantics){: external}


## Step 5: Produce data using the REST Producer API
{: #produce_data_api}
{: api}

Use the v2 endpoint of the REST Producer API to send messages of type `text`, `binary`, `JSON`, or `avro` to topics. With the v2 endpoint you can use the {{site.data.keyword.databases-for-mongodb}} schema registry by specifying the schema for the avro data type.

The following code shows an example of sending a message of `text` type by using curl:

```sh
curl -v -X POST \
-H "Authorization: Bearer $token" -H "Content-Type: application/json" -H "Accept: application/json" \
-d '{
  "headers": [
    {
      "name": "colour",
      "value": "YmxhY2s="
    }
  ],
  "key": {
    "type": "text",
    "data": "Test Key"
  },
  "value": {
    "type": "text",
    "data": "Test Value"
  }
}' \
"$kafka_http_url/v2/topics/$topic_name/records"
```
{: codeblock}

For more information, see the [{{site.data.keyword.databases-for-mongodb}} REST Producer API reference](https://cloud.ibm.com/apidocs/event-streams/restproducer){: external}.


### Producer configuration settings
{: #producer_config_api}
{: api}

For details of some of the most important settings that you can configure for the producer, see the following information:

* [General configuration settings](/docs/EventStreams?topic=EventStreams-producing_messages#config_settings){: external}
* [Delivery semantics](/docs/EventStreams?topic=EventStreams-producing_messages#delivery_semantics){: external}


## Step 6: Consume data using the console
{: #consume_data_ui}
{: ui}

You cannot consume data by using the console. You can consume data using the [CLI](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=cli#consume_data_cli) or the [Kafka API](https://kafka.apache.org/documentation/#consumerapi).

## Step 6: Consume data using the CLI
{: #consume_data_cli}
{: cli}

You can use the {{site.data.keyword.databases-for-mongodb}} Kafka console consumer tool to consume data.

The console tools are in the `bin` directory of your Kafka client download.

You must provide a list of brokers and SASL credentials. After you create the properties file as described in [produce data using the CLI](#produce_data_cli), run the console consumer in a terminal as follows:

```bash
   kafka-console-consumer.sh --bootstrap-server BOOTSTRAP_ENDPOINTS --consumer.config CONFIG_FILE --topic TOPIC_NAME
```
{: codeblock}

Replace the following variables in the example with your own values:

- BOOTSTRAP_ENDPOINTS with the value from your {{site.data.keyword.databases-for-mongodb}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.
- CONFIG_FILE with the path of the configuration file.

You can use many of the other options of this tool, except for those that require access to ZooKeeper. For more information, see [Using Kafka console tools with Event Streams](/docs/EventStreams?topic=EventStreams-kafka_console_tools).

### Consumer configuration settings
{: #consumer_config_cli}
{: cli}

For details of some of the most important settings that you can configure for the consumer, see the following information:

* [General configuration settings](/docs/EventStreams?topic=EventStreams-consuming_messages#configuring_consumer_properties){: external}
* [Consumer liveness](/docs/EventStreams?topic=EventStreams-consuming_messages#consumer_liveness){: external}
* [Consumer groups](/docs/EventStreams?topic=EventStreams-consuming_messages#consumer_groups){: external}
* [Delivery semantics](/docs/EventStreams?topic=EventStreams-producing_messages#delivery_semantics){: external}
* [Managing offsets](/docs/EventStreams?topic=EventStreams-consuming_messages#managing_offsets){: external}
* [Handling consumer rebalancing](/docs/EventStreams?topic=EventStreams-consuming_messages#consumer_rebalancing){: external}


## Step 6: Consume data using an API
{: #consume_data_api}
{: api}

You cannot consume data using an {{site.data.keyword.databases-for-mongodb}} API although consumption of data from Kafka is possible using the native Kafka libraries. For more information, see [Kafka consumer API](https://kafka.apache.org/documentation/#consumerapi).

As an alternative, use the [CLI](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=cli#consume_data_cli).


## Step 7: Connect {{site.data.keyword.mon_full_notm}} for operational visibility by using the console
{: #connect_monitoring_ui}
{: ui}

You can use {{site.data.keyword.mon_full_notm}} to get operational visibility into the performance and health of your applications, services, and platforms. {{site.data.keyword.mon_full_notm}} provides administrators, DevOps teams, and developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.

For more information about how to use {{site.data.keyword.monitoringshort}} with {{site.data.keyword.databases-for-mongodb}}, see:
* [Opting in to metrics](/docs/EventStreams?topic=EventStreams-metrics#opt_in_metrics){: external}
* [Enabling default metrics](/docs/EventStreams?topic=EventStreams-metrics#enabling_default_metrics){: external}
* [Enabling enhanced metrics](/docs/EventStreams?topic=EventStreams-metrics#opt_in_enhanced_metrics){: external}
* [Viewing details of available metrics](/docs/EventStreams?topic=EventStreams-metrics#metric_details){: external}
* [Understanding metrics cost information](/docs/EventStreams?topic=EventStreams-metrics#metric_costs){: external}


## Step 7: Connect {{site.data.keyword.mon_full_notm}} for operational visibility by using the CLI
{: #connect_monitoring_cli}
{: cli}

You cannot connect {{site.data.keyword.mon_full_notm}} by using the CLI. Use the [console](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=ui#connect_monitoring_ui) to complete this task.

## Step 7: Connect {{site.data.keyword.mon_full_notm}} for operational visibility by using the API
{: #connect_monitoring_api}
{: api}

You cannot connect {{site.data.keyword.mon_full_notm}} by using the API. Use the [console](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=ui#connect_monitoring_ui) to complete this task.


## Step 8: Connect {{site.data.keyword.at_full}} to audit service activity
{: #activity_tracker_ui}
{: ui}

{{site.data.keyword.at_full_notm}} allows you to view, manage, and audit service activity to comply with corporate policies and industry regulations. {{site.data.keyword.at_short}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. Use {{site.data.keyword.at_short}} to track how users and applications interact with the {{site.data.keyword.databases-for-mongodb}} service on the Standard and Enterprise plans.

To get up and running with {{site.data.keyword.at_short}}, see [Getting Started with {{site.data.keyword.at_short}}](/docs/activity-tracker?topic=activity-tracker-getting-started#gs_objectives){: external}.

{{site.data.keyword.at_short}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_short}} service in the same location where your service instance is available. For more information, see [Launch the web UI](/docs/activity-tracker?topic=activity-tracker-getting-started#gs_step4){: external}.

For more information about events specific to {{site.data.keyword.databases-for-mongodb}}, see:

* [Where to view events](/docs/EventStreams?topic=EventStreams-at_events#ui){: external}
* [Topic events](/docs/EventStreams?topic=EventStreams-at_events#topic-events){: external}
* [Message audit events](/docs/EventStreams?topic=EventStreams-at_events#message-events){: external}
* [Other events](/docs/EventStreams?topic=EventStreams-at_events#other-events){: external}

Events are formatted according to the Cloud Auditing Data Federation (CADF) standard. For further details of the information they include, see [CADF standard](/docs/activity-tracker?topic=activity-tracker-about#cadf_standard){: external}.


## Step 8: Connect {{site.data.keyword.at_full}} using the CLI to audit service activity
{: #activity_tracker_cli}
{: cli}

You cannot connect {{site.data.keyword.at_short}} using the CLI. Use the [console](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=ui#activity_tracker_ui) to complete this task.

## Step 8: Connect {{site.data.keyword.at_full}} using the API to audit service activity
{: #activity_tracker_api}
{: api}

You cannot connect {{site.data.keyword.at_short}} using the API. Use the [console](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=ui#activity_tracker_ui) to complete this task.


## Step 9: (Optional) Use Kafka Connect or ksqlDB
{: #kafka_connect_ksql}

### Kafka Connect
{: #kafka_connect}

Kafka Connect is part of the Apache Kafka project and allows you to connect external systems to Kafka. It consists of a runtime  that can run connectors to copy data to and from a cluster.

Its key benefits are as follows:

* Scalability: it can easily scale from a single worker to many.
* Reliability: it automatically manages offsets and the lifecycle of connectors
* Extensibility: the community has built connectors for most popular systems. {{site.data.keyword.IBM_notm}} has connectors for [MQ](/docs/EventStreams?topic=EventStreams-mq_connector){: external} and [Cloud Object Storage](/docs/EventStreams?topic=EventStreams-cos_connector){: external}.

For more information, see [Using Kafka Connect with Event Streams](/docs/EventStreams?topic=EventStreams-kafka_connect){: external}.

Kafka Connect is not part of the managed {{site.data.keyword.databases-for-mongodb}} service.

### ksqlDB
{: #ksqldb}

You can use [KSQL](https://github.com/confluentinc/ksql){: external} with the {{site.data.keyword.databases-for-mongodb}} Enterprise plan for stream processing.
{: shortdesc}

ksqlDB is a purpose-built database for event streaming. Use it to build end-to-end event streaming applications quickly with a purpose-built stream processing database for Apache Kafka.

First complete these [setup steps](/docs/EventStreams?topic=EventStreams-ksql_using##kqsldbsteps){: external}. Then the quickest and easiest way to run ksqlDB with {{site.data.keyword.databases-for-mongodb}} is to use a docker container as described in [ksqlDB quickstart](https://ksqldb.io/quickstart.html){: external}.


## Step 10: Get help
{: #getting_help}

For a general overview of how to get help with {{site.data.keyword.databases-for-mongodb}} and where to get support, see [Getting help and support](/docs/EventStreams?topic=EventStreams-gettinghelp){: external}.

[FAQs](/docs/EventStreams?topic=EventStreams-faqs){: external} details answers to some of the common questions about {{site.data.keyword.databases-for-mongodb}}.

If you're experiencing a problem with {{site.data.keyword.databases-for-mongodb}}, here's a list of the information you need to gather before you open a case [Reporting a problem to the Event Streams team - Standard and Enterprise plans](/docs/EventStreams?topic=EventStreams-report_problem_enterprise){: external}.
