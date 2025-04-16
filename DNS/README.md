# 🛠️ DNS Configuration Guide – IT-LAB-UAI
In this repository, we describe how the DNS was installed in the IT-LAB of Universidad Adolfo Ibáñez.  
First, it's important to note that we already have a Cisco router pre-configured, with VLANs and subnets properly defined.  
Documentation regarding the creation and configuration of the Cisco router can be found at [CISCO](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md).  

What we will describe below is how the DNS was installed and set up according to the required configuration.

## 📚 Table of Contents
- [🧰 Prerequisites](#-prerequisites)
- [📦 Installation](#-installation)
- [🔍 Verifying the Installation](#-verifying-the-installation)
- [📖 DNS Configuration Overview](#-dns-configuration-overview)
  - [🖥️ Server-Side Configuration](#-server-side-configuration)
  - [📡 Router-Side Configuration](#-router-side-configuration)
- [⚙️ Step 1: Configuring the DNS Service on the Host Machine](#-step-1-configuring-the-dns-service-on-the-host-machine)
  - [🔌 1. Set the Listening Interface](#-1-set-the-listening-interface)
  - [🎯 2. Restrict Listening Addresses](#-2-restrict-listening-addresses)
  - [🪵 3. Enable Logging for Debugging](#-3-enable-logging-for-debugging)
  - [✅ 4. Apply the Configuration](#-4-apply-the-configuration)
- [🔧 Step 2: Configuring the Router to Use the DNS Server](#-step-2-configuring-the-router-to-use-the-dns-server)
  - [🔗 1. Connect to the Cisco Router](#-1-connect-to-the-cisco-router)
  - [🔐 2. Enter Privileged EXEC Mode](#-2-enter-privileged-exec-mode)
  - [🛠️ 3. Enter Global Configuration Mode](#-3-enter-global-configuration-mode)
  - [📂 4. Modify the DHCP Pool](#-4-modify-the-dhcp-pool)
  - [🌐 5. Set the DNS Server for the DHCP Pool](#-5-set-the-dns-server-for-the-dhcp-pool)
  - [💾 6. Save and Exit](#-6-save-and-exit)
- [📝 Adding DNS Entries](#-adding-dns-entries)



### 🧰 Prerequisites:
The DNS service will be implemented using the `dnsmasq` package.  
It’s important to consider which server will host the DNS service, as the corresponding subnets must be able to reach this server in order to access the DNS.

### 📦 Installation

To get started, we need to install the package that will allow us to set up the DNS service.  
This package is called `dnsmasq`, and it can be installed using Debian's package manager with the following command:

```bash
sudo apt update
sudo apt install dnsmasq
```

### 🔍 Verifying the Installation

To confirm that `dnsmasq` was installed successfully, you can run the following command to check its version:

```bash
dnsmasq --version
```
Also you can check the status of the service by running the following command: 
```bash
sudo systemctl status dnsmasq
```

### 📖 DNS Configuration Overview

Now that the `dnsmasq` service is installed and running, we need to apply two configurations:
1. **Server-side** – on the machine where the DNS service is hosted.
2. **Router-side** – to redirect DNS queries from the network to our DNS server.

> **Note:** This setup assumes that an external service is already providing DHCP to the network.  
Our DNS server will **not** act as a DHCP server — it will solely handle DNS resolution.

#### 🖥️ Server-Side Configuration

On the host machine, we will configure the DNS service to listen on a specific network interface and resolve queries sent to it.

#### 📡 Router-Side Configuration

We will update the router’s VLAN settings to specify our DNS server as the default DNS for all clients in the network.  

While it’s technically possible to configure each machine individually to use the DNS server, this approach is inefficient, tedious, and defeats the purpose of having a centralized DNS.  
Therefore, we will proceed with configuring DNS at the **network level**, directly through the router.

---


### ⚙️ Step 1: Configuring the DNS Service on the Host Machine

With the `dnsmasq` service already running, we now need to configure it properly.  
The main configuration file is located at `/etc/dnsmasq.conf`.

Open this file in your preferred text editor and make the following changes:

---

#### 🔌 1. Set the Listening Interface

First, specify the network interface(s) on which the DNS service should listen.  
Uncomment the `interface=` line and set it to your desired interface.  
In our case, the machine is using `enp2s0`, so the line should look like:

`interface=enp2s0`

---

#### 🎯 2. Restrict Listening Addresses

To restrict which addresses the DNS server will respond to, uncomment and edit the `listen-address=` line.

You should include:
- `127.0.0.1` to allow the local machine to use the DNS
- The machine’s local IP address (e.g., `192.168.2.2`)

The line should look like this:
`listen-address=127.0.0.1,192.168.2.2`

This ensures that the DNS service only responds to queries directed at those specific addresses and ignores others.

---

#### 🪵 3. Enable Logging for Debugging

To enable query logging for troubleshooting, uncomment the following line:

`log-queries`

Then, specify the log file path by adding:
`log-facility=/var/log/dnsmasq.log`


Make sure this file exists before restarting the service. You can create it using:

```bash
sudo touch /var/log/dnsmasq.log
```
---
#### ✅ 4. Apply the Configuration

After making these changes, save the file and restart the service for the changes to take effect:

```bash
sudo systemctl restart dnsmasq
```
You can verify the status of the service with:
```bash
sudo systemctl status dnsmasq
```
Additionally, you can monitor the logs for DNS activity using:
```bash
cat /var/log/dnsmasq.log
```

### 🔧 Step 2: Configuring the Router to Use the DNS Server

To complete the setup, we need to configure the router to direct DNS queries to our new DNS server.  
This configuration will be done at the **VLAN (DHCP pool)** level, allowing each VLAN to use the same or different DNS servers depending on your needs.

---

#### 🔗 1. Connect to the Cisco Router

You must first connect to the Cisco router via console.  
There is a detailed guide on how to do this in the **CISCO documentation** found in the Documentation repository.

Once connected, you will be greeted with a prompt like:
`
Router-1>
`

> **Note:** The router name may vary depending on your setup. In the Cisco documentation, we named it `Router-1`.

---
#### 🔐 2. Enter Privileged EXEC Mode
To enter privileged mode, type:

`
Router-1>enable
`

You will be prompted to enter the router's secret password. After entering it, you’ll be in EXEC mode.

---
#### 🛠️ 3. Enter Global Configuration Mode

Once inside, type:

`
Router-1# configure terminal
`

---
#### 📂 4. Modify the DHCP Pool
Next, you need to access the DHCP pool configuration for the VLAN you want to update. In this example, we are using the pool named SERVICIOS:

`
Router-1(config)# ip dhcp pool SERVICIOS
`

---
#### 🌐 5. Set the DNS Server for the DHCP Pool
Now, update the DNS server addresses. Previously, the DNS servers might have been set to 8.8.8.8 (primary) and 8.8.4.4 (secondary). We will now set our custom DNS server (e.g. 192.168.2.2) as the primary, and retain 8.8.8.8 as the secondary:

`
Router-1(dhcp-config)# dns-server 192.168.2.2 8.8.8.8
`

#### 💾 6. Save and Exit
After making the changes, exit configuration mode and (optionally) save the changes to startup config: 

`Router-1(dhcp-config)#exit`

`Router-1(config)# exit` 

`Router-1# write memory`

 write memory is optional but recommended — it saves your changes so they persist after a reboot

### 📝 Adding DNS Entries

Now your service is configured to run on its own.  
If you want to add custom entries to the `dnsmasq` DNS service, you can do so by editing the `/etc/hosts` file on the machine running the service.  
By modifying this file, you are creating mappings between IP addresses and hostnames.

> **Reminder:** Always restart the `dnsmasq` service after making changes to the configuration or the `/etc/hosts` file.

There is also an example configuration file included in this directory named `dnsmasq.conf`.  
This file serves as a reference for how the `/etc/dnsmasq.conf` file on your machine should look.

There will also be an example of the `/etc/hosts` file in this directory,  
which you can review to see how IPs and hostnames are linked for the DNS server to resolve.



