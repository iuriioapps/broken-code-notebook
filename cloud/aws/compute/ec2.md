---
description: Elastic Compute Cloud
---

# EC2

### Pricing Models

* On-demand
* Reserved
* Spot
* Dedicated hosts

#### On-Demand

* Users that want the low cost and flexibility of EC2 without any up-front payment of long-term commitments.
* Applications with short term, spiky, or unpredictable workloads that can not be interrupted.
* Applications being developed or tested on EC2 for the first time.

#### Reserved

* Applications with steady state or predictable usage
* Applications that require reserved capacity
* Users able to make upfront payments to reduce their total computing costs even further
  * Standard RI \(up to 75% off On-Demand\)
  * Convertible RI \(up to 54% off On-Demand\), capability to change the attributes of the RI as long as the exchange results in the creation of Reserved Instances of equal or greater value.
  * Scheduled RI's available to launch within the time windows you reserve. This option allows you to match your capacity reservation to a predictable recurring schedule that only requires a fraction of a day, a week or a month.

#### Spot

* Application that has flexible start and end times.
* Application that is only feasible at very low compute prices.
* Users with urgent computing needs for large amounts of additional capacity.

#### Dedicated Hosts

* Useful for regulatory requirements that may not support multi-tenant virtualization.
* Greate for licensing which does not support multi-tenancy or cloud deployments.
* Can be purchased On-Demand \(hourly\).
* Can be purchased as a Reservation for up to 70% off the On-Demand price.

#### Dedicated Instances vs. Dedicated Hosts

* Both dedicated instances and dedicated hosts have dedicated hardware.
* Dedicated instances are charged by the instance, dedicated hosts are charged by the host.
* If you have specific regulatory requirements or licensing conditions, choose dedicated hosts.
* Dedicated instances may share the same hardware with other AWS instances from the same account that are not dedicated.
* Dedicated hosts give you much better visibility into things like sockets, cores, and host ID.

### Instance types

| FAMILY | SPECIALITY | USE CASE |
| :--- | :--- | :--- |
| **F1** | FPGA | Genomics, financial, video, big data |
| **I3** | High-speed storage | NoSQL DB, data warehousing |
| **G3** | Graphics intensive | Video encoding, 3D |
| **H1** | High disk throughput | MapReduce, HDFS |
| **T3** | Low cost, general | Web servers, small DBs |
| **D2** | Dense storage | File server, Hadoop |
| **R5** | Memory optimized | Memory-intensive apps |
| **M5** | General purpose | App servers |
| **C5** | Compute optimized | CPU intensive apps |
| **P3** | Graphics, general purpose | ML, cryptocurrency |
| **X1** | Memory optimized | SAP HANA, Spark |
| **Z1D** | High compute capacity and memory | Analytics, certain databases |
| **A1** | ARM-based workloads | Scale-out workloads |
| **U-6tb1** | Bare metal | No virtualization overhead |

### Instance metadata

* curl http://169.254.169.154/latest/meta-data
* curl http://169.254.169.154/latest/user-data

### Launch errors

* **InstanceLimitExceeded** error - you have exceeded the default limit for number of instances you can launch in a region
* **InsufficientInstanceCapacity** error - AWS does not currently have enough available On-Demand capacity to service your request

### Troubleshooting

* Instances not launching into Autoscaling group
  * associated key pair does not exist
  * security group does not exist
  * autoscaling config is not working correctly
  * autoscaling group not found
  * instance type specified is not supported in AZ
  * AZ is no longer supported
  * invalid EBS device mapping
  * autoscaling service is not enabled in your account
  * attempting to attach EBS block device to an instance store AMI

## AMI - Amazon Machine Image

### Converting unencrypted AMI to encrypted AMI

* Create snapshot of the unencrypted root device volume.
* Create a copy of the snapshot, select the encryption option
* Create an AMI from the encrypted snapshot
* Use that AMI to launch new encrypted instance

#### Sharing AMIs

* AMIs can be shared and copied between user accounts
* Restrictions
  * Encrypted AMIs
  * Copy the underlying snapshot, re-encrypt using your own key and create a new AMI from the snapshot
  * AMIs with associated `billingProducts` code \(e.g. Windows AMIs, RedHat, AWS Marketplace AMIs\)
  * Launch an EC2 instance using the shared AMI and create an AMI from that instance.

## Placement Groups

When you launch a new EC2 instance, EC2 service attempts to place an instance in such a way that all of your instances are spread out across underlying hardware to minimize the correlated failures. You can use placement groups to influence the placement of a group of independent instances to meet the needs of your workload. Depending of the type of your workloads, you can create a placement group using one of the following placement strategies:

* **Cluster** - packs instances close together inside the AZ. This strategy enables workloads to achieve low-latency network performance for tightly-coupled node-to-node communication that is typical of HPC \(high-performance compute\) applications.
* **Partition** - spreads your instances across logical partitions such that group of instances in one partition do not share the underlying hardware with group of instances in different partitions.This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra and Kafka.
* **Spread** - strictly places a small group of instances across distinct underlying hardware to reduce correlated failure. 
* A clustered placement group can't span multiple AZs. A spread and partitioned placement group can.
* The name you specify for placement group must be unique within your AWS account.
* Only certain types of instances can be launched in a placement group \(compute optimized, GPU, memory optimized, storage optimized\).
* AWS recommends homogeneous instances within the clustered placement group.
* You can not merge placement groups.
* You can not move an existing instance into a placement group. You can create an AMI from your existing instance, and then launch a new instance from the AMI into a placement group.
* A spread placement group supports a maximum of 7 running instances per AZ.

## ENI vs ENA vs EFA

* **ENI - Elastic Network Interface** - essentially a virtual network card. 
  * It allows:
    * A primary private IPv4 address from the IPv4 address range of your VPC.
    * One or more secondary private IPv4 addresses from the IPv4 address range of your VPC.
    * One Elastic IP address \(IPv4\) per private IPv4.
    * One public IPv4 address.
    * One or more IPv6 addresses
    * One or more security groups
    * A MAC address
    * A source / destination check flag
    * A description
  * Scenarios for ENIs:
    * Create a managed network
    * Use network and security appliances in your VPC
    * Create a dual-homed instances with workloads/roles on distinct subnets
    * Create a low budget, high-availability solution
* **EN - Enhanced Networking**. 
  * Uses single root I/O virtualization \(SR-IOV\) to provide high-performance networking capabilities on supported instance types.
  * What is `Enhanced Networking`
    * It uses single root I/O virtualization \(SR-IOV\) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces.
    * Enhanced networking provides higher bandwidth, higher packet per second \(PPS\) performance and consistently lower inter-instance latencies. There is no additional charge for using enhanced networking.
    * Use where you want good network performance.
  * Depending on your instance type, enhanced networking can be enabled using:
    * **Elastic Network Adapter \(ENA\)**, which supports network speeds of up to **100 Gbps** for supported instance types
    * Intel 82599 **Virtual Function \(VF\)** interface, which supports network speeds of up to **10Gbps** for supported instance types.
    * In most cases you probably what ENA over VF.
* **EFA - Elastic Fabric Adapter.** 
  * A network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing \(HPC\) and machine learning applications.
  * EFA provides lower and more consistent latency and higher throughput than the TCP transport traditionally used in cloud-based HPC systems.
  * EFA can use OS bypass. OS bypass enables HPC and machine learning applications to bypass the operating system kernel and to communicate directly with the EFA device. It makes it a lot faster with a lot lower latency. Not supported with Windows currently, only Linux.

