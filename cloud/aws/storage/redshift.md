# Redshift

Amazon Redshift is a fast and powerful, fully-managed, petabyte-scale data warehouse service.

### Redshift can be configured as follows

* Single node
* Multi-node
  * Leader node - manages client connections and receives queries
  * Compute node - store data and perform queries and computations. Can create up to 128 compute nodes.

### Redshift advanced compression

Columnar data stores can be compressed much more than row-based data stores because similar data is stored sequentially on disk. Amazon Redshift employs multiple compression techniques and can often achieve significant compression relative to traditional relational data stores. In addition, Amazon Redshift does not require indexes or materialized views, and so uses less space than traditional RDS. When loading data into an empty table, Amazon Redshift automatically samples data and selects the most appropriate compression scheme.

### Massively Parallel Processing \(MPP\)

Amazon Redshift automatically distributes data and query load across all nodes. Redshift makes it easy to add nodes to your data warehouse and enables you to maintain fast query performance as your data warehouse grows.

### Backups

* Enabled by default, with 1 day retention period.
* Maximum retention period is 35 days.
* Redshift always attempts to maintain at least 3 copies of your data \(the original and replica on the compute nodes and a backup in S3\).
* Redshift can also asynchronously replicate your snapshots to S3 in another region for disaster recovery.

### Encryption

* Encrypted in transit using SSL
* Encrypted at rest using AES-256
* By default Redshift takes care of key management, but KMS or CloudHSM keys can be used to.

### Availability

* Currently only available in 1 AZ
* Can restore snapshots to new AZ in the event of an outage.

 

