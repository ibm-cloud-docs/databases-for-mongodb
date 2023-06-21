---

copyright:
  years: 2019, 2023
lastupdated: "2023-06-21"

keywords: provision cloud databases, terraform, provisioning parameters, cli, resource controller api, provision mongodb, provision mongodb enterprise, provision mongodb ee

subcollection: cloud-databases

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning
{: #provisioning}

Provision a {{site.data.keyword.databases-for-mongodb_full}} deployment through the [catalog page]((https://cloud.ibm.com/catalog/services/databases-for-mongodb)){: external}, the [{{site.data.keyword.databases-for}} CLI](/docs/databases-for-mongodb?topic=databases-for-mongodb-cdb-reference){: external}, the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5){: external}, or through [Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.

## Provisioning through the catalog
{: #catalog}
{: ui}

Deploy from the catalog by specifying the following parameters:

- [Required]{: tag-red} **The service name** - The name can be any string and is the name that is used on the web and in the CLI to identify the new deployment.
- [Required]{: tag-red} **Location** - The deployment's public cloud region or Satellite location.
- [Required]{: tag-red} **Database Version** - The deployment version of your database. To ensure optimal performance, run the preferred version. The latest minor version is used automatically.
- [Optional]{: tag-purple} **The resource group** - If you are organizing your services into [resource groups](/docs/account?topic=account-account_setup), specify the resource group in this field. Otherwise, you can leave it at default.
- [Optional]{: tag-purple} **Key Protect instance and disk encryption key** - If you use Key Protect, an instance and key can be selected to encrypt the deployment's disk. If you do not use your own key, the deployment automatically creates and manages its own disk encryption key.
- [Optional]{: tag-purple} **Initial resource allocation** - Specify initial memory and disk sizes for your databases. The minimum sizes of memory and disk are selected by default. 
- [Optional]{: tag-purple} **CPU allocation** - Choose dedicated compute resources for your deployment. With dedicated cores, your resource group is given a single-tenant host with a minimum reserve of cpu shares. Your deployments are then allocated the number of CPUs you specify. If not specified in the provisioning request by using the API or CLI, the default minimum is used.
- [Optional]{: tag-purple} **Endpoints** - Configure the [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) on your deployment. The default is that connections to your deployment can be made from the public network.

After you select the appropriate settings, click **Create** to start the provisioning process. 

## Provisioning through the CLI
{: #use-cli}
{: cli}

Use the [{{site.data.keyword.cloud_notm}} CLI tool](https://www.ibm.com/cloud/cli){: external} to communicate with {{site.data.keyword.cloud_notm}} from your CLI.

To create a {{site.data.keyword.databases-for}} deployment, use the {{site.data.keyword.cloud_notm}} CLI tool to request a service instance with the {{site.data.keyword.databases-for-mongodb}} service ID.

The command looks like:

```sh
ibmcloud resource service-instance-create <service-name> databases-for-mongodb
 <service-plan-id> <region> --service-endpoints <SERVICE_ENDPOINTS_TYPE>
```
{: .pre}

To check provisioning status, use the following command that reports the current state of the service instance.

```sh
ibmcloud resource service-instance <service-name>
```
{: .pre}

For more information, see [Working with resources and resource groups](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_instances){: external}.

### More flags and parameters
{: #flags-params}
{: cli}

The `--service-endpoints` flag enables connections to your deployments from the public internet and over the {{site.data.keyword.cloud_notm}} Private network using [Service Endpoints](/docs/services/cloud-databases?topic=cloud-databases-service-endpoints). By default, connections to your deployment can be made from the public network. Possible values are `public`, `private`, `public-and-private`. If the flag is omitted, the default is public endpoints.

The command looks like:

```sh
ibmcloud resource service-instance-create <service-name> --service-endpoints <endpoint-type>
```
{: .pre}

The `service-instance-create` command supports a `-p` flag, which allows JSON-formatted parameters to be passed to the provisioning process. Some parameter values are Cloud Resource Names (CRNs), which uniquely identify a resource in the cloud. All parameter names and values are passed as strings.

For example, if a database is being provisioned from a particular backup and the new database deployment needs a total of 9 GB of memory across three members, then the command to provision 3 GB per member looks like:
```sh
ibmcloud resource service-instance-create databases-for-mongodb <service-name> standard us-south \
-p \ '{
  "backup_id": "crn:v1:blue:public:databases-for-mysql:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "members_memory_allocation_mb": "3072"
}'
```
{: .pre}

## Provisioning through the Resource Controller API
{: #provision-controller-api}
{: api}

Provision new deployments with the [Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller){: external}. To use the Resource Controller API, you need some additional preparation.

1. [Obtain an IAM token from your API token](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#authentication){: external}.
1. You need to know the ID of the resource group that you would like to deploy to. This information is available through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_groups).

   Use a command like: 
   ```sh
   ibmcloud resource groups
   ```
   {: pre}

1. You need to know the region to which you would like to deploy.

   To list all of the regions that deployments can be provisioned into from the current region, use the {{site.data.keyword.databases-for}} CLI plug-in. 
   
   The command looks like: 

   ```sh
   ibmcloud cdb regions --json
   ```
   {: pre}


Once you have all the information, [Create (provision) a new resource instance](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance){: external} with the {{site.data.keyword.cloud_notm}} Resource Controller.

```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "blue-us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-mongodb-standard"
  }'
```
{: .pre}

The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required.
{: required}

## Provisioning with Terraform
{: #provisioning-terraform}
{: terraform}

If you use Terraform to manage your infrastructure, the [{{site.data.keyword.cloud_notm}} provider for Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started) supports provisioning {{site.data.keyword.databases-for}} deployments. Find a sample Terraform configuration file at [{{site.data.keyword.databases-for}} Resources](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: .external}.

## List of Additional Parameters
{: #provisioning-parameters-api}
{: api}

* `backup_id`- A CRN of a backup resource to restore from. The backup must be created by a database deployment with the same service ID. The backup is loaded after provisioning and the new deployment starts up that uses that data. A backup CRN is in the format `crn:v1:<...>:backup:<uuid>`. If omitted, the database is provisioned empty.
* `version` - The version of the database to be provisioned. If omitted, the database is created with the most recent major and minor version.
* `disk_encryption_key_crn` - The CRN of a KMS key (for example, [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) or [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about)), which is then used for disk encryption. A KMS key CRN is in the format `crn:v1:<...>:key:<id>`.
* `backup_encryption_key_crn` - The CRN of a KMS key (for example, [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) or [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about)), which is then used for backup encryption. A KMS key CRN is in the format `crn:v1:<...>:key:<id>`. 
   
   To use a key for your backups, you must first [enable the service-to-service delegation](/docs/cloud-databases?topic=cloud-databases-key-protect#byok-for-backups).
   {: note}

* `members_memory_allocation_mb` -  Total amount of memory to be shared between the database members within the database. For example, if the value is "6144", and there are three database members, then the deployment gets 6 GB of RAM total, giving 2 GB of RAM per member. If omitted, the default value is used for the database type is used.
* `members_disk_allocation_mb` - Total amount of disk to be shared between the database members within the database. For example, if the value is "30720", and there are three members, then the deployment gets 30 GB of disk total, giving 10 GB of disk per member. If omitted, the default value for the database type is used.
* `members_cpu_allocation_count` - Enables and allocates the number of specified dedicated cores to your deployment. For example, to use two dedicated cores per member, use `"members_cpu_allocation_count":"2"`. If omitted, the default value "Shared CPU" uses compute resources on shared hosts.
* `service-endpoints` - Selects the types [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) supported on your deployment. Options are `public`, `private`, or `public-and-private`. If omitted, the default is `public`. 
   
   In the CLI, `service-endpoints` is a flag, not a parameter.
   {: note}
