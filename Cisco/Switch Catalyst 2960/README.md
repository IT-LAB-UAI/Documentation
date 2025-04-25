# 🛠️ Cisco Switch Configuration Guide – IT-LAB-UAI
Welcome to the official network configuration guide for IT-LAB-UAI, the IT laboratory of Universidad Adolfo Ibáñez. This document outlines the step-by-step process to configure a Cisco Catalyst 2960 switch.
This guide asume that the router is allready configured as is said on the [Router 2901 README.md](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md).

Whether you're a student setting up your first Cisco lab or an assistant maintaining lab infrastructure, this guide will walk you through:

Resetting and preparing the switch
Replicate the VLANs configured on the router
Configuring interfaces to be assigned for each VLAN
💡 Note: This guide assumes basic knowledge of Cisco IOS CLI and access to privileged mode on the switch.
⚠️ Note: The VLAN IDs and IP address ranges should match the router's configuration. This example assumes 5 VLANs already defined on the router:
VLAN 1 (Management), VLAN 2 (Server), VLAN 3 (Computers), VLAN 4 (Service), VLAN 5 (WiFi).

## 📚 Table of Contents

- [🛠️ Cisco Router Configuration Guide – IT-LAB-UAI](#-cisco-router-configuration-guide--it-lab-uai)  
- [🔌 Connecting to Cisco Catalyst 2960 Switch via `minicom`](#-connecting-to-cisco-catalyst-2960-switch-via-minicom)
- [🧰 Switch Configuration – Catalyst 2960](#-switch-configuration--catalyst-2960)
  - [🔧 Step 1: Enter Privileged EXEC Mode](#-step-1-enter-privileged-exec-mode)
  - [📝 Step 2: Enter Global Configuration Mode](#-step-2-enter-global-configuration-mode)
  - [📛 Step 3: Set a Hostname for the Switch](#-step-3-set-a-hostname-for-the-switch)
  - [🪪 Step 4: Create the VLANs](#-step-4-create-the-vlans)
  - [🌐 Step 5: Configure the Trunk Port to the Router](#-step-5-configure-the-trunk-port-to-the-router)
  - [🪛 Step 6: Configure Access Ports for End Devices](#-step-6-configure-access-ports-for-end-devices)
  - [🌐 Step 7: (Optional) Set Up Management IP on VLAN 1](#-step-7-optional-set-up-management-ip-on-vlan-1)
  - [🌍 Step 8: Set the Default Gateway for Switch](#-step-8-set-the-default-gateway-for-switch)
  - [💾 Step 9: Save the Configuration](#-step-9-save-the-configuration)
  - [🧪 Step 10: Test Connectivity](#-step-10-test-connectivity)

✅

## 🔌 Connecting to Cisco Ctalyst 2960 via `minicom`

To establish a console connection  using `minicom`, follow these steps:

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
   sudo minicom <config_name>
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

Of course! Here's a **step-by-step configuration guide** to set up a **Cisco Catalyst switch** to work with your router in a **Router-on-a-Stick** setup using VLANs. It assumes you're using a switch like the **Catalyst 2960** and connecting it to your **Cisco 2901 router** via `GigabitEthernet0/1`.

---


## Enter Privileged EXEC Mode

```bash
Switch> enable
```

## 📝 Step 2: Enter Global Configuration Mode

```bash
Switch# configure terminal
```

## 📛 Step 3: Set a Hostname for the Switch

```bash
Switch(config)# hostname access-switch-1
```

## 🪪 Step 4: Create the VLANs

```bash
access-switch-1(config)# vlan 1
access-switch-1(config-vlan)# name Management
access-switch-1(config-vlan)# exit

access-switch-1(config)# vlan 2
access-switch-1(config-vlan)# name Server
access-switch-1(config-vlan)# exit

access-switch-1(config)# vlan 3
access-switch-1(config-vlan)# name Computers
access-switch-1(config-vlan)# exit

access-switch-1(config)# vlan 4
access-switch-1(config-vlan)# name Service
access-switch-1(config-vlan)# exit

access-switch-1(config)# vlan 5
access-switch-1(config-vlan)# name WiFi
access-switch-1(config-vlan)# exit
```
## 🌐 Step 5: Configure the Trunk Port to the Router

> The router is connected to `GigabitEthernet0/1`

```bash
access-switch-1(config)# interface GigabitEthernet0/1
access-switch-1(config-if)# switchport trunk encapsulation dot1q
access-switch-1(config-if)# switchport mode trunk
access-switch-1(config-if)# switchport trunk native vlan 1
access-switch-1(config-if)# exit
```

## 🪛 Step 6: Configure Access Ports for End Devices

### VLAN 2 – Servers (`G0/2 to G0/4`)
```bash
access-switch-1(config)# interface range GigabitEthernet0/2 - 0/4
access-switch-1(config-if-range)# switchport mode access
access-switch-1(config-if-range)# switchport access vlan 2
access-switch-1(config-if-range)# exit
```
### VLAN 3 – Computers (`G0/5 to G0/7`)
```bash
access-switch-1(config)# interface range GigabitEthernet0/5 - 0/7
access-switch-1(config-if-range)# switchport mode access
access-switch-1(config-if-range)# switchport access vlan 3
access-switch-1(config-if-range)# exit
```
### VLAN 4 – Service Equipment (`G0/8 to G0/9`)
```bash
access-switch-1(config)# interface range GigabitEthernet0/8 - 0/9
access-switch-1(config-if-range)# switchport mode access
access-switch-1(config-if-range)# switchport access vlan 4
access-switch-1(config-if-range)# exit
```
### VLAN 5 – WiFi Devices (`G0/10 to G0/11`)
```bash
access-switch-1(config)# interface range GigabitEthernet0/10 - 0/11
access-switch-1(config-if-range)# switchport mode access
access-switch-1(config-if-range)# switchport access vlan 5
access-switch-1(config-if-range)# exit
```
## 🌐 Step 7: (Optional) Set Up Management IP on VLAN 1

```bash
access-switch-1(config)# interface Vlan1
access-switch-1(config-if)# ip address 192.168.1.2 255.255.255.0
access-switch-1(config-if)# no shutdown
access-switch-1(config-if)# exit
```
## 🌍 Step 8: Set the Default Gateway for Switch

```bash
access-switch-1(config)# ip default-gateway 192.168.1.1
```

---

## 💾 Step 9: Save the Configuration

```bash
access-switch-1# end
access-switch-1# write memory
```
## 🧪 Step 10: Test Connectivity

1. Connect a PC to an access port (e.g. G0/5 in VLAN 3).
2. Set the PC's IP to something like `192.168.3.10`.
3. From the PC, test:
   ```bash
   ping 192.168.3.1       # Router subinterface for VLAN 3
   ping 192.168.1.1       # Router gateway (VLAN 1)
   ping 8.8.8.8           # Test Internet connectivity
   ```
