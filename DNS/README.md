# DNS 
In this repository, we describe how the DNS was installed in the IT-LAB of Universidad Adolfo Ibáñez.  
First, it's important to note that we already have a Cisco router pre-configured, with VLANs and subnets properly defined.  
Documentation regarding the creation and configuration of the Cisco router can be found at [CISCO]().  

What we will describe below is how the DNS was installed and set up according to the required configuration.

### Prerequisites:
The DNS service will be implemented using the `dnsmasq` package.  
It’s important to consider which server will host the DNS service, as the corresponding subnets must be able to reach this server in order to access the DNS.

### Installation

To get started, we need to install the package that will allow us to set up the DNS service.  
This package is called `dnsmasq`, and it can be installed using Debian's package manager with the following command:

```bash
sudo apt update
sudo apt install dnsmasq
```

### Verifying the Installation

To confirm that `dnsmasq` was installed successfully, you can run the following command to check its version:

```bash
dnsmasq --version
```
Also you can check the status of the service by running the following command: 
```bash
sudo systemctl status dnsmasq
