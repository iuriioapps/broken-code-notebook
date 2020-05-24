# DirectConnect

## Direct Connect

AWS Direct Connect is a cloud service solution that makes it easy to establish a dedicated network connection from your premises to AWS. Using AWS Direct Connect you can establish private connectivity between AWS and your datacenter, office, or colocation environment, which in many cases can reduce your network costs, increase bandwidth throughput and provide a more consistent network experience than internet-based connection.

**BGP - Border Gateway Protocol** - is a standardized exterior gateway protocol designed to exchange routing and reachability information. It's used by AWS Direct Connect.

### Steps to setting up a Direct Connect

* Create a virtual interface in the Direct Connect console. This a **PUBLIC Virtual Interface**.
* Go to the VPC console and then to VPN connections. Create a customer gateway.
* Create a Virtual Private Gateway.
* Attach a Virtual Private Gateway to the desired VPC.
* Select VPN Connections and create new VPN Connection.
* Select the Virtual Private Gateway and the Customer Gateway.
* Once the VPN is available, set up the VPN on the customer gateway firewall.

{% embed url="https://www.youtube.com/watch?v=dhpTTT6V1So" %}



