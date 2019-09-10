---
copyright:
  years: 2019
lastupdated: "2019-05-01"

keyowrds: maongodb, databases, pricing

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Pricing
{: #pricing}

An {{site.data.keyword.databases-for-mongodb_full}} Standard plan deploys as one highly available MongoDB cluster with two data members. Your data is replicated on both members. The Standard plan is priced based on the total amount of disk storage, RAM, and backup storage that is allocated to deployments, prorated hourly. Databases for MongoDB deployments have a minimum of 10 GB of disk and 1 GB of RAM per data member.

## Cost Breakdown

**Disk storage per data member** - gigabytes of disk that are allocated to a {{site.data.keyword.databases-for-mongodb}} data member, or the size of your data.  
**RAM per data member** - gigabytes of RAM that are allocated to a {{site.data.keyword.databases-for-mongodb}} data member.  
**Backup storage** - amount of storage used for backups by a {{site.data.keyword.databases-for-mongodb}} deployment.

Resources | Breakdown | Price
-------|-------|-------
10 GB-Month disk | 2 members x 10 GB x $0.58 | $11.60
1 GB-Month RAM | 2 members x 1 GB  x $8.50 | $17
{: caption="Table 1. Pricing example for two data members" caption-side="top"}

Total per month = $28.60/Month  
Total per hour = $.04/Hour  

All prices here are in US dollars. To see pricing in your local currency, you can to use the pricing calculator.
{: .tip}

## Using the Pricing Calculator

For pricing estimation, use the **Add to Estimate** button at the bottom of the [{{site.data.keyword.databases-for-mongodb}} catalog page](https://cloud.ibm.com/catalog/services/databases-for-mongodb). Input your total consumption across two data members into the calculator. This is roughly double the size of your data because your data is replicated to both members. For example, 10 GB of disk and 1 GB of RAM across two data members would be priced at 20 GB of disk and 2 GB of RAM respectively.

![Pricing calculator estimation with 10 GB of disk and 1 GB of RAM, per member](images/pricing-calc.png)

## Backups Pricing

Users also receive their total disk space purchased, per database, in free backup storage. For example, in a month, if you have a {{site.data.keyword.databases-for-mongodb}} deployment that has 10 GB of disk per member, and two data members, you receive 20 GB of backup storage free for that month. If your backup storage utilization is greater than 20 GB for the month in this scenario, each gigabyte is charged at an overage $0.03/month. Most deployments will not ever go over the allotted credit.

## Dedicated Cores Pricing

When you provision, you have the option of selecting the initial CPU allocation for your deployment. With dedicated cores, your resource group is given a single-tenant host with a guaranteed minimum reserve of cpu shares. Your deployments are then allocated the number of CPUs you specify. The cost of dedicated cores is $45 per core per month, and each member gets the selected number of cores. For example, if you provision a deployment with 3 dedicated cores per member, that is a total of 6 cores, and billed at $270 per month. 

Dedicated cores are an optional feature. The default `Shared CPU` setting provisions your deployment on hosts with shared compute resources and incurs no additional charge.

## Scaling per Member

{{site.data.keyword.databases-for-mongodb}} deployments have minimum and maximum allocation for disk and RAM as shown. Scaling deployments through the API/CLI provides more granularity and also allows a user to scale a database instance up to 4 TB of disk per member.
- Disk minimum - 10 GB per member
- Disk maximum - 4 TB per member
- Disk step granularity through API/CLI - 2048 MB total, 1024 MB per member
- RAM minimum - 1 GB per member
- RAM maximum - 112 GB per member
- RAM step granularity through API/CLI - 256 MB total, 128 MB per member