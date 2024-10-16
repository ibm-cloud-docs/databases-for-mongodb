---

copyright:
  years: 2024
lastupdated: "2024-10-16"

keywords:

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}


# Understanding data portability for {{site.data.keyword.databases-for-mongodb}}
{: #data-portability}


[Data portability](#x2113280){: term} involves a set of tools, and procedures that enable customers to export the digital artifacts that would be needed to implement similar workload and data processing on different service providers or on-prem software. It includes procedures for copying and storing the service customer's content, including the related configuration used by the service to store and process the data, on customer's own location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

{{site.data.keyword.cloud}} services provide interfaces and instructions to guide the customer to copy and store the service customer content, including the related configuration, on their own selected location.

The customer then is responsible for the use of the exported data and configuration for the purpose of data portability to other infrastructures.
This can involve:

- The planning and execution for setting up alternate infrastructure on on different cloud providers or on-prem software that provide similar capabilities to the IBM services.
- The planning and execution for the porting of the required application code on the alternate infrastructure, including the adaptation of customer's application code, deployment automation, and so on.
- The conversion of the exported data and configuration to format required by the alternate infrastructure and adapted applications.


For more information about your responsibilities when using {{site.data.keyword.databases-for-mongodb}}, see [Shared responsibilities for {{site.data.keyword.databases-for-mongodb}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-responsibilities-cloud-databases).

## Data export procedures
{: #data-portability-procedures}

{{site.data.keyword.databases-for-mongodb}} provides mechanisms to export your content that has been uploaded, stored, and processed using the service.


To export your MongoDB data use Mongo's [mongodump](https://www.mongodb.com/try/download/database-tools){: external} facility, which comes bundled with the command line tools in the download.

You can export entire databases or collections within databases by following the steps in the [MongoDB documentation](https://www.mongodb.com/docs/database-tools/mongodump/#mongodb-binary-bin.mongodump){: external}.




## Exported data formats
{: #data-portability-data-formats}



The data exported will be in Mongo's [BSON format](https://www.mongodb.com/resources/languages/bson){: external}.

The exported data can be uploaded to any other MongoDB instance using the [mongorestore](https://www.mongodb.com/docs/database-tools/mongorestore/) facility, which also comes bundled in the above tools download.

## Data ownership
{: #data-ownership}

All exported data are classified as Customer content and therefore apply to them the full customer ownership and licensing rights, as stated in [{{site.data.keyword.cloud_notm}} Service Agreement](https://www.ibm.com/terms/?id=Z126-6304_WS).
