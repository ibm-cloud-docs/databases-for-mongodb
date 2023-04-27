---

copyright:
  years: 2015, 2023
lastupdated: "2023-04-27"

keywords: mongodb, databases, mongodb compass, mongodbee, mongodb enterprise, mongodb ee provision, mongodb compass, mongodb ops manager

subcollection: databases-for-mongodb

content-type: tutorial
services: 
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.databases-for-mongodb}}
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

<!-- The title of your H1 should be Getting started with _service-name_, where _service-name_ is the non-trademarked short version keyref. -->

<!-- The goal should be a tutorial of 10 minutes or less. -->

_The short description should be a single, concise paragraph that contains one or two sentences and no more than 50 words. Briefly mention what the user's learning goal is and include the following SEO keywords in the title short description: IBM Cloud, ServiceName, tutorial. If the release phase of your service is experimental or beta, be sure to indicate that in the first occurrence of the service name, for example, Cost and Asset Management (Experimental)._ For example: "In this getting started tutorial, we'll take you through a sample node.js ToDo app that will take you about 10 minutes to deploy."

IBM Cloud® Databases for MongoDB allows developers to take advantage of the latest MongoDB features: rich JSON documents, powerful query language, multi-document transactions, and authentic APIs. The service also automates common database administration tasks like high availability, backups, encryption, and infrastructure planning.
{: shortdesc}

## Before you begin
{: #prereqs}

You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external} and a {{site.data.keyword.databases-for-mongodb}} deployment. Give your deployment a memorable name that appears in your account's Resource List and set the [Admin Password](/docs/databases-for-mongodb?topic=databases-for-mongodb-admin-password) for your deployment. If your deployment is not using public endpoints, take [these additional steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-endpoints#private-endpoints) to configure private endpoint access.

MongoDB cannot support both public and private endpoints simultaneously. This cannot be changed after provisioning.
{: important}

Finally, review [Getting to production](/docs/cloud-databases?topic=cloud-databases-best-practices) for general guidance on setting up a basic {{site.data.keyword.databases-for-mongodb}} deployment.

<!-- For each step in your tutorial, add an H2 section. The title should be task-oriented and descriptive. Recommendation is no more than 9 steps. -->

## _Title should be task oriented and descriptive_
{: #anchor_value}
{: step}

<!-- Introduce each major step with a description of what it will accomplish. If there are sequential substeps, use an ordered list for each substep. Don't include the step number. -->

_If you have substeps, introduce them and then follow with the steps as paragraph chunks. Do not use numbers or substep labels for a single substep or unless it is a true sequence._

_For commands, introduce the command and then surround what the user must enter in the command prompt with three backticks, set the programming language if it applies, and follow with a pre attribute if you want the copy button applied._ For example:
Now you're ready to start working with the app. First, clone the repo with the sample app code.

   ```sh
   git clone https://github.com/IBM-Cloud/get-started-node
   ```
   {: pre}
 
   Then, change the directory to where the sample app is located.
 
   ```sh
   cd get-started-node
   ```
   {: pre}

## _Title should be task oriented and descriptive_
{: #anchor_value}
{: step}

## _Title should be task oriented and descriptive_
{: #anchor_value}
{: step}

## _Title should be task oriented and descriptive_
{: #anchor_value}
{: step}

_If you have any "tips" to include for this step, add the content in the flow where you would like it to appear. This information should be not be required information for completing the step, but helpful information in explaining additional concepts about what the user is doing in the step. It should be in the following format:_

One to two sentences of content that can include inline links or lists.
{: tip}

_You can have multiple "tips" per step. Each tip will output with a *Tip:* label and be formatted in a nested, styled box._

## Next steps
{: #anchor_value}

_What's the single thing the user needs to do next? Think "guided journey." Either provide information that leads the user to production use, for example HA, how to make a service secure, or how to connect to on-premise data. Or you can point the user to another tutorial. Give a choice between two options max._
