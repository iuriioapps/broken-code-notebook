# Config

AWS Config is a fully managed service that provides you with an AWS resource inventory, configuration history and configuration notifications to enable security and governance.

* Enables
  * compliance auditing
  * security analysis
  * resource tracking
* Provides
  * configuration snapshots and log config changes of AWS resources
  * automated compliance checking

### Terminology

* **Configuration Items** - point-in-time attributes of resources
* **Configuration Snapshots** - collection of config items
* **Configuration Stream** - stream of changed config items
* **Configuration History** - collection of config items for a resource over time
* **Configuration Recorder** - the configuration of config that reads and stores config items

### Recorder setup

* logs config for account in region
* stores in S3
* notifies SNS

### What can we see

* Resource type
* Resource ID
* Compliance 
* Timeline
  * Configuration details
  * Relationships
  * Changes
  * CloudTrail events

### Compliance checks

* Trigger
  * Periodic
  * Configuration snapshot delivery
* Managed rules
  * About 40 \(maybe more...\)
  * Basic but fundamental

