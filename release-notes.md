---

copyright:
  years: 2019, 2023
lastupdated: "2023-05-23"

keywords: databases-for-mongodb release notes

subcollection: databases-for-mongodb

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for {{site.data.keyword.databases-for-mongodb_full}}
{: #mongodb-relnotes}

Use these release notes to learn about the latest updates to {{site.data.keyword.databases-for-mongodb_full}} that are grouped by _date_ or _build number_.
{: shortdesc}

## 22 May 2023
{: #databases-for-mongodb-18may2023}
{: release-note}

Setting up disk alerts for disk utilization tutorial
:  In this tutorial, you use the {{site.data.keyword.cloud_notm}} API and the [{{site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started){: external} to set up an alert that emails you whenever the disk utilization of your database exceeds 90%. This specific example creates an alert on a {{site.data.keyword.databases-for-elasticsearch}} deployment, but it is applicable to all the databases in the IBM {{site.data.keyword.databases-for}} catalog. For more information, see [Setting up disk alerts for disk utilization](/databases-for-mongodb?topic=databases-for-mongodb-disk-util-alert-tutorial).

## 12 December 2022
{: #databases-for-mongodb-12dec2022}
{: release-note}

Point-in-time Recovery 
:  {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition offers PITR using any timestamp greater than the earliest available recovery point. To discover the earliest recovery point through the API, use the [point-in-time-recovery timestamp endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#capability). For more information, see [Point-in-time Recovery (PITR)](https://test.cloud.ibm.com/docs/databases-for-mongodb?topic=databases-for-mongodb-pitr&interface=ui).

## 19 October 2022
{: #databases-for-mongodb-19oct2022}
{: release-note}

Deploying and Connecting a Cloud Databases Instance Tutorial
:  This tutorial guides you through the process of deploying a {{site.data.keyword.databases-for}} instance and connecting it to a web front end by creating a webpage that allows visitors to input a word and its definition. These values are then stored in a database running on {{site.data.keyword.databases-for}}. You install the database infrastructure by using Terraform and your web application uses the popular Express framework. The application can then be run locally, or by using Docker. For more information, see [Deploying and Connecting a {{site.data.keyword.databases-for}} Instance](/docs/databases-for-mongodb?topic=cloud-databases-create-instance-tutorial).

## 11 October 2022
{: #databases-for-mongodb-11oct2022}
{: release-note}

Protecting {{site.data.keyword.databases-for-mongodb_full}} resources with context-based restrictions
:  Context-based restrictions (CBR) give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud}} resources based on the context of access requests. Access to {{site.data.keyword.databases-for}} resources can be controlled with CBR and identity and access management (IAM) policies. For more information, see [Protecting Cloud Databases resources with context-based restrictions](/docs/databases-for-mongodb?topic=cloud-databases-cbr&interface=ui).

## 22 June 2022
{: #databases-for-mongodb-22june2022}
{: release-note}

Mapping Global COVID-19 cases with the {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Analytics Add-On and Tableau tutorial published
:  The {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Analytics Add-On allows you to run long-running analytical queries or provision a [MongoDB Connector for business intelligence(BI)](https://docs.mongodb.com/bi-connector/current/){: .external} to make your query data compatible with BI tools, such as [Tableau](https://www.tableau.com/){: .external}. For more information, see [{{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Analytics Add-On](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics). This tutorial familiarizes you with the Analytics Add-On using Tableau to visualize data in your MongoDB instance. For more information, see [Mapping Global COVID-19 cases with the Databases for MongoDB EE (Enterprise Edition) Analytics Add-On and Tableau](/docs/databases-for-mongodb?topic=cloud-databases-bi-connector-tutorial-description).

## 31 May 2022
{: #databases-for-mongodb-31may2022}
{: release-note}

Provision a {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition instance with Terraform tutorial published
:  In this tutorial, you learn how to use Terraform to provision a Databases for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition instance that includes the {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition Analytics Add-On. For more information, see [Provision a Databases for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition instance with Terraform](/docs/databases-for-postgresql?topic=cloud-databases-tutorial-provision-mongodbee-tf).

## 26 April 2022
{: #databases-for-mongodb-26apr02022}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition Analytics Add-On Release
:  The {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition Analytics Add-On allows you to execute long-running analytical queries and/or provision a [MongoDB Connector for business intelligence(BI)](https://docs.mongodb.com/bi-connector/current/) to make your query data compatible with BI tools, such as [Tableau](https://www.tableau.com/). For more information, see the documentation [here](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics).

## 17 September 2021
{: #databases-for-mongodb-17sep02021}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}} 4.0 End of Life in April, 2022
:  On April 27, 2022, all {{site.data.keyword.databases-for-mongodb_full}} instances on version 4.0 that are still active will be upgraded in-place to the next major version. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/databases-for-mongodb-40-end-of-life-in-april-2022).

## 1 December 2020
{: #databases-for-mongodb-01dec2020}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}} 3.6 End of Life in April, 2021
:  On April 27, 2021, all {{site.data.keyword.databases-for-mongodb_full}} instances on version 3.6 that are still active will be upgraded in-place to the next major version. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/databases-for-mongodb-36-end-of-life-in-april-2021).

## 31 August 2020
{: #databases-for-mongodb-31aug2020}
{: release-note}

General Availability of {{site.data.keyword.databases-for-mongodb_full}} Enterprise
:  Introducing IBM Cloud Databases for MongoDB Enterprise. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/powering-up-databases-for-mongodb).

## 26 May 2020
{: #databases-for-mongodb-26may2020}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}}'s Improved High Availability Architecture
:  At IBM Cloud Data Services, we are tirelessly investigating improvements to our service in order to offer our customers the best possible price-to-performance ratio balanced with data availability and safety. As part of that work, we are changing the default number of data members for {{site.data.keyword.databases-for-mongodb_full}} from two to three. See blog post announcement [here](https://www.ibm.com/cloud/blog/databases-for-mongodb-improved-high-availability-architecture).

## 13 April 2020
{: #databases-for-mongodb-13apr2020}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}} autoscaling
:  We are excited to announce that autoscaling of your deployments based on disk capacity and disk I/O utilization is now available for {{site.data.keyword.databases-for-mongodb_full}} via the UI, API, and CLI. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-databases-portfolio-introduces-autoscaling).

## 6 August 2019
{: #databases-for-mongodb-06aug2019}
{: release-note}

New Regions Available for IBM Cloud Database Services
:  {{site.data.keyword.databases-for-mongodb_full}} is now available to be deployed in Seoul; South Korea; and Chennai, India. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/new-regions-available-for-ibm-cloud-database-services).

## 27 February 2019
{: #databases-for-mongodb-27feb2019}
{: release-note}

General Availability of {{site.data.keyword.databases-for-mongodb_full}}
:  {{site.data.keyword.databases-for-mongodb_full}} added to the [IBM Cloud Databases](https://www.ibm.com/cloud/databases) family. See blog post announcement [here](https://www.ibm.com/cloud/blog/ibm-cloud-databases-for-mongodb-is-generally-available#:~:text=IBM%20Cloud%20is%20announcing%20the,with%20enterprise%20security%20in%20mind.).
