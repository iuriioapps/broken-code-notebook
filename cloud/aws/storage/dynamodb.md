# DynamoDB

AWS DynamoDB is a fast and flexible NoSQL database service for all applications that need consistent, single-digit millisecond latency at any scale. It is a fully managed database and supports both document  and key-value data models.

### Basics

* NoSQL database that provides fast and predictable performance with seamless scalability.
* Offers encryption at rest.
* You can create database tables that can store and retrieve any amount of data, and serve any level of request traffic.
* You can scale up or down your table throughput capacity without downtime without downtime or performance degradation, and use console to monitor resource utilization, and performance metrics.
* Provides on-demand backup capability as well as point-in-time recovery, you can restore that table to any point in time during the last 35 days.
* All of your data is stored in partitions, backed by SSDs and automatically replicated across multiple AZs in AWS region, providing built-in high availability and durability.
* You can create tables that are automatically replicated across two or more regions, with full support for multi-master writes.

### Core Components

* **Tables** -  a collection of items
  * DynamoDB stores data in a table, which is a collection of data.
  * Are seamless.
  * There is initial limit of 256 tables per region.
* **Items** - a collection of attributes
  * DynamoDB uses primary keys to uniquely identify each item in a table and secondary indexes to provide more querying flexibility.
  * Each table contains zero or more items.
* **Attributes** - a fundamental data element
  * DynamoDB supports nested attributes up to 32 levels deep.
* **Primary Key** - uniquely identifies each item in a table, so that no two items can have the same key
  * Must be scalar
  * **Partition key** - a simple primary key, composed of one attribute.
  * **Partition key and sort key \(composite primary key\)** - composed of two attributes
  * DynamoDB uses the partition key value as an input to an internal hash function. The output from the hash function determines the partition in which the item will be stored. All items with the same partition key are stored together, in sorted order by sort key value. If no sort key is used, no two items can have the same partition key value.
* **Secondary indexes** - lets you query the data in the table using an alternative key, in addition to queries against the primary key
  * You can create one or more secondary indexes on the table. Supports local secondary indexes and global secondary indexes.
  * **Global secondary index** - an index with partition key and sort key that can be different from those of the table.
  * **Local secondary index** - an index that has the same partition key as the table, but a different sort key.
  * You can define up to 5 global secondary indexes and up to 5 local secondary indexes.
* **Streams** - an optional feature that captures the data modification event in DynamoDB tables
  * The naming convention for DynamoDB stream endpoints is `streams.dynamodb.amazonaws.com`
  * Each event is represented by a stream record and captures the following events:
    * a new item is added to the table - captures the image of the entire item, including all of its attributes
    * an item is updated - captures the "before" and "after" image of any attributes that were modified in the item.
    * an item is deleted - captures the image of the entire item before it was deleted.
  * Each stream record also contains the name of the table, the event timestamp, and other metadata.
  * Stream records are organized into groups, or shards, Each shard acts as a container for multiple stream records, and contains information required for accessing and iterating through these records.
  * Stream records have a lifetime of 24 hours, after that they are removed from the stream.
  * You can use streams with Lambda to create a trigger.
  * Streams enable powerful solutions such as data replication within or across regions, materialized views of data in DynamoDB tables, data analytics using Kinesis materialized views and much more.

### Data Types for Attributes

* **Scalar types** - a scalar type can represent exactly one value. The scalar types are number, string, binary, boolean and null. Primary keys should be scalar types.
* **Document types** - a document type can represent a complex structure with nested attributes such as you would find in a JSON document. Document types are list and map.
* **Set types** - a set types can represent multiple scalar values. The set types are string set, number set and binary set.

#### Other notes

* When you read data from DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data, but you should eventually have consistent reads.
* When you request a strongly consistent reads, DynamoDB returns result with the most up-to-date data, reflecting the updates from all prior write operations that were successful. A strongly consistent read might not be available is there is a network delay or outage.
* DynamoDB does not support strongly consistent reads across regions.
* When you create a table or index in DynamoDB, you must specify your throughput capacity requirements for read and write activity in terms of:
  * One **read capacity unit \(RCU\)** represents one strongly consistent read per second, or two eventually consistent reads per second, for an item up to 4KB in size. If you need to read an item that is larger than 4KB, DynamoDB will need to consume additional RCUs.
  * One **write capacity unit \(WCU\)** represents one write per second for an item up to 1KB in size. If you need to write an item that is larger than 1KB, DynamoDB will need to consume additional WCUs.
