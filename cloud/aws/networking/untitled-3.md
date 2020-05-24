---
description: Virtual Private Cloud
---

# VPC

![](../../../.gitbook/assets/nat-instance-diagram.png)

Amazon VPC lets you provision a logically isolated section of the AWS cloud, where you can launch AWS resources in a virtual network that you define. You have complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.

You can easily customize the network configuration for your VPC. For example, you can create a public facing subnet for your web servers, that has access to the internet, and place your backend systems, such as databases or application servers, in a private subnet with no internet access. You can leverage multiple layers of security, including security groups and network access control lists, to help control access to EC2 instances in each subnet.

Additionally, you can create hardware VPN connection between your corporate datacenter and your VPC and leverage the AWS cloud as an extension of your corporate datacenter.

### VPC Features

* Launch instances into subnet of your choosing
* Assign custom IP address ranges in each subnet
* Create Internet Gateway and attach it to your VPC
* Much better security control over your AWS resources
* Instance security groups
* Subnet network access control lists \(ACLs\)

### VPC Peering

* Allows you to connect one VPC with another via a direct network route using private IP addresses.
* Instances behave as if they were on the same private network
* You can peer VPCs with other AWS accounts as well as with other VPCs in the same account.
* Peering is a star configuration: i.e. 1 central VPC peers with 4 others. _**No transitive peering.**_
* You can peer between regions.

### VPC tips

* Think of VPC as a logical datacenter in AWS.
* Consists of IGWs \(Internet Gateways or Virtual Private Gateways\), Route Tables, Network ACLs, Subnets and Security Groups
* 1 subnet = 1 availability zone
* Security groups are stateful
* Network ACLs are stateless
* No transitive peering is allowed between VPCs.
* When you create a VPC, a default Route Table, Network ACL and a default security group is created. No subnets or default Internet Gateway is created.
* Amazon always reserves 5 IP addresses within your subnet.
* You can only have 1 IGW per VPC.
* 5 VPCs per region by default \(can be increased\).

## Network Access Translation \(NAT\)

### NAT instances

You can use NAT instance in a public subnet in your VPC to enable instances in the private subnet to initiate outbound IPv4 traffic to the internet or other AWS services, but prevent the instances from receiving the inbound traffic initiated by someone on the internet.

#### Tips

* When creating a NAT instance, disable source/destination check on the instance.
* NAT instances must be a public subnet.
* There must be a route out of the private subnet to the NAT instance in order for this to work.
* The amount of traffic the NAT instance can support depends on the instance size.
* You can create high availability using Autoscaling Group., multiple subnets in different AZs, and a script to automate failover.
* NAT instance is behind the security group. 

### NAT Gateways

Similar to NAT instances, but managed by AWS. NAT Gateways are recommended, instead of NAT instances.

#### Tips

* Redundant inside the Availability Group.
* Preferred by the enterprise
* Starts of 5Gb/s and scales currently up to 45Gb/s.
* No maintenance \(patching\) needed.
* Not associated with security groups.
* Automatically assigned a public IP address.
* No need to disable source/destination checks.
* Remember to update your route tables.
* If you have resources in multiple AZs and they share one NAT Gateway, in the event that NAT Gateway's AZ is down, resources in the other AZ lose internet access. To create an AZ-independent architecture, create a NAT Gateway in each AZ and configure your routing to ensure that resources use the NAT Gateway in the same AZ.

## Security Groups

* All inbound traffic is blocked by default. Protocols / ports must be explicitly open.
* All outbound traffic is allowed..
* Changes to the security group take effect immediately.
* You can have any number of EC2 instances within a security group.
* You can have multiple security groups attached to to EC2 instance.
* Security groups are stateful.
* If you create an inbound rule allowing traffic in, that traffic is automatically allowed back out.
* You can not block a specific IP address using security group, instead use Access Control Lists.
* You can specify "allow" rules, but not "deny" rules.

## Network Access Control Lists \(NACLs\)

A network ACL is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.

#### Tips

* Your VPC automatically comes with default network ACL, and by default it allows all inbound and outbound traffic.
* You can create custom network ACLs. By default each custom NACL denies all inbound and outbound traffic until you add rules.
* Each subnet in your VPC must be associated with a NACL, the subnet is automatically associated with the default NACL.
* Block IP addresses using NACL, not Security Groups.
* You can associate NACL with multiple subnets; however a subnet can only be associated with one NACL at a time. When you associate a subnet with NACL, previous association is removed.
* NACLs contain a numbered list of rules that are evaluated in order, starting with the lowest numbered rule.
* NACLs have separate inbound and outbound rules, and each rule can either allow or deny traffic.
* NACLs are stateless: responses to the allowed inbound traffic are subject to the rules for outbound traffic \(and vice versa\).

## VPC Flow Logs

VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow Logs data is stored using CloudWatch. After you created the Flow Log, you can view and retrieve its data in the CloudWatch Logs.

Flow logs can be created at 3 levels:

* VPC
* Subnet
* Network interface

#### Tips

* You can not enable flow logs for VPCs that are peered with your VPC unless the peer VPC is in your account.
* You can not tag a flow log.
* After you created a flow log, you can not change it's configuration; for example, you can not associate a different IAM role with the flow log.

Not all traffic is monitored

* Traffic generated by instances when they contact the AWS DNS server, If you use your own DNS server, than all traffic to that DNS server is logged.
* Traffic generated by WIndows instance for Amazon Windows Activation not logged.
* Traffic to and from 169.254.169.254 IP for instance metadata is not logged.
* DHCP traffic is not logged.
* Traffic to the reserved IP address for the default VPC router is not logged.

## Bastion Host

A bastion host is a special purpose computer on a network specifically designed and configured to withstand attacks. The computer generally hosts a single application, for example, a proxy server, and all other services are removed or limited to reduce the threat to the computer. It's hardened in this manner primarily due to its location and purpose, which is either on the outside of a firewall on in a demilitarized zone \(DMZ\) and usually involves access from untrusted networks or computers.

## VPC Endpoint

A VPC Endpoint enables you to privately connect  your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring the Internet Gateway, NAT device, VPN connection or AWS DirectConnect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

Endpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components that allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic.

There are 2 types of VPC endpoints:

* Interface endpoints
* Gateway endpoints

Interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported service. Many AWS services are supported.

Following gateway endpoints are supported:

* S3
* DynamoDB



