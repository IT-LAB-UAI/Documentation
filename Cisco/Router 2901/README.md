# 🛠️ Cisco Router Configuration Guide – IT-LAB-UAI

Welcome to the official network configuration guide for **IT-LAB-UAI**, the IT laboratory of **Universidad Adolfo Ibáñez**. This document outlines the step-by-step process to configure a **Cisco 2901 router** for a multi-VLAN lab environment using trunking, subinterfaces, DHCP, and NAT.

This setup is designed to support a segmented network with distinct VLANs for management, servers, computers, services, and WiFi — all routed through a single trunk interface for simplicity and scalability.

Whether you're a student setting up your first Cisco lab or an assistant maintaining lab infrastructure, this guide will walk you through:

- Resetting and preparing the router
- Defining VLANs and DHCP pools
- Configuring internal and external interfaces
- Implementing subinterfaces with 802.1Q encapsulation
- Enabling NAT for internet access across all VLANs
- Troubleshooting NAT and routing issues

> 💡 **Note:** This guide assumes basic knowledge of Cisco IOS CLI and access to privileged mode on the router.


## 📚 Table of Contents


- [🛠️ Cisco Router Configuration Guide – IT-LAB-UAI](#-cisco-router-configuration-guide--it-lab-uai)  
- [🔌 Connecting to Cisco 2901 Router via `minicom`](#-connecting-to-cisco-2901-router-via-minicom)  
- [🧹 Resetting the Cisco 2901 Router to Factory Defaults](#-resetting-the-cisco-2901-router-to-factory-defaults)  
  - [🔄 Steps to Erase the Current Configuration](#-steps-to-erase-the-current-configuration)  
- [⏩ Skipping Initial Configuration Dialog](#-skipping-initial-configuration-dialog)  
- [📡 Disabling Automatic TFTP Configuration Fetch](#-disabling-automatic-tftp-configuration-fetch)  
  - [🔧 Disable TFTP Configuration Fetch](#-disable-tftp-configuration-fetch)  
- [🧩 Lab VLAN Setup](#-lab-vlan-setup)  
- [📝 Defining VLANs and DHCP Pools](#-defining-vlans-and-dhcp-pools)  
  - [📋 General VLAN DHCP Configuration Format](#-general-vlan-dhcp-configuration-format)  
  - [💡 Example - Server VLAN](#-example---server-vlan)  
  - [📦 DHCP Pool Configuration for All VLANs](#-dhcp-pool-configuration-for-all-vlans)  
- [🔌 Configuring Interfaces: External (DHCP) and Internal (Trunk)](#-configuring-interfaces-external-dhcp-and-internal-trunk)  
  - [🌐 External Interface (`GigabitEthernet0/0`)](#-external-interface-gigabitethernet00)  
  - [🖧 Internal Trunk Interface (`GigabitEthernet0/1`)](#-internal-trunk-interface-gigabitethernet01)  
- [🧱 Configuring Subinterfaces for Each VLAN](#-configuring-subinterfaces-for-each-vlan)  
  - [❓ Why Do We Use 802.1Q Encapsulation?](#-why-do-we-use-8021q-encapsulation)  
  - [📐 Subinterface Configuration Template](#-subinterface-configuration-template)  
  - [🧪 Example - VLAN 1 (Management)](#-example---vlan-1-management)  
  - [🧬 Subinterface Configuration for All VLANs](#-subinterface-configuration-for-all-vlans)  
- [🔒 Explanation of Access-Lists](#-explanation-of-access-lists)  
  - [📃 Standard Access-List 1 – Permit 192.168.0.0/16](#-standard-access-list-1--permit-19216800-16)  
  - [📡 Access-List 101 – DHCP and Inter-VLAN Restrictions](#-access-list-101--dhcp-and-inter-vlan-restrictions)  
  - [🛡️ Access-List 105 – Restrict VLAN 5 to External Access Only](#-access-list-105--restrict-vlan-5-to-external-access-only)  


✅

## 🔌 Connecting to Cisco 2901 Router via `minicom`

To establish a console connection with the Cisco 2901 router using `minicom`, follow these steps:

1. **Install `minicom`** (if not already installed):

   ```bash
   sudo apt-get update
   sudo apt-get install minicom
   ```

2. **Identify the serial port** connected to your console cable:

   ```bash
   dmesg | grep tty
   ```

   Look for something like `/dev/ttyUSB0` (for USB-to-serial adapters) or `/dev/ttyACM0`.

3. **Set up `minicom` with correct serial settings**:

   ```bash
   sudo minicom -s
   ```

   - Navigate to **"Serial port setup"**
   - Set **Serial Device** to your detected port (e.g., `/dev/ttyUSB0`)
   - Set **Bps/Par/Bits** to `9600 8N1`
   - Disable **Hardware Flow Control** and **Software Flow Control**
   - Save settings as default or save as a new configuration, then access it throw

   ```bash
   sudo minicom config_name
   ```

5. **Start `minicom`** with the selected configuration:

   ```bash
   sudo minicom
   ```

6. **Exit `minicom`** at any time by pressing:
   ```
   Ctrl + A, then Z → then X to exit
   ```

💡 If you encounter permission issues accessing `/dev/ttyUSB0`, add your user to the `dialout` group:

```bash
sudo usermod -aG dialout $USER
logout
```


## 🧹 Resetting the Cisco 2901 Router to Factory Defaults

Before starting any configuration, it's important to reset the router to its factory state. This ensures that no previous settings interfere with your new setup.

### 🔄 Steps to Erase the Current Configuration:

1. Enter privileged EXEC mode:
   ```shell
   enable
   ```

2. Erase the startup configuration:
   ```shell
   write erase
   ```

3. Delete the VLAN database file:
   ```shell
   delete vlan.dat
   ```

4. Reload the router to apply the reset:
   ```shell
   reload
   ```

   You may be prompted to confirm — type `yes` when asked.

> 💡**Note:** If the router has an enable password and you do not know it, consult Cisco's official documentation to perform a password recovery or manual factory reset.

## ⏩ Skipping Initial Configuration Dialog

After the router reloads, you'll be prompted with the following message:

```
Would you like to enter the initial configuration dialog? [yes/no]:
```

For now, we will skip this step. Respond with:

```shell
no
```

Then you'll be taken to the router prompt:

```
Press RETURN to get started!
Router>
```
## 📡 Disabling Automatic TFTP Configuration Fetch

After resetting the router, you might notice that it attempts to fetch a configuration file from a TFTP server. This happens because we erased the startup configuration, and by default, the router tries to retrieve a config from the network.

To prevent this behavior, we disable the automatic configuration service.

### 🔧 Disable TFTP Configuration Fetch:

```shell
configure terminal
no service config
exit
```

This command stops the router from trying to auto-load configuration files from a TFTP server during boot.



## 🧩 Lab VLAN Setup

For our lab environment, we will be using 5 VLANs, each assigned to a specific subnet:

| VLAN Name   | Subnet         | VLAN ID |
|-------------|----------------|---------|
| Management  | 192.168.1.0/24 | 1       |
| Servers     | 192.168.2.0/24 | 2       |
| Computers   | 192.168.3.0/24 | 3       |
| Services    | 192.168.4.0/24 | 4       |
| WiFi        | 192.168.5.0/24 | 5       |


## 📝 Defining VLANs and DHCP Pools

Before configuring the subinterfaces, we must define each VLAN and assign a DHCP pool so that connected devices receive IP addresses automatically.

### 📋 General VLAN DHCP Configuration Format:

```shell
enable
configure terminal

ip dhcp pool <VLAN_Name>
 network <VLAN_IP> <VLAN_MASK>
 default-router <VLAN_DEFAULT_ROUTER>
 dns-server <VLAN_DNS_SERVER>
```

We are using **Google's DNS (8.8.8.8)** as the default. This can be changed later if a local DNS server is configured.

### 💡 Example - Server VLAN:

```shell
ip dhcp pool Server
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 8.8.8.8
```

### 📦 DHCP Pool Configuration for All VLANs:

```shell
ip dhcp pool Management
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8

ip dhcp pool Server
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 8.8.8.8

ip dhcp pool Computer
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.1
 dns-server 8.8.8.8

ip dhcp pool Services
 network 192.168.4.0 255.255.255.0
 default-router 192.168.4.1
 dns-server 8.8.8.8

ip dhcp pool Wifi
 network 192.168.5.0 255.255.255.0
 default-router 192.168.5.1
 dns-server 8.8.8.8
```

Once all VLANs are defined and DHCP is set, y🔒 **Explanation of Access-Lists**ou can proceed with configuring the trunk and subinterfaces as shown in the previous step.


## 🔌 Configuring Interfaces: External (DHCP) and Internal (Trunk)

After setting up the VLANs and DHCP pools, we now configure the physical interfaces of the router.

### 🌐 External Interface (`GigabitEthernet0/0`)

This is the **uplink** interface that connects to the outside network (e.g. the university network). It will obtain its IP address via DHCP and is marked as the **NAT outside** interface.

```shell
enable
configure terminal

interface GigabitEthernet0/0
 ip address dhcp
 ip nat outside
 no shutdown
 exit
```

This interface will be used for NAT and external communication.

---

### 🖧 Internal Trunk Interface (`GigabitEthernet0/1`)

This interface connects to the switch and will carry all VLAN traffic using subinterfaces. We designate this as the **internal trunk** interface where all VLANs will converge.

```shell
enable
configure terminal

interface GigabitEthernet0/1
 no ip address
 no ip route-cache
 no shutdown
 exit
```

> 💡 We chose this approach to keep cable management simple and efficient. If desired, you could assign each VLAN to a different physical interface, but this adds complexity and usually doesn’t provide a performance benefit for most lab setups.


## 🧱 Configuring Subinterfaces for Each VLAN

Now that the DHCP pools, VLANs, and both the internal (`GigabitEthernet0/1`) and external (`GigabitEthernet0/0`) interfaces are set up, we need to create **subinterfaces** on the internal trunk port. Each subinterface will be mapped to a specific VLAN and will act as the **default gateway** for that VLAN.

### ❓ Why Do We Use 802.1Q Encapsulation?

We use **802.1Q (Dot1Q)** encapsulation to allow multiple VLANs to be carried over a single physical link (the trunk). This tagging standard allows Ethernet frames to carry VLAN identification information. On subinterfaces, we use:

```shell
encapsulation dot1Q <vlan-id> [native]
```

- `<vlan-id>` specifies which VLAN the subinterface is associated with.
- `native` is used to mark that VLAN as untagged (optional — typically used for VLAN 1 or a designated native VLAN).

---

### 📐 Subinterface Configuration Template

Each subinterface follows this pattern:

```shell
enable
configure terminal
interface GigabitEthernet0/1.<VLAN_ID>
 encapsulation dot1Q <VLAN_ID> native
 ip address <VLAN_GATEWAY_IP> <SUBNET_MASK>
 ip nat inside
 no ip route-cache
 exit
```

---

### 🧪 Example - VLAN 1 (Management)

```shell
interface GigabitEthernet0/1.1
 encapsulation dot1Q 1 native
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no ip route-cache
 exit
```

---

### 🧬 Subinterface Configuration for All VLANs

```shell
interface GigabitEthernet0/1.1
 encapsulation dot1Q 1 native
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no ip route-cache
 exit

interface GigabitEthernet0/1.2
 encapsulation dot1Q 2
 ip address 192.168.2.1 255.255.255.0
 ip nat inside
 no ip route-cache
 exit

interface GigabitEthernet0/1.3
 encapsulation dot1Q 3
 ip address 192.168.3.1 255.255.255.0
 ip nat inside
 no ip route-cache
 exit

interface GigabitEthernet0/1.4
 encapsulation dot1Q 4
 ip address 192.168.4.1 255.255.255.0
 ip nat inside
 no ip route-cache
 exit

interface GigabitEthernet0/1.5
 encapsulation dot1Q 5
 ip address 192.168.5.1 255.255.255.0
 ip nat inside
 no ip route-cache
 exit
```

Each subinterface is now ready to route packets between VLANs and provide gateway functionality. All of them are also marked as `ip nat inside` to support NAT

---

## 🔒 **Explanation of Access-Lists**

Access control lists (ACLs) are used in Cisco devices to control traffic based on IP addresses, protocols, and port numbers. Let’s walk through each ACL and its rules.

---

### ✅ **Standard Access-List 1**

```bash
access-list 1 permit 192.168.0.0 0.0.255.255
```

- **Type:** Standard ACL (1–99)
- **Function:** Allows IP traffic **from any host** in the IP range `192.168.0.0` to `192.168.255.255`.
- **Wildcard mask:** `0.0.255.255` means the last two octets can vary (like a `/16` subnet mask).
- **Usage:** This ACL is likely applied **inbound on an interface** to filter by source IP only.

---

### 🔐 **Extended Access-List 101**

These ACLs are **extended** (100–199), which can filter traffic based on source/destination IP, protocol (TCP, UDP, ICMP, etc.), and port numbers.

```bash
access-list 101 permit udp any eq bootpc any eq bootps
access-list 101 permit udp any eq bootps any eq bootpc
```

- **Purpose:** Allow **DHCP communication** (BOOTP is the underlying protocol of DHCP).
  - `bootpc` = UDP port **68** (client)
  - `bootps` = UDP port **67** (server)
- **Use:** These two lines allow DHCP **requests and responses** between any client and any server.

```bash
access-list 101 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 deny ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 deny ip 192.168.4.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 deny ip 192.168.5.0 0.0.0.255 192.168.1.0 0.0.0.255
```

- **Purpose:** Deny **any traffic** from the mentioned networks (`192.168.2.x` to `192.168.5.x`) **destined to** the `192.168.1.x` network.
- **Wildcard mask `0.0.0.255`** means it matches all hosts in a `/24` subnet.

```bash
access-list 101 permit ip any any
```

- **Purpose:** Allows **all other IP traffic** not denied above.
- **Note:** Since ACLs are processed top-down, this line acts as a **default allow**, making the list "deny some, permit the rest".

---

### 🔐 **Extended Access-List 105**

This ACL is similar to 101, but tailored specifically for traffic **originating from 192.168.5.x**.

```bash
access-list 105 permit udp any eq bootpc any eq bootps
access-list 105 permit udp any eq bootps any eq bootpc
```

- Again, allows **DHCP client-server communication** (UDP ports 67 and 68).

```bash
access-list 105 deny ip 192.168.5.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 105 deny ip 192.168.5.0 0.0.0.255 192.168.2.0 0.0.0.255
access-list 105 deny ip 192.168.5.0 0.0.0.255 192.168.3.0 0.0.0.255
access-list 105 deny ip 192.168.5.0 0.0.0.255 192.168.4.0 0.0.0.255
```

- Blocks any IP traffic from the `192.168.5.x` network to all the **other networks** listed (`192.168.1.x` through `192.168.4.x`).
- This isolates `192.168.5.x` from accessing other internal networks.

```bash
access-list 105 permit ip 192.168.5.0 0.0.0.255 any
```

- **Allows** any other traffic from `192.168.5.x` to the outside (e.g., Internet or unlisted destinations).
- Together with the previous `deny` rules, this creates a **"limited access" policy**: 192.168.5.x can reach the outside but not internal segments.


## 🧠 Summary of What’s Happening

| ACL | Purpose |
|-----|---------|
| `access-list 1` | Standard ACL to allow traffic from a large internal network range (`192.168.0.0/16`). |
| `access-list 101` | Extended ACL allowing DHCP traffic, **blocking internal segment `192.168.x.x` to 192.168.1.x**, and permitting everything else. |
| `access-list 105` | More restrictive; **denies 192.168.5.x** access to all other internal networks, allows only DHCP and general outbound traffic. |

These ACLs likely support a **segmented network** where:
- DHCP is permitted network-wide.
- `192.168.1.x` is a protected or core network.
- `192.168.5.x` is a more restricted or guest network.
- Other networks have partial access based on their role.
- 

## 🧾 Configuration Example

```plaintext
primary-router#show running-config 
Building configuration...

Current configuration : 4159 bytes
!
! Last configuration change at 21:12:47 UTC Wed Apr 16 2025
! NVRAM config last updated at 21:12:50 UTC Wed Apr 16 2025
! NVRAM config last updated at 21:12:50 UTC Wed Apr 16 2025
version 15.1
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname primary-router
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
no ipv6 cef
ip source-route
ip cef
!
!         
!
!
ip dhcp pool MANAGEMENT
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1 
 dns-server 8.8.8.8 
!
ip dhcp pool SERVER
 network 192.168.2.0 255.255.255.0
 bootfile netboot.xyz.kpxe
 next-server 192.168.2.2 
 default-router 192.168.2.1 
 dns-server 192.168.2.2 8.8.8.8 
!
ip dhcp pool COMPUTERS
 network 192.168.3.0 255.255.255.0
 bootfile netboot.xyz.kpxe
 next-server 192.168.2.2 
 default-router 192.168.3.1 
 dns-server 192.168.2.2 
!
ip dhcp pool SERVICE
 network 192.168.4.0 255.255.255.0
 dns-server 8.8.8.8 
 default-router 192.168.4.1 
!
ip dhcp pool WIFI
 network 192.168.5.0 255.255.255.0
 default-router 192.168.5.1 
 dns-server 8.8.8.8 
!
ip dhcp pool SERVERS
 bootfile undionly.kpxe
 next-server 192.168.2.2 
!
!
multilink bundle-name authenticated
!
!
crypto pki token default removal timeout 0
!
!
license udi pid CISCO2901/K9 sn FTX15498074
!
!
!
!
!         
!
!
!
interface Embedded-Service-Engine0/0
 no ip address
 shutdown
!
interface GigabitEthernet0/0
 ip address dhcp
 ip nat outside
 ip virtual-reassembly in
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 no ip address
 no ip route-cache
 duplex auto
 speed auto
!
interface GigabitEthernet0/1.1
 encapsulation dot1Q 1 native
 ip address 192.168.1.1 255.255.255.0
 ip access-group 101 in
 ip nat inside
 ip virtual-reassembly in
 no ip route-cache
!
interface GigabitEthernet0/1.2
 encapsulation dot1Q 2
 ip address 192.168.2.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 no ip route-cache
!
interface GigabitEthernet0/1.3
 encapsulation dot1Q 3
 ip address 192.168.3.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 no ip route-cache
!
interface GigabitEthernet0/1.4
 encapsulation dot1Q 4
 ip address 192.168.4.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 no ip route-cache
!
interface GigabitEthernet0/1.5
 encapsulation dot1Q 5
 ip address 192.168.5.1 255.255.255.0
 ip access-group 105 in
 ip nat inside
 ip virtual-reassembly in
 no ip route-cache
!
interface GigabitEthernet0/0/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface GigabitEthernet0/1/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface GigabitEthernet0/2/0
 no ip address
 shutdown 
 duplex auto
 speed auto
!
interface GigabitEthernet0/3/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip nat inside source list 1 interface GigabitEthernet0/0 overload
ip route 0.0.0.0 0.0.0.0 10.80.3.1 254
ip route 0.0.0.0 0.0.0.0 10.80.3.1 254
!
access-list 1 permit 192.168.0.0 0.0.255.255
access-list 101 permit udp any eq bootpc any eq bootps
access-list 101 permit udp any eq bootps any eq bootpc
access-list 101 deny   ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 deny   ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 deny   ip 192.168.4.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 deny   ip 192.168.5.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 permit ip any any
access-list 105 permit udp any eq bootpc any eq bootps
access-list 105 permit udp any eq bootps any eq bootpc
access-list 105 deny   ip 192.168.5.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 105 deny   ip 192.168.5.0 0.0.0.255 192.168.2.0 0.0.0.255
access-list 105 deny   ip 192.168.5.0 0.0.0.255 192.168.3.0 0.0.0.255
access-list 105 deny   ip 192.168.5.0 0.0.0.255 192.168.4.0 0.0.0.255
access-list 105 permit ip 192.168.5.0 0.0.0.255 any
!
!
!
control-plane
!
!
!
line con 0
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport input all
 transport output pad telnet rlogin lapb-ta mop udptn v120 ssh
 stopbits 1
line vty 0 4
 login
 transport input all
!
scheduler allocate 20000 1000
end
```
