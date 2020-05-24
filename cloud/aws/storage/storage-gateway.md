# Storage Gateway

* Storage Gateway consists of an on-premises software appliance which connects with AWS cloud-based storage to give you a seamless and secure integration between your on-premises IT environment and AWS.

* **File Gateway**

  * Files stored as objects in S3 buckets
  * Accessed using SMB or NFS mount points
  * To your on-premises systems this appears like a file system mount backed by S3
  * All the benefits of S3: bucket policies, S2 versioning, lifecycle management, replication, etc.
  * Low-cost alternative to on-premises storage

* **Volume Gateway**

  * Provides cloud backed storage which is accessed using iSCSI protocol
  * 2 different types available:
    * Gateway Stored Volumes
      * The gateway stores all your data locally, so your applications get low latency access to the entire dataset
      * You need your own storage infrastructure as all data is stored locally in your data center
      * Provides durable off-site async backups in the form of EBS snapshots which are stored in S3
    * Gateway Cached Volumes
      * The gateway stores all your data in S3 and caches only frequently accessed data locally
      * You need only enough local storage capacity to store the frequently accessed data
      * Applications still get low-latency access to frequently used data without a large investment in on-premise storage

* **Tape Gateway**
  * Virtual Tape Library which provides cost effective data archiving in the cloud using Glacier
  * You don't need to invest in your own tape backup infrastructure
  * Integrates with existing tape backup infrastructure - NetBackup, Backup Exec, Veeam, etc. which connect to the VTL using iSCSI
  * Data is stored on virtual tapes which are stored in Glacier and accessed using VTL

