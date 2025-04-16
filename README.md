# Documentation

This README serves as an index to the documentation for each project carried out in the IT LAB.  
You can use this file to navigate to the README of each individual project.

# [Cisco](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md)
## 📚 Cisco Router Configuration – Table of Contents

- [🔄 Resetting the Cisco 2901 Router to Factory Defaults](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#resetting-the-cisco-2901-router-to-factory-defaults)
  - [🔧 Steps to Erase the Current Configuration](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#steps-to-erase-the-current-configuration)
- [⏩ Skipping Initial Configuration Dialog](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#skipping-initial-configuration-dialog)
- [📡 Disabling Automatic TFTP Configuration Fetch](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#disabling-automatic-tftp-configuration-fetch)
  - [🛑 Disable TFTP Configuration Fetch](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#disable-tftp-configuration-fetch)
- [🧩 Lab VLAN Setup](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#lab-vlan-setup)
- [📝 Defining VLANs and DHCP Pools](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#defining-vlans-and-dhcp-pools)
  - [📋 General VLAN DHCP Configuration Format](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#general-vlan-dhcp-configuration-format)
  - [💡 Example - Server VLAN](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#example---server-vlan)
  - [📦 DHCP Pool Configuration for All VLANs](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#dhcp-pool-configuration-for-all-vlans)
- [🔌 Configuring Interfaces: External (DHCP) and Internal (Trunk)](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#configuring-interfaces-external-dhcp-and-internal-trunk)
  - [🌐 External Interface (`GigabitEthernet0/0`)](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#external-interface-gigabitethernet00)
  - [🖧 Internal Trunk Interface (`GigabitEthernet0/1`)](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#internal-trunk-interface-gigabitethernet01)
- [🧱 Configuring Subinterfaces for Each VLAN](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#configuring-subinterfaces-for-each-vlan)
  - [❓ Why Do We Use 802.1Q Encapsulation?](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#why-do-we-use-8021q-encapsulation)
  - [📐 Subinterface Configuration Template](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#subinterface-configuration-template)
  - [🧪 Example - VLAN 1 (Management)](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#example---vlan-1-management)
  - [🧬 Subinterface Configuration for All VLANs](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#subinterface-configuration-for-all-vlans)

# [DNS](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md)

# [LAB-Control](https://github.com/IT-LAB-UAI/LAB-Control/blob/develop/README.md)

# [NETBOOT.XYZ](https://github.com/IT-LAB-UAI/Documentation/blob/main/Netboot.xyz/README.md)

# [TFTP](https://github.com/IT-LAB-UAI/Documentation/blob/main/TFTP/README.md)

# [PXE](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md)
### 📚 PXE Boot System Documentation – Table of Contents

- [🧩 PXE Boot System](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-pxe-boot-system)
- [🧱 Prerequisites](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-prerequisites)
- [📝 What This Guide Covers](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-what-this-guide-covers)
- [🖥️ System Architecture](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#️-system-architecture)
- [🔄 System Workflow](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-system-workflow)
- [🧭 PXE Bootfile Configuration on the Server](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-walkthrough-pxe-bootfile-configuration-on-the-server)
  - [📌 1. Define Your PXE Bootfile](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-1-define-your-pxe-bootfile)
  - [📂 2. Choose a Directory for Your PXE Files](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-2-choose-a-directory-for-your-pxe-files)
  - [⚙️ 3. Update the TFTP Server Configuration](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-3-update-the-tftp-server-configuration)
  - [🔄 4. Restart the TFTP Service](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-4-restart-the-tftp-service)
  - [✅ 5. Verify TFTP Service Is Running Correctly](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-5-verify-tftp-service-is-running-correctly)
- [📝 Configuring Unattended Installation via `preseed.cfg`](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-configuring-unattended-installation-via-preseedcfg)
- [🌐 Exposing the `preseed.cfg` File to the Network](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-exposing-the-preseedcfg-file-to-the-network)
- [🔧 Finalizing the Server-Side: What's Next?](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-finalizing-the-server-side-whats-next)
- [📡 Configure the Cisco Router](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-configure-the-cisco-router)
  - [⚙️ Cisco Router Configuration](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#️-cisco-router-configuration)
- [🖥️ Final Step: Host Machine Configuration](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#️-final-step-host-machine-configuration)
  - [⚙️ BIOS/UEFI Settings](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#️-biosuefi-settings)
  - [⌨️ Booting into PXE](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#️-booting-into-pxe)
  - [📥 Installing Debian via Preseed](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md#-installing-debian-via-preseed)
---

Inside each directory, you will find information specific to that project, as they may vary in type and scope.
