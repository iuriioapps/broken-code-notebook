---
description: Elastic Block Store
---

# EBS

{% hint style="info" %}
IOPS - Input / output Operations per Second

* TiB - Tibibyte \( $$2^{40}$$ bytes\), 1TiB  ~= 1.0995 TB
* GiB - Gibibyte \( $$2^{30}$$ bytes\), 1GiB ~= 1.074 GB
{% endhint %}

Amazon EBS provides persistent block storage volumes for use with EC2 instances. Each EBS volume is automatically replicated within its AZ to protect from component failure, offering high availability and durability.

### Types

* General purpose SSD
* Provisioned IOPS SSD
* Throughput optimized HDD
* Cold HDD
* Magnetic

#### General purpose SSD

General purpose SSD volume that balances price and performance for a wide variety of transaction workloads

* Use case: most workloads
* API name: gp2
* Size: 1GiB - 16 TiB
* Max IOPS: 16000

#### Provisioned IOPS SSD

High-performance SSD volume designed for mission-critical operations

* Use case: databases
* API name: io1
* Size: 4GiB - 16TiB
* Max IOPS: 64000

#### Throughput-optimized HDD

Low cost HDD volume designed for frequently accessed, throughput intensive workloads.

* Use case: big data, data warehouse
* API name: st1
* Size: 500GiB - 16TiB
* Max IOPS: 500

#### Cold HDD

Lowest cost HDD volume designed for less frequently accessed workloads.

* Use case: file servers
* API name: sc1
* Size: 500GiB - 16TiB
* Max IOPS: 250

#### Magnetic

Previous generation HDD

* Use case: workloads where data is infrequently accessed
* API name: Standard
* Size: 1GiB - 1TiB
* Max IOPS: 40-200

### Volumes and Snapshots

* Volumes exist on EBS. Snapshots exist on S3.
* Snapshots are point-in-time copies of volumes.
* Snapshots are incremental - this means that only the blocks that have changed are moved to S3.
* If this is your first snapshot, it may take some time to create.
* To create a snapshot for EBS volume that serve as a root device, you should stop the instance before taking a snapshot. However, you can take a snapshot while instance is running.
* You can create AMIs from both volumes and snapshots.
* You can change EBS volume sizes on the fly, including the size and storage type.
* Volumes will always be in the same availability zone as EC2 instance it's attached to.
* To move EC2 volume from one AZ to another, take a snapshot of it, create an AMI of the snapshot to launch the EC2 instance in another AZ.
* To move and EC2 volume from one region to another, take a snapshot of it, create an AMI from the snapshot, and then copy AMI from one region to the other. Then use the copied AMI to launch the new EC2 instance in the new region.

### EBS encryption

* Snapshots of encrypted volumes are encrypted automatically.
* Volumes restored from encrypted snapshots are encrypted automatically.
* You can share snapshots, but only if they are unencrypted. These snapshots can be shared with another AWS account or made public.
* You can now encrypt root device volumes upon creation of the EC2 instance.

### EBS vs Instance Store Volumes

All AMIs are categorized as either backed by EBS or backed by instance store. 

* For EBS volumes: the root device for an instance launched from the AMI is an Amazon EBS volume created from EBS snapshot.
* For instance store volumes: the root device for an instance launched from the AMI is an instance store volume created from a template stored in S3. 
* Instance store volumes are sometimes called _**Ephemeral Storage**_
* Instance store volumes can not be stopped. If the underlying host fails, you will lose your data.
* EBS backed instances can be stopped. You will not lose the data on this instance if it's stopped.
* You can reboot both, you will not lose your data.
* By default, both root volumes will be deleted on permination. However, with EBS volumes, you can tell AWS to keep root device volume.

### I/O credits

* When your volume requires more than baseline performance I/O level, it simply uses I/O credits in the credit balance to burst to the required performance level, up to maximum of 3000 IOPS.
* Each volume receives an initial I/O credit balance of 5,400,000 I/O credits.
* This is enough to sustain the maximum burst performance of 3000 IOPS for 30 minutes.
* When you're not going over your provisioned I/O level, you will be earning credits.

### Pre-warming EBS volumes

New EBS volumes receive their maximum performance the moment they are available and do not require initialization \(formerly known as pre-warming\). However, storage blocks on volumes that were restored from snapshots must be initialized \(pulled down from S3 and written to the volume\). before you can access the block. This preliminary action takes time and can cause a significant increase in latency of an I/O operation the first time each block is accessed. For most applications, amortizing this cost over the lifetime of the volume is acceptable. Performance is restored after the data is accessed once.

You can avoid this performance hit in the production environment by reading from all of the blocks on your volume before you use it; this process is called initialization. For a new volume created from snapshot, you should read all the blocks that have data before using the volume.

### Modifying EBS volumes

If your EBS volume is attached to a current generation EC2 instance type, you can increase its size, change it's volume type or \(for io1 volumes\) adjust its IOPS performance, all without detaching it. You can apply those changes to detached volumes as well.

* Issue the modification command \(console or command line\).
* Monitor the progress of the modification.
* If the size of the volume was modified, extend the volume's file system to take advantage of the increased storage capacity.

