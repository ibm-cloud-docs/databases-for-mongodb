---

copyright:
  years: 2019, 2023
lastupdated: "2023-06-26"

keywords: provision cloud databases, terraform, provisioning parameters, cli, resource controller api, provision mongodb, provision mongodb enterprise, provision mongodb ee

subcollection: cloud-databases

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning
{: #provisioning-new}

Provision a {{site.data.keyword.databases-for-mongodb_full}} deployment through the [catalog page](https://cloud.ibm.com/catalog/services/databases-for-mongodb){: external}, the [{{site.data.keyword.databases-for}} CLI](/docs/databases-for-mongodb?topic=databases-for-mongodb-cdb-reference){: external}, the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5){: external}, or through [Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.

## Provisioning through the catalog
{: #catalog}
{: ui}

Deploy from the catalog by specifying the following parameters:

- **Service name** - The name can be any string and is the name that is used on the web and in the CLI to identify the new deployment.
- **Location** - The deployment's public cloud region or Satellite location.
- **Database Version** [Set only at deployment]{: tag-red} - The deployment version of your database. To ensure optimal performance, run the preferred version. The latest minor version is used automatically. For more information, see [Database Versioning Policy](/docs/databases-for-mongodb?topic=cloud-databases-versioning-policy){: external}.
- **Resource allocation** - Specify initial RAM, disk, and cores for your databases. The minimum sizes of memory and disk are selected by default. With dedicated cores, your resource group is given a single-tenant host with a minimum reserve of CPU shares. Your deployments are then allocated the number of cores you specify. *Once provisioned, disk cannot be scaled down.*
- **Endpoints** [Set only at deployment]{: tag-red} - Configure the [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) on your deployment. *A MongoDB deployment cannot have both public and private endpoints simultaneously*.
- **The resource group** - If you are organizing your services into [resource groups](/docs/account?topic=account-account_setup), specify the resource group in this field. Otherwise, you can leave it at default. For more information, see [Managing resource groups](/docs/account?topic=account-rgs).
- **Encryption** - If you use [Key Protect](/docs/cloud-databases?topic=cloud-databases-key-protect&interface=ui), an instance and key can be selected to encrypt the deployment's disk. If you do not use your own key, the deployment automatically creates and manages its own disk encryption key.

After you select the appropriate settings, click **Create** to start the provisioning process.

## Provisioning through the CLI
{: #use-cli}
{: cli}

Follow the instructions provided in the documentation to install the [{{site.data.keyword.cloud_notm}} CLI tool](https://www.ibm.com/cloud/cli){: external}.

1. Log in to {{site.data.keyword.cloud_notm}}. If you use a federated user ID, it's important that you switch to a one-time passcode (`ibmcloud login --sso`), or use an API key (`ibmcloud --apikey key or @key_file`) to authenticate. For more information about how to log in by using the CLI, see [General CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login){: external} under `ibmcloud login`.

   ```sh
   ibmcloud login
   ```
   {: pre}

1. Create an instance.

   ```sh
   ibmcloud resource service-instance-create <INSTANCE_NAME> databases-for-mongodb <PLAN_ID> <LOCATION> <SERVICE_ENDPOINTS_TYPE>
   ```
   {: pre}

   You will see a response like: 
   
   ```sh
   Creating service instance INSTANCE_NAME in resource group default of account USER...
   OK
   Service instance INSTANCE_NAME was created.
                        
   Name:                INSTANCE_NAME
   ID:                  crn:v1:bluemix:public:databases-for-mongodb:us-east:a/40ddc34a846383BGB5b60e:dd13152c-fe15-4bb6-af94-fde0af5303f4::
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

   The fields in the command are described in the table that follows.

   | Field | FlagDescription |
   |-------|------------|
   | `INSTANCE_NAME` | The name can be any string and is the name that is used on the web and in the CLI to identify the new deployment. |
   | `SERVICE_NAME` | `databases-for-mongodb` |
   | `SERVICE_PLAN_NAME` |  Standard plan (`standard`) or Enterprise plan (`enterprise`)|
   | `LOCATION` | The location where you want to deploy. To retrieve a list of regions, use the `ibmcloud regions` command. |
   | `SERVICE_ENDPOINTS_TYPE` | Configure the [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) of your deployment, either `public` or `private`. *A MongoDB deployment cannot have both public and private endpoints simultaneously*. If the `-p` flag is omitted, the default is `public`. |
   {: caption="Table 1. Basic command format fields" caption-side="top"}

   To see a list of resource groups, run the following command:

   ```sh
   ibmcloud resource groups
   ```
   {: pre}

   For more information, see [Managing resource groups](/docs/account?topic=account-rgs).

1. To check provisioning status, use the following command:

   ```sh
   ibmcloud resource service-instance <"INSTANCE_NAME">
   ```
   {: .pre}

   When provisioning is complete, you will see a response like: 
   
   ```sh
   ibmcloud resource service-instance INSTANCE_NAME                                              
   Retrieving service instance INSTANCE_NAME in resource group default under account USER's Account as USER...
   OK
                          
   Name:                  INSTANCE_NAME
   ID:                    crn:v1:bluemix:public:databases-for-mongodb:us-east:a/40ddc34a953a8c02f109835656860e:dd13152c-fe15-4bb6-af94-fde0af5303f4::
   GUID:                  dd13152c-fe15-4bb6-af94-fde5654765
   Location:              LOCATION
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

1. Create credentials for the instance.

   ```sh
   ibmcloud resource service-key-create <SERVICE_KEY_NAME> Manager --instance-name <"instance_name">
   ```
   {: pre}

1. Retrieve the connection details.

   ```sh
   ibmcloud resource service-key <SERVICE_KEY_NAME>
   ```
   {: pre}

After you run this code, the connection details will be exported as environment variables, allowing you to use them in your applications or scripts for connecting to the MongoDB instance.

### More flags and parameters
{: #flags-params}
{: cli}

The `--service-endpoints` flag enables connections to your deployments from the public internet and over the {{site.data.keyword.cloud_notm}} Private network using [Service Endpoints](/docs/services/cloud-databases?topic=cloud-databases-service-endpoints). By default, connections to your deployment can be made from the public network. Possible values are `public`, `private`, `public-and-private`. 

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
