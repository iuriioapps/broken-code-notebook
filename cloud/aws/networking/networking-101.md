# Networking 101

## DNS - Domain Name Service

* **Top Level Domain** - .com, .edu. These top level domains are controlled by IANA (Internet Assigned Number Authority) in a root zone database.
* **Registrar** - is an authority that can assign domain names directly under one or more top level domains. These domains are registered with InterNIC, a service of ICANN, which enforces uniqueness of domain names across the internet. Each domain name becomes registered in a central database known as WHOIS database.
* **SOA - Start of Authority Record** - the SOA record stores information about:
  * The name of the server that supplies the data for the zone
  * The administrator of the zone
  * The current version of the data file
  * The default number of seconds for the TTL file on resource records.
* **NS - Name Server Records**, they are used by Top Level Domain servers to direct traffic to the Content DNS server which contains the authoritative DNS records.
* User -> TLD -> NS -> SOA
* **A-Record** - an "A" record is a fundamental type of the DNS record. The "A" in the A-Record stands for "Address". The A-Record is used to translate the name of the domain to an IP address.
* **TTL** - the length that a DNS record is cached on either the resolving server or the users own local PC is equal to the value of "Time To Live" in seconds. The lower the TTL, the faster changes to the DNS records take to propagate throughout the internet.
* **CNAME** - Canonical Name - A CNAME can be used to resolve one domain name to another.
* **Alias Record** - Alias records are used to map resource record sets in your hosted zone to ELB, CloudFront distributions, or S3 buckets that are configured as websites. Alias records work as CNAME record in the you can map one DNS record name to another "target" DNS name.\
  Example: "demo.com" -> elb123.elb.aws.com\
  Key difference: a CNAME can't be used for naked domain names (zone apex record). You can't have a CNAME for http://example.com, it must be either an A-Record or an Alias record.

## CIDR - Classless Interdomain Routing

CIDR is a notation for describing blocks of IP addresses and is used heavily in various networking configurations. IP addresses contain 4 octets, each consisting of 8 bits, giving values between 0 and 255. The decimal value that comes after the slash is the number of bits consisting of the routing prefix. This in turn can be translated into netmask, and also designates how many available addresses are in the block:

* 10.0.1.0/28 => 00001010 00000000 00000001 0000\_\_\_\_ (4 empty bits)
* netmask: 255.255.255.240
* first IP: 10.0.1.1
* last IP: 10.0.14&#x20;
* available IPs: 16 ( $$2^4$$ )