* Throttling prevents your application from consuming too many capacity units. DynamoDB can throttle read or write requests that exceed throughput settings for a table, and can also throttle read requests for an index.
* When a request is throttled, it fails with an HTTP 400 error \(BadRequest\) and a `ProvisionedThroughputExceedException`

### Throughput management

* **DynamoDB autoscaling**
  * Define a range \(upper and lower limit\) for RCU and WCU, and define target utilization percentage within a range.
  * A table or a global secondary index can increase its provisioned RCUs to handle sudden increase in traffic, without request throttling.
  * DynamoDB autoscaling can decrease the throughput when the workload decreases so that you don't pay for unused provisioned capacity.
* **Provisioned throughput** - manually defined maximum amount of capacity that the application can consume from a table or index. If your application exceeds provisioned throughput settings, it is a subject to request throttling.
* **Reserved capacity** - with reserved capacity, you pay a one-time upfront fee to a minimum usage level over a period of time, for cost saving solutions.

### Capacity unit consumption

* **RCU** - strongly consistent reads consume 1 capacity unit, while eventually consistent read request consume $$1/2$$ of read capacity unit
  * `GetItem` - reads single item
  * `BatchGetItem` - reads up to 100 items
  * `Query` - reads multiple items that have the same partition key
  * `Scan` - reads all of the items in the table
* **WCU** - write capacity unit
  * `PutItem` - writes single item
  * `UpdateItem` - modifies a single item
  * `DeleteItem` - removes a single item
  * `BatchWriteItem` - writes up to 25 items to one or more tables

### Autoscaling

* When you use AWS Console to create a new table, autoscaling is enabled by default.
* You create a scaling policy for a table or a global secondary index. The scaling policy specifies whether you want to scale read capacity or write capacity \(or both\) and the minimum and maximum provisioned capacity unit settings for the table or index. The scaling policy also contains a target utilization, which is the percentage of consumed provisioned throughput at a point in time.
* Autoscaling does not prevent you from manually modifying provisioned throughput.
* If you enable autoscaling for a table that has a global secondary indexes, AWS highly recommends that you also apply autoscaling uniformly to those indexes.

### DynamoDB Items

* You can use `UpdateItem` operation to implement an atomic counter - a numeric attribute that is incremented, unconditionally, without interfering with other write requests.
* DynamoDB optionally supports conditional writes for these operations: `PutItem`, `UpdateItem`, `DeleteItem`. A conditional write will succeed only if the item attributes meet one or more expected conditions.
* Conditional writes can be idempotent if the conditional check is on the same attribute that is being updated. DynamoDB performs a given write request only if the certain attribute values in the item match what you expect them to be at the time of the request.
* Expressions
  * To get only a few attributes of an item, use a `projection` expression.
  * An expression attribute name is a placeholder that you use in an expression, as an alternative to an actual attribute name. An expression attribute name must begin with a `#` and be followed by one or more alphanumeric characters.
  * For `PutItem`, `UpdateItem` and `DeleteItem` operations, you can specify a condition expression to determine which items should be modified. If the condition expression evaluates to true, the operation succeeds.
  * An update expression specifies how `UpdateItem` will modify attributes of an item - for example, setting a scalar value, or removing elements from a list or a map.

### Time To Live \(TTL\)

* Allows you to define when items in a table expire so that they can be automatically deleted from the database.

### Queries

* A query operation finds items based on a primary key values. You can query any table or secondary index that has a composite primary key.
* A key condition expression is a search criteria that determines the items to be read from table or index.
* You must specify the partition key name and value as an equality condition.
* You can optionally provide a second condition for the sort key.
* A single query operation can retrieve maximum of 1Mb.
* For further refining of query operations you can optionally provide a filter expression, to determine which items within the query results should be returned. All of the other results are discarded.
* Query operation allows you to limit the number of items that it returns in the result by setting the `Limit` parameter to the maximum number of items you want.
* DynamoDB paginates the results from query operations, where query results are divided into "pages" of data that are 1Mb in size \(or less\).
* `ScannedCount` is a number of items that matched the key condition expression, before filter expression \(if present\) was applied.
* `Count` is the number of items that remain, after the filter expression \(if present\) was applied.

