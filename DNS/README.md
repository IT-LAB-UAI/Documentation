# DNS 
In this repository, we describe how the DNS was installed in the IT-LAB of Universidad Adolfo Ibáñez.  
First, it's important to note that we already have a Cisco router pre-configured, with VLANs and subnets properly defined.  
Documentation regarding the creation and configuration of the Cisco router can be found at [CISCO]().  

What we will describe below is how the DNS was installed and set up according to the required configuration.

### Prerequisites:
The DNS service will be implemented using the `dnsmasq` package.  
It’s important to consider which server will host the DNS service, as the corresponding subnets must be able to reach this server in order to access the DNS.

