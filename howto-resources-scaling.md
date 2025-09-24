---

copyright:
  years: 2019, 2025
lastupdated: "2025-09-24"

keywords: mongodb, databases, scaling, memory, disk IOPS, CPU

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Scaling disk, memory, and CPU
{: #resources-scaling}

The Shared Compute hosting model supports more fine-grained resource allocations that are not shown in the UI to maintain clarity. For more information, see [Hosting models](/docs/databases-for-mongodb?topic=databases-for-mongodb-hosting-models&interface=cli).
{: ui}

To scale an [Isolated Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=cli#hosting-models-iso-compute-cli) host flavor instance, set the relevant `hostflavor` parameter to the Isolated Compute size you're targeting, such as "b3c.4x16.encrypted". As this includes CPU and RAM allocation selections, do not separately select CPU and RAM.
{: cli}

To scale a [Shared Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=cli#hosting-models-shared-compute-cli) host flavor instance between the minimum CPU value and 2 CPU, set the CPU to 0 and scale the RAM allocation using the following commands in this documentation. The CPU value will scale as a ratio of 1 CPU : 8 GB RAM, up to 2 CPU. To scale above 2 CPU, set the CPU and RAM allocations to your target allocation. For both, make sure to include the relevant `hostflavor` parameter of "multitenant".
{: cli}

To scale an [Isolated Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=api#hosting-models-iso-compute-api) host flavor instance, set the relevant `host_flavor` parameter to the Isolated Compute size you're targeting, such as "b3c.4x16.encrypted". As this includes CPU and RAM allocation selections, do not separately select CPU and RAM.
{: api}

To scale a [Shared Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=api#hosting-models-shared-compute-api) host flavor instance between the minimum CPU value and 2 CPU, set the CPU to 0 and scale the RAM allocation using the following commands. The CPU value will scale as a ratio of 1 CPU : 8 GB RAM, up to 2 CPU. To scale above 2 CPU, set the CPU and RAM allocations to your target allocation. For both, make sure to include the relevant `host_flavor` parameter of "multitenant".
{: api}

To scale an [Isolated Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=terraform#hosting-models-shared-compute-terraform) host flavor instance, set the relevant `host_flavor` parameter to the Isolated Compute size you're targeting, such as "b3c.4x16.encrypted". As this includes CPU and RAM allocation selections, do not separately select CPU and RAM.
{: terraform}

To scale a [Shared Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=terraform#hosting-models-iso-compute-terraform) host flavor instance between the minimum CPU value and 2 CPU, set the CPU to 0 and scale the RAM allocation using the following commands. The CPU value will scale as a ratio of 1 CPU : 8 GB RAM, up to 2 CPU. To scale above 2 CPU, set the CPU and RAM allocations to your target allocation. For both, make sure to include the relevant `host_flavor` parameter of "multitenant".
{: terraform}

You can manually adjust the amount of resources available to your {{site.data.keyword.databases-for-mongodb}} deployment to suit your workload and the size of your data.

## Resource breakdown
{: #resources-breakdown}

{{site.data.keyword.databases-for-mongodb}} runs with three data members in a cluster, and resources are allocated to all members equally. For example, the minimum disk size of a MongoDB deployment is 30720 MB, which equates to an initial size of 10240 MB per member. Minimum RAM for a MongoDB deployment is 12288 MB, which equates to an initial allocation of 4096 MB per member.

Billing is based on the _total_ amount of resources that are allocated to the service.
{: tip}

When you provision a deployment, you can select the initial resource allocation of disk and memory. After provision, you can scale your deployment as it needs more resources.

### Disk usage
{: #resources-scaling-disk-usage}

Your disk allocation must be enough to store all of your data. Your data is replicated to both data members so the total amount of disk that you use is at least twice the size of your data set. 

Disk allocation also affects the performance of the disk, with larger disks having higher performance. Baseline Input-Output Operations per second (IOPS) performance for disk is 10 IOPS for each GB. Scale disk to increase the IOPS that your deployment can handle.
 
You cannot scale down storage. If your data set size has decreased, you can recover space by backing up and restoring to a new deployment.
{: tip} 

### RAM
{: #resources-scaling-ram}

Memory resources are used for database operations and also control the amount of memory that is allocated to the [internal and file system cache](/docs/databases-for-mongodb?topic=databases-for-mongodb-ha-dr). If your database can serve most of the requests from the cache, then it doesn't have to read from disk and performs better. 

The amount of memory you allocate to your deployment is split between all members. Adding memory to the total allocation adds memory to all members equally.

### vCPU
{: #resources-scaling-dedicated-cores}

If you find that your database workloads need more CPU resources, you can scale the amount of CPU allocated to your service. If your database instance is on an Isolated Compute hosting model, select the CPU x RAM configuration that matches your resource needs. If your database instance is on a Shared Compute or Dedicated Core hosting model, select the CPU allocation that you want for your database. 

Old style dedicated core instances are deprecated, and will be removed in May 2025. For more information on the new hosting models, see the [Hosting models overview](/docs/databases-for-mongodb?topic=databases-for-mongodb-hosting-models).

## Scaling considerations
{: #resources-scaling-scale-consider}

- Scaling your deployment up might cause your databases to restart. If your scaled deployment needs to be moved to a host with more capacity, then the databases are restarted as part of the move.

- Scaling down RAM or CPU does not trigger database node restarts.

- Disk cannot be scaled down.

- Scaling between hosting models (Shared Compute, Isolated Compute, and Dedicated Cores) moves your deployment to new hosts. Your databases are restarted as part of that move. As your deployment is moved to a new host, this can also take longer than just adding more resources. For more information, see [Shared Compute and Isolated Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models).

- Similarly, drastically increasing RAM or disk can take longer than smaller increases to account for provisioning more underlying hardware resources.

- Scaling operations are logged in [{{site.data.keyword.atracker_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-at_events).

- If you find consistent trends in resource usage or would like to set up scaling when certain resource thresholds are reached, checkout enabling [autoscaling](/docs/databases-for-mongodb?topic=databases-for-mongodb-autoscaling) on your deployment.

## Review current resources and hosting model
{: #review-resources-ui}
{: ui}

In the **Resources** tab, you find both **Hosting model** and **Resource allocations** tiles. These tiles reflect your current resources and hosting model. Select *Configure* to adjust the settings in each tile. 

## Scaling in the UI
{: #resources-scaling-scale-ui}
{: ui}

In the **Resources** tab of the UI, select *Configure* on the **Resource allocations** tile. This opens up a panel where you can adjust your resources. 

If your database is on the Isolated Compute hosting model, you then see a "Host sizes" table, where you can select the vCPU and RAM configuration per member for your database. 

If you are on the Shared Compute hosting model, you see the Small configuration, providing 0.5 vCPU and 4 GB RAM per member; the Small Custom option; or Custom configuration. Small Custom indicates that your database was scaled with the CLI, API, or Terraform, which provides more fine-grained resource scaling, along with an option for automatically allocated vCPU pro-rated against RAM value. On the UI, you can scale to Small and Custom, but are not be able to scale to the fine-grained values provided by the CLI, API, or Terraform. With Custom, drag the slider or adjust the value in the input box to select your database's per member vCPU and RAM values. 

The "Disk (GB/member)" slider is your disk selection per member. Drag the slider or adjust the number in the input box to change the number of GB disk. Note that Disk is tied to IOPS at 1 GB = 10 IOPS. 

Members is the number of members of your database. For MongoDB, members are set to 3. 

Review your total estimated cost in the calculator on the bottom. Note that if you have grandfathered costs, also known as legacy pricing structure, scaling your database instance removes some or all of your legacy pricing. For more information on grandfathering and when it ends, see [Hosting model transition timeline](/docs/cloud-databases?topic=cloud-databases-hosting-model-transition&interface=ui#hosting-model-transition-timeline-may25). 

After you are done, click *Apply changes* to trigger the scaling operation. 

## Switch to and between hosting models in the UI
{: #resources-switching-ui}
{: ui}

In the **Resources** tab of the UI, select *Configure* on the **Hosting model** tile. This opens up a panel where you can adjust your hosting model selection. 

The first option available is **Select your hosting model**. Here, you can switch to a different hosting model. 

Below, you see the options to also adjust the resources of the new hosting model you've selected. Follow the instructions in the previous section, "Scaling in the UI" to adjust your resources.

Clicking *Apply changes* triggers this scale operation. 

## Review current resources and hosting model 
{: #review-resources-cli}
{: cli}

[{{site.data.keyword.cloud_notm}} CLI cloud databases plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) supports viewing and scaling the resources on your deployment. Use the command `cdb deployment-groups` to see current resource information for your service, including which resource groups are adjustable. To scale any of the available resource groups, use `cdb deployment-groups-set` command. 

For example, with the following command you can view the resource groups for a deployment named "example-deployment". Note that this command will also reveal if your database is a [Shared Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-shared-compute-ui) or [Isolated Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-iso-compute-ui) instance through the `hostflavor` attribute. If the `hostflavor` is null, it is on an old style hosting model.

```sh
ibmcloud cdb deployment-groups <INSTANCE_NAME_OR_CRN>
```
{: pre}

This command produces the output:

```sh
Group   member
Count   3
|
+   Memory
|   Allocation                      6144mb
|   Allocation per member           2048mb
|   Minimum                         6144mb
|   Step Size                       256mb
|   Adjustable                      true
|   Cpu Enforcement Ratio Ceiling   49152mb
|   Cpu Enforcement Ratio           8192mb

|
+   CPU
|   Allocation              0
|   Allocation per member   0
|   Minimum                 6
|   Step Size               2
|   Adjustable              true
|                           
+   HostFlavor    
|   ID            multitenant
|   Name          
|   HostingSize   
|
+   Disk
|   Allocation              30720mb
|   Allocation per member   10240mb
|   Minimum                 30720mb
|   Step Size               2048mb
|   Adjustable              true
```
{: pre}

The deployment has three members, with 6144 MB of RAM and 30720 MB of disk allocated in total. The "per member" allocation is 2048 MB of RAM and 10240 MB of disk. The minimum value is the lowest the total allocation that can be set. The step size is the smallest amount by which the total allocation can be adjusted.

## Resources and scaling in the CLI
{: #resources-scaling-cli}
{: cli}

The `cdb deployment-groups-set` command allows either the total RAM or total disk allocation to be set in MB. For example, to scale the memory of the "example-deployment" to 4096 MB of RAM for each memory member (for a total memory of 12288 MB), you use the command:

```sh
ibmcloud cdb deployment-groups-set <INSTANCE_NAME_OR_CRN> member --memory 12288
```
{: pre}

## Determine the hosting model of your database
{: #resources-hosting-determine-cli}
{: cli}

Use the following command to review the value of the `hostflavor` attribute. This will be null if the database is on a deprecated hosting model (not Shared or Isolated Compute).

```sh
ibmcloud cdb groups <deployment_id> --json
```
{: pre}

## Switching to and between hosting models in the CLI
{: #resources-switching-cli}
{: cli}

If your database is a [Shared Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-shared-compute-ui) instance, you can adjust the memory, CPU, and disk options with the following command. If your database is not on Shared Compute, this command will also move a database from a different hosting model to the Shared Compute hosting model.

```sh
ibmcloud cdb deployment-groups-set <deploymentid> <groupid> [--memory <val>] [--cpu <val>] [--disk <val>] [--hostflavor multitenant]
```
{: pre}

For example, use the following to scale to a Shared Compute instance or scale up your Shared Compute instance:

```sh
ibmcloud cdb deployment-groups-set crn:abc ... xyz:: member  --memory 24576 --cpu 6  --hostflavor multitenant
```
{: pre}

If your database is an [Isolated Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-iso-compute-ui) instance, memory and CPU are adjusted together by selecting the Isolated Compute size (see all sizes in Table 1). Disk is scaled separately. If your database is not on Isolated  Compute, this command  will also move a database from a different hosting model to the Isolated Compute hosting model.

Note that since the host flavor selection includes CPU and RAM sizes (`b3c.4x16.encrypted` is 4 CPU and 16 RAM), this request does not accept both an Isolated size selection and separate CPU and RAM allocation selections.  

```sh
ibmcloud cdb deployment-groups-set <deploymentid> <groupid> [--disk <val>] [--hostflavor <hostflavor>]
```
{: pre}

For example, use the following to scale to an Isolated Compute instance or scale up your Isolated Compute instance:

```sh
ibmcloud cdb deployment-groups-set crn:abc ... xyz:: member  --hostflavor b3c.8x32.encrypted
```
{: pre}

CPU and RAM autoscaling is not supported on {{site.data.keyword.databases-for}} Isolated Compute. Disk autoscaling is available. If you provisioned an isolated instance or switched over from a deployment with autoscaling, monitor your resources using [{{site.data.keyword.monitoringfull}} integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring), which provides metrics for memory, disk space, and disk I/O utilization. To add resources to your instance, manually scale your deployment.
{: note}

### The `hostflavor` parameter
{: #host-flavor-parameter-cli}
{: cli}   

The `hostflavor` parameter defines your compute sizing. To provision a Shared Compute instance, specify `multitenant`. To provision an Isolated Compute instance, input the appropriate value for your desired CPU and RAM configuration. 

| **Host flavor** | **hostflavor value** |
|:-------------------------:|:---------------------:|
| Shared Compute            | `multitenant`    |
| 4 CPU x 16 RAM            | `b3c.4x16.encrypted`    |
| 8 CPU x 32 RAM            | `b3c.8x32.encrypted`    |
| 8 CPU x 64 RAM            | `m3c.8x64.encrypted`    |
| 16 CPU x 64 RAM           | `b3c.16x64.encrypted`   |
| 32 CPU x 128 RAM          | `b3c.32x128.encrypted`  |
| 30 CPU x 240 RAM          | `m3c.30x240.encrypted`  |
{: caption="Host flavor sizing parameter" caption-side="bottom"}

## Review current resources and hosting model
{: #review-resources-api}
{: api}

The _Foundation endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically.

To view the current and scalable resources on a deployment, use the [/deployments/{id}/groups](https://cloud.ibm.com/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl) endpoint. Note that this command will also reveal if your database is a [Shared Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-shared-compute-ui) or [Isolated Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-iso-compute-ui) instance through the `host_flavor` attribute. If the `host_flavor` is null, it is on an old style hosting model.  

```sh
curl -X GET https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups -H 'Authorization: Bearer <>' \
```
{: pre}

## Scaling with the API
{: #resources-scaling-api}
{: api}

To scale the memory of a deployment to 4096 MB of RAM for each memory member (for a total memory of 12288 MB), use the following command:

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/member 
-H 'Authorization: Bearer <>' 
-H 'Content-Type: application/json' 
-d '{"memory": {"allocation_mb": 12288}}' \
```
{: pre}

For more information, see the [API reference](/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeploymentscalinggroups).

## Determine the hosting model of your database
{: #resources-hosting-determine-api}
{: api}

Use the following command to review the value of the `host_flavor` attribute. This will be null if the database is on a deprecated hosting model (not Shared or Isolated Compute).

```sh
curl -X GET https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups 
-H 'Authorization: Bearer <>' \
```
{: pre}

## Switching to and between hosting models in the API
{: #resources-switching-api}
{: api}

To scale any {{site.data.keyword.databases-for}} Shared Compute instance, use the the following command, setting `host_flavor` to `multitenant`. If your database is not on Shared Compute, this command will also move a database from a different hosting model to the Shared Compute hosting model.

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/member
-H 'Authorization: Bearer <>'
-H 'Content-Type: application/json'
-d '{"host_flavor": {"id": "multitenant"},
     "cpu": {"allocation_count": 3},
     "memory": {"allocation_mb": 12288}
    }' \
```
{: pre}

To scale any instance into a {{site.data.keyword.databases-for}} Isolated Compute instance or to scale to a different Isolated Compute size, use the `host_flavor` parameter, this time set to the desired Isolated Compute size. Available hosting sizes and their `host_flavor` value parameters are listed in [Table 1](#host-flavor-parameter-api). For example, `{"host_flavor": "b3c.4x16.encrypted"}`. Note that since the host flavor selection includes CPU and RAM sizes (`b3c.4x16.encrypted` is 4 CPU and 16 RAM), this request does not accept both, an Isolated size selection and separate CPU and RAM allocation selections. Scale with the {{site.data.keyword.databases-for}} [API scaling endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#setdeploymentscalinggroup){: external}, with a command like:

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/member
-H 'Authorization: Bearer <>'
-H 'Content-Type: application/json'
-d '{"host_flavor": {"id": "b3c.4x16.encrypted"}}' \
```
{: pre}

CPU and RAM allocation is not allowed when provisioning or scaling through Isolated Compute. Specify `mulitenant` for the `host_flavor` parameter to have independent CPU and RAM selections.
{: note}

CPU and RAM autoscaling is not supported on {{site.data.keyword.databases-for}} Isolated Compute. Disk autoscaling is available. If you have provisioned an Isolated instance or switched over from a deployment with autoscaling, keep an eye on your resources using [{{site.data.keyword.monitoringfull}} integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring), which provides metrics for memory, disk space, and disk I/O utilization. To add resources to your instance, manually scale your deployment.
{: note}

### The `host flavor` parameter
{: #host-flavor-parameter-api}
{: api}

The `host_flavor` parameter defines your compute sizing. To provision a Shared Compute instance, specify `multitenant`. To provision an Isolated Compute instance, input the appropriate value for your desired CPU and RAM configuration.

| **Host flavor** | **host_flavor value** |
|:-------------------------:|:---------------------:|
| Shared Compute            | `multitenant`    |
| 4 CPU x 16 RAM            | `b3c.4x16.encrypted`    |
| 8 CPU x 32 RAM            | `b3c.8x32.encrypted`    |
| 8 CPU x 64 RAM            | `m3c.8x64.encrypted`    |
| 16 CPU x 64 RAM           | `b3c.16x64.encrypted`   |
| 32 CPU x 128 RAM          | `b3c.32x128.encrypted`  |
| 30 CPU x 240 RAM          | `m3c.30x240.encrypted`  |
{: caption="Table 1 Host flavor sizing parameter" caption-side="bottom"}

## Review current resources and hosting model
{: #review-resources-terraform}
{: terraform}

Review resource allocations to your database by checking your terraform scripts for `cpu { allocation_count = }`, `memory {allocation_mb = }`, and `disk { allocation_mb = }`. Review the `host_flavor` setting to determine if your database is a [Shared Compute](https://cloud.ibm.com/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-shared-compute-ui) or [Isolated Compute](https://cloud.ibm.com/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-iso-compute-ui) style hosting model. If `host_flavor` does not exist, your database is on an old style hosting model. 


## Scaling with Terraform
{: #resources-scaling-terraform}
{: terraform}

Before executing a Terraform script on an existing instance, use the `terraform plan` command to compare the current infrastructure state with the desired state defined in your Terraform files. Any alteration to the `resource_group_id`, `service plan`, `version`, `key_protect_instance`, `key_protect_key`, `backup_encryption_key_crn` attributes recreates your instance. For a list of current argument references with the `Forces new resource` specification, see the [ibm_database Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.
{: important}

Scale your instance by adjusting your Terraform script for the resource you're interested in. In the following example, `cpu`, `memory`, and `disk` allocations are specified. Note that if you have a host flavor selected (Isolated Compute or Shared Compute Multitenant), keep the host flavor selection in your script. 

To implement your change, run `terraform apply`.

```terraform
data "ibm_resource_group" "group" {
  name = "<your_group>"
}
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "standard"
  location          = "eu-gb"
  service           = "databases-for-mongodb"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]
  adminpassword     = "password12"
  group {
    group_id = "member"
    cpu {
      allocation_count = 6
    }
    memory {
      allocation_mb = 24576
    }
    disk {
      allocation_mb = 256000
    }
  }
  users {
    name     = "user123"
    password = "password12"
  }
  allowlist {
    address     = "172.168.1.1/32"
    description = "desc"
  }
}
output "ICD MongoDB database connection string" {
  value = "http://${ibm_database.test_acc.ibm_database_connection.icd_conn}"
}
```
{: codeblock}

## Switching to and Scaling hosting models in Terraform
{: #resources-switching-terraform}
{: terraform}

Select the [hosting model](/docs/databases-for-mongodb?topic=databases-for-mongodb-hosting-models&interface=terraform) you want your database to be scaled to. You can change this later.

To scale your {{site.data.keyword.databases-for-mongodb}} instance to the Shared Compute hosting flavor, set the `"host_flavor"` parameter to `multitenant`. This works if you want to scale to the Shared Compute hosting flavor, or if you want to keep the host flavor and scale your resources. To implement your change, run `terraform apply`. 

See the following example:

```terraform
data "ibm_resource_group" "group" {
  name = "<your_group>"
}
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "standard"
  location          = "eu-gb"
  service           = "databases-for-mongodb"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]
  adminpassword     = "password12"
  group {
    group_id = "member"
    host_flavor {
      id = "multitenant"
    },
    cpu {
      allocation_count = 6
    }
    memory {
      allocation_mb = 24576
    }
    disk {
      allocation_mb = 256000
    }
  }
  users {
    name     = "user123"
    password = "password12"
  }
  allowlist {
    address     = "172.168.1.1/32"
    description = "desc"
  }
}
output "ICD MongoDB database connection string" {
  value = "http://${ibm_database.test_acc.ibm_database_connection.icd_conn}"
}
```
{: codeblock}

Scale your {{site.data.keyword.databases-for-mongodb}} instance to Isolated Compute with the same `"host_flavor"` parameter, set to the desired Isolated size. This command works to scale your database instance to a different Isolated Compute size, as well as to move from another host flavor to the Isolated Compute host flavor. Available hosting sizes and their `host_flavor value` parameters are listed in [Table 1](#host-flavor-parameter-terraform). For example, `{"host_flavor": "b3c.4x16.encrypted"}`. Note that since the host flavor selection includes CPU and RAM sizes (`b3c.4x16.encrypted` is 4 CPU and 16 RAM), this request does not accept both an Isolated size selection and separate CPU and RAM allocation selections.

To implement your change, run `terraform apply`.

```terraform
data "ibm_resource_group" "group" {
  name = "<your_group>"
}
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "standard"
  location          = "eu-gb"
  service           = "databases-for-mongodb"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]
  adminpassword     = "password12"
  group {
    group_id = "member"
    host_flavor {
      id = "b3c.8x32.encrypted"
    }
    disk {
      allocation_mb = 256000
    }
  }
  users {
    name     = "user123"
    password = "password12"
  }
  allowlist {
    address     = "172.168.1.1/32"
    description = "desc"
  }
}
output "ICD MongoDB database connection string" {
  value = "http://${ibm_database.test_acc.ibm_database_connection.icd_conn}"
}
```
{: codeblock}
