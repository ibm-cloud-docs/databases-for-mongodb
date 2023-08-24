---
copyright:
  years: 2023
lastupdated: "2023-08-24"

subcollection: databases-for-mongodb, oplog, operations log, oplogurl

keywords: 

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB operations log (oplog) FAQ
{: #mongodb-faq-oplog}
{: faq}
{: support}

The MongoDB oplog (operations log) is a special capped collection that records all operations that modify the data stored in the database. For more information, see [Replica Set Oplog](https://www.mongodb.com/docs/manual/core/replica-set-oplog/){: external}.
{: shortdesc}

## Accessing MongoDB oplog through `oplogUrl`
{: #mongodb-faq-oplog}
{: faq}
{: support}

To access the MongoDB oplog through `oplogUrl`, create an `oplog` user using the {{site.data.keyword.databases-for-mongodb_full}} `admin` user. The `oplog` user can set `&readPreference=primary` so that reads always happen from the primary.

As the `admin` user, use a command like:

```sh
db.createUser({user: "oplogger", pwd: "(redacted)", roles: [{role: "read", db: "local"}, {"role": "readAnyDatabase", "db": "admin"}, "readWriteAnyDatabase", "dbAdminAnyDatabase" ]})
```
{: pre}
