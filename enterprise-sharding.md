---
copyright:

  years: 2025
lastupdated: "2025-11-03"

keywords: databases, mongodbee, Enterprise Edition, sharding, horizontal scaling

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise Edition Sharding
{: #enterprise-sharding}

{{site.data.keyword.databases-for-mongodb_full}} EE (Enterprise Edition) Sharding allows you to horizontally scale your workloads by distributing data across multiple machines.

## When to use {{site.data.keyword.databases-for-mongodb}} EE Sharding
{: #enterprise-sharding-when-to-use}

- Your database needs grow beyond what vertical scaling can handle – adding more CPUs or RAM eventually hits physical limits; MongoDB on IBM Cloud supports a maximum of 4 TB per replica set.
- You require higher throughput for reads and writes – horizontal scaling adds cluster nodes, allowing you to distribute the load across multiple shards instead of overloading a single node.
- Your application benefits from fine‑grained query isolation across large collections – by choosing appropriate shard keys you can keep related documents together while evenly distributing the rest of the data, reducing cross‑shard traffic for common queries and keeping hot spots in check.

## Enterprise plan versus Enterprise Sharding plan
{: #enterprise-sharding-enterprise-vs-sharding}

|                  Feature             | Enterprise Edition (single replica set) | Enterprise Edition Sharding           |
|--------------------------------------|-----------------------------------------|--------------------------------------------|
| Maximum storage                      | Up to 4 TB per replica set              | Up to 24 TB (6 shards × 4 TB each)  |
| Scaling type                         | Vertical scaling: add CPU and RAM to existing nodes | Horizontal scaling: add shards to distribute data <br> Vertical scaling: add CPU and RAM to individual shards |
| Throughput scaling                   | Limited by single cluster hardware       | Increases as shards are added             |
{: caption="Enterprise plan versus Enterprise Sharding plan" caption-side="bottom"}

## MongoDB EE Sharding considerations
{: #enterprise-sharding-considerations}

Sharding is not only an infrastructure operation. It is a shared responsibility between the provider and the customer. {{site.data.keyword.databases-for}} is responsible for provisioning nodes according to user needs, scaling vertically or horizontally. You are expected to:  

- Enable sharding for each collection. Unsharded collections get stored in a single shard. These unbalanced nodes can get full before others and cause operational problems.
- Select an appropriate [shard key](https://www.mongodb.com/docs/manual/core/sharding-shard-key/){: external} to distribute data evenly and avoid shard overloading.  
- Optimize queries to minimize scatter and gather operations, which are costly in both time and resources.  
- Additional infrastructure incurs additional cost. For more information, see [Pricing](/docs/databases-for-mongodb?topic=databases-for-mongodb-pricing).  

For more information, see [Responsibilities for {{site.data.keyword.databases-for}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-responsibilities-cloud-databases).
{: tip}

### Key considerations
{: #enterprise-sharding-key-considerations}

- **Max cluster size**: Up to 6 shards × 3 members (18 members total).  
- **Storage**: Maximum of 4 TB per shard (2 TB recommended best practice).  
- **Scaling**: Shards can only be added, not removed. Plan your scaling strategy in advance.  
- **Hosting model**: Requires **Isolated Compute**.  
- **Supported versions**: MongoDB EE ≥ 7.0 only.  
- **Migration**: Existing unsharded deployments cannot be converted. To adopt sharding, create a new sharded deployment and migrate your data.  
- **Shard-key design**: Use high-cardinality keys to ensure balanced distribution and reduce scatter/gather overhead. 
 
