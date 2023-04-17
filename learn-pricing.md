---
copyright:
  years: 2019, 2023
lastupdated: "2023-04-17"

keyowrds: mongodb, databases, pricing, scaling, resources

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Pricing
{: #pricing}

An {{site.data.keyword.databases-for-mongodb_full}} Standard plan deploys as one highly available MongoDB cluster with three data members. Your data is replicated on all members.The {{site.data.keyword.databases-for-mongodb}} Enterprise plan is also deployed as one highly available MongoDB cluster but with three data members.

The Standard plan is priced based on the total amount of disk storage, RAM, and backup storage that is allocated to deployments, prorated hourly. {{site.data.keyword.databases-for-mongodb}} deployments have a minimum of 10 GB of disk and 1 GB of RAM per data member.

 The Enterprise plan pricing is based on the total amount of disk storage, RAM, and backup storage that is allocated to deployments, prorated hourly. {{site.data.keyword.databases-for-mongodb}} deployments have a minimum of 20 GB of disk, 6 cores, and 14 GB of RAM per data member.

 An {{site.data.keyword.databases-for-mongodb}} EE Analytics node is priced as a data member.

## Using the Pricing Calculator
{: #mongodb-price-calc}

Templates are provided for ease of use and provide balanced resource allocations appropriate for general purpose workloads. The **Custom** tab can be used to configure Disk, RAM, and vCPU, as desired.

For pricing estimation, use the **Add to Estimate** button at the bottom of the [{{site.data.keyword.databases-for-mongodb}} catalog page](https://cloud.ibm.com/catalog/databases-for-mongodb). Input your total consumption across two data members into the calculator. This is roughly double the size of your data because your data is replicated to both members. For example, 10 GB of disk and 1 GB of RAM across two data members would be priced at 20 GB of disk and 2 GB of RAM respectively.

## Backups Pricing
{: #pricing-backup}

You receive your total disk space purchased, per database, in free backup storage. For example, in a given month, if you have a {{site.data.keyword.databases-for-mongodb}} deployment that has 20 GB of disk per member, and has three data members, you receive 60 GB of backup storage free for that month. If your backup storage utilization is greater than 60 GB for the month (in this scenario), you are charged an overage of $0.03/month per gigabyte. 

By default, {{site.data.keyword.databases-for}} provides a daily backup that is stored for 30 days. These backups, and any on-demand backups you make, all count toward the above allocation.

In the above example, if your database contains 2 GB of data and you have not taken any on-demand backups, then your total backup size is 2 GB x 30 = 60 GB. Your backup costs are nil.

If your database contains 15 GB of data and you have not taken any on-demand backups, then your total backup size is 15 GB x 30 = 450 GB. In this scenario, your backup costs are (450 GB - 60 GB) * 0.03 = $11.7 per month.

Most deployments will not ever go over the allotted credit.

## Dedicated Cores Pricing
{: #mongodb-core-pricing}

You have the option of selecting the CPU allocation for your for {{site.data.keyword.databases-for-mongodb}} Standard Edition deployment, while dedicated cores are required for for {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployments. With dedicated cores, your resource group is given a single-tenant host with a guaranteed minimum reserve of cpu shares. Your deployments are then allocated the number of CPUs you specify. The cost of dedicated cores is $45 per core per month for the Standard plan, and $60 per core per month for the Enterprise plan. Each member gets the selected number of cores. For example, if you provision a deployment with 3 dedicated cores per member on the Standard plan, that is a total of 9 cores, and billed at $405 per month. 

The default `Shared CPU` setting provisions your for {{site.data.keyword.databases-for-mongodb}} Standard Edition deployment on hosts with shared compute resources and incurs no additional charge.

Dedicated cores are an optional feature for {{site.data.keyword.databases-for-mongodb}} Standard Edition and required for {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployments. 
{: .note}

## Scaling per Member
{: #mongodb-scale-member}

{{site.data.keyword.databases-for-mongodb}} deployments have minimum and maximum allocation for disk and RAM as shown. Scaling deployments through the API/CLI provides more granularity and also allows a user to scale a database instance up to 4 TB of disk per member.

### {{site.data.keyword.databases-for-mongodb}} Two-member Standard Plan:
{: #mongodb-standard-plan-two-member}

| Resource | Minimum | Maximum | Scaling Granularity (API/CLI) |
| ---------- | ----- | ----- | ------- |
| Disk | 5 GB per member | 4 TB per member | 1024 MB per member |
| RAM | 1 GB per member | 112 GB per member | 128 MB per member |
| CPU (if enabled) | 3 CPUs per member | 28 CPUs per member| 1 CPU per member |
{: caption="Table 1. Per Member Standard Scaling Limits" caption-side="top"}

### {{site.data.keyword.databases-for-mongodb}} Three-member Enterprise Plan:
{: #mongodb-enterp-plan-three-member}

| Resource | Minimum | Maximum | Scaling Granularity (API/CLI) |
| ---------- | ----- | ----- | ------- |
| Disk | 20 GB per member | 4 TB per member | 1024 MB per member |
| RAM | 14 GB per member | 112 GB per member | 128 MB per member |
| CPU | 6 CPUs per member | 28 CPUs per member| 1 CPU per member |
{: caption="Table 2. Per Member Enterprise Scaling Limits" caption-side="top"}


