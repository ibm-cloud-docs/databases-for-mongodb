---

copyright:
  years: 2019, 2021
lastupdated: "2021-11-30"

keywords: mongodb, databases, scaling, memory, disk IOPS, CPU

subcollection: databases-for-mongodb

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Scaling Disk, Memory, and CPU
{: #resources-scaling}

You can manually adjust the amount of resources available to your {{site.data.keyword.databases-for-mongodb_full}} deployment to suit your workload and the size of your data.

## Resource Breakdown

{{site.data.keyword.databases-for-mongodb_full}} runs with three data members in a cluster, and resources are allocated to all members equally. For example, the minimum disk size of a MongoDB deployment is 30720 MB, which equates to an initial size of 10240 MB per member. Minimum RAM for a MongoDB deployment is 3072 MB, which equates to an initial allocation of 1024 MB per member.

Billing is based on the _total_ amount of resources that are allocated to the service.
{: .tip}

When you [provision](/docs/databases-for-mongodb?topic=cloud-databases-provisioning#provisioning) a deployment, you can select the initial resource allocation of disk and memory. After provision, you can scale your deployment as it needs more resources.

### Disk Usage

Your disk allocation has to be enough to store all of your data. Your data is replicated to both data members so the total amount of disk you use is at least twice the size of your data set. 

Disk allocation also affects the performance of the disk, with larger disks having higher performance. Baseline Input-Output Operations per second (IOPS) performance for disk is 10 IOPS for each GB. Scale disk to increase the IOPS your deployment can handle.
 
You cannot scale down storage. If your data set size has decreased, you can recover space by backing up and restoring to a new deployment.
{: .tip} 

### RAM

Memory resources are used for database operations and also controls the amount of memory that is allocated to the [internal and filesystem cache](/docs/databases-for-mongodb?topic=databases-for-mongodb-high-availability). If your database can serve most of the requests from the cache, then it doesn't have to read from disk and performs better. 

The amount of memory you allocate to your deployment is split between all members. Adding memory to the total allocation adds memory to all members equally.

### Dedicated Cores

You can enable or increase the CPU allocation to the deployment. With dedicated cores, your resource group is given a single-tenant host with a reserve of CPU shares. Your deployment is then guaranteed the minimum number of CPUs you specify. The default of 0 dedicated cores uses compute resources on shared hosts.

## Scaling Considerations

- Scaling your deployment up might cause your databases to restart. If your scaled deployment needs to be moved to a host with more capacity, then the databases are restarted as part of the move.

- Scaling down RAM or CPU does not trigger database node restarts.

- Disk can not be scaled down.

- A few scaling operations can be more long running than others. Enabling dedicated cores moves your deployment to its own host and can take longer than just adding more cores. Similarly, drastically increasing RAM or Disk can take longer than smaller increases to account for provisioning more underlying hardware resources.

- Scaling operations are logged in [{{site.data.keyword.at_full}}](/docs/databases-for-mongodb?topic=cloud-databases-activity-tracker).

- If you find consistent trends in resource usage or would like to set up scaling when certain resource thresholds are reached, checkout enabling [autoscaling](/docs/databases-for-mongodb?topic=databases-for-mongodb-autoscaling) on your deployment.

## Scaling in the UI

A visual representation of your data members and their resource allocation is available on the _Resources_ tab of your deployment's _Manage_ page. 

![The Scale Resources Panel in _Resources_](images/scaling-update.png)

Adjust the slider to increase or decrease the resources that are allocated to your service. The UI currently uses a coarser-grained resolution than is available via the CLI or API. The UI shows the total allocated memory or disk for the position of the slider. Click **Scale** to trigger the scaling operations and return to the dashboard overview. 

## Scaling in the CLI 

[{{site.data.keyword.cloud_notm}} CLI cloud databases plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) supports viewing and scaling the resources on your deployment. Use the command `cdb deployment-groups` to see current resource information for your service, including which resource groups are adjustable. To scale any of the available resource groups, use `cdb deployment-groups-set` command. 

For example, the command to view the resource groups for a deployment named "example-deployment" is  
`ibmcloud cdb deployment-groups example-deployment`

This command produces the output:

```
Group   member
Count   3
|
+   Memory
|   Allocation              6144mb
|   Allocation per member   2048mb
|   Minimum                 6144mb
|   Step Size               256mb
|   Adjustable              true
|
+   CPU
|   Allocation              0
|   Allocation per member   0
|   Minimum                 6
|   Step Size               2
|   Adjustable              true
|
+   Disk
|   Allocation              30720mb
|   Allocation per member   10240mb
|   Minimum                 30720mb
|   Step Size               2048mb
|   Adjustable              true
```

The deployment has three members, with 6144 MB of RAM and 30720 MB of disk allocated in total. The "per member" allocation is 2048 MB of RAM and 10240 MB of disk. The minimum value is the lowest the total allocation that can be set. The step size is the smallest amount by which the total allocation can be adjusted.

The `cdb deployment-groups-set` command allows either the total RAM or total disk allocation to be set, in MB. For example, to scale the memory of the "example-deployment" to 4096 MB of RAM for each memory member (for a total memory of 12288 MB), you use the command:  
`ibmcloud cdb deployment-groups-set example-deployment member --memory 12288`

## Scaling in the API

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically.

To view the current and scalable resources on a deployment,
```
curl -X GET -H "Authorization: Bearer $APIKEY" 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups'
```

To scale the memory of a deployment to 4096 MB of RAM for each memory member (for a total memory of 12288 MB).
```
curl -X PATCH 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups/member' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"memory": {
        "allocation_mb": 12288
      }
    }'
```

More information is in the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl).