### Scans

* A scan operation reads every item in a table or secondary index. By default, a `Scan` operation returns all of the data attributes for every item in the table or index.
* Scan always returns a result set. If no matching items are found, the result set will be empty.
* A single scan request can retrieve a maximum of 1Mb of data.
* You can optionally provide a filter expression.
* You can limit the number of items in the result.
* DynamoDB paginates the results from `Scan` operations.
* `ScannedCount` is the number of items evaluated, before any scan filter is applied.
* `Count` is the number of items that remain, after a filter expression \(if present\) was applied.
* A `Scan` operation performs eventually consistent reads, by default.
* By default, `Scan` operation process data sequentially.

### Global Tables

* Global tables provide a solution for deploying multi-region, multi-master databases, without having to build and maintain your own replication solution.
* You specify the AWS regions where you want the table to be available. DynamoDB performs all tasks to create internal tables in these regions, and propagate ongoing data changes to all of them.
* Replica tables:
  * A single DynamoDB table that functions as a part of a global table.
  * Each replica stores the same set of items.
  * Any given global table can only have one replica table per region.
  * You can add new or delete replicas from global table.
* To ensure eventual consistency, DynamoDB global tables use a "last write wins" reconciliation between concurrent updates, where DynamoDB makes a best effort to determine the last writer.
* If a single regions becomes isolated or degraded, your application can redirect to a different region and perform reads and writes against different replica table. DynamoDB also keeps track of any writes that have been performed, but have not yet been propagated to all of the replica tables.
* Requirements for adding a new replica table:
  * must have the same partition key as all of the other replicas
  * must have the same write capacity management settings specified
  * must have same name as all of the other replicas
  * must have streams enabled, with the stream containing both new and old images of the item
  * none of the replica tables in the global table can contain any data
* If global secondary indexes are specified, then the following conditions must also be met:
  * global secondary index must have the same name
  * global secondary index must have the same partition key and sort key \(if present\)

### On-demand Backup and Restore

* You can use IAM to restrict DynamoDB backup and restore actions for some resources
* All backup and restore actions are captured and recorded in CloudTrail
* Backups:
  * Each time you create a backup, the entire table is backed up
  * All backups and restores in the DynamoDB work without consuming any provisioned throughput on the table.
  * DynamoDB backups do no guarantee causal consistency across the items; however the skew between updates in a backup is usually much less than a second
  * Backup and restore works only in the same AWS region as the source table
  * Included in the backup are
    * database data
    * global secondary indexes
    * local secondary indexes
    * streams
    * provisioned RCU and WCU
  * While backup is in progress you can't do the following:
    * pause or cancel backup operation
    * delete the source table of the backup
    * disable backups on the table
* Restore:
  * You can not overwrite the existing table during the restore operation
  * You restore backup to a new table
  * For tables with even data distribution across your primary keys, the restore time is proportional to the largest single partition by item count and not the overall table size.
  * If your source table contains data with significant skew, the time to restore may increase.

### Security

* Encryption
  * Encrypts your data at rest using KMS key
  * Encryption at rest can be enabled only when you're creating new table
  * After encryption is enabled, it can not be disabled
  * Uses AES 256 encryption
  * The following are encrypted
    * base table
    * LSI
    * GSI
* Authentication and access control
  * Access to DynamoDB requires credentials
  * Aside from valid credentials, you also need permissions to create or access DynamoDB resources
* You can create indexes and streams only in the context of the existing DynamoDB table, referred to as subresources
* Resources and subresources have unique ARN associated with them
* A permission policy describes who has access to what
  * Identity based policies:
    * attach a permission policy to a user or a group in your account
    * attach a permission policy to a role \(grant cross-account permissions\)
  * Policy elements
    * Resource - use an ARN to identify the resource that the policy applies to
    * Action - use action keywords to identify resource operations that you want to allow or deny
    * Effect - specify the effect, either allow or deny, when the user requests the specific action
    * Principal - the user that the policy is attached to is the implicit principal
  * Web Identity Federation - customers can sign in to an Identity Provider and then obtain temporary security credentials from AWS STS.





