# Documentation

This README serves as an index to the documentation for each project carried out in the IT LAB.  
You can use this file to navigate to the README of each individual project.

# [Cisco](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md)
### 📚 Cisco Router Configuration – Table of Contents
- [🔄 Resetting the Cisco 2901 Router to Factory Defaults](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-resetting-the-cisco-2901-router-to-factory-defaults)
  - [🔧 Steps to Erase the Current Configuration](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-steps-to-erase-the-current-configuration)
- [⏩ Skipping Initial Configuration Dialog](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-skipping-initial-configuration-dialog)
- [📡 Disabling Automatic TFTP Configuration Fetch](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-disabling-automatic-tftp-configuration-fetch)
  - [🛑 Disable TFTP Configuration Fetch](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-disable-tftp-configuration-fetch)
- [🧩 Lab VLAN Setup](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-lab-vlan-setup)
- [📝 Defining VLANs and DHCP Pools](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-defining-vlans-and-dhcp-pools)
  - [📋 General VLAN DHCP Configuration Format](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-general-vlan-dhcp-configuration-format)
  - [💡 Example - Server VLAN](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-example---server-vlan)
  - [📦 DHCP Pool Configuration for All VLANs](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-dhcp-pool-configuration-for-all-vlans)
- [🔌 Configuring Interfaces: External (DHCP) and Internal (Trunk)](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-configuring-interfaces-external-dhcp-and-internal-trunk)
  - [🌐 External Interface (`GigabitEthernet0/0`)](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-external-interface-gigabitethernet00)
  - [🖧 Internal Trunk Interface (`GigabitEthernet0/1`)](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-internal-trunk-interface-gigabitethernet01)
- [🧱 Configuring Subinterfaces for Each VLAN](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-configuring-subinterfaces-for-each-vlan)
  - [❓ Why Do We Use 802.1Q Encapsulation?](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-why-do-we-use-8021q-encapsulation)
  - [📐 Subinterface Configuration Template](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-subinterface-configuration-template)
  - [🧪 Example - VLAN 1 (Management)](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-example---vlan-1-management)
  - [🧬 Subinterface Configuration for All VLANs](https://github.com/IT-LAB-UAI/Documentation/blob/main/Cisco/README.md#-subinterface-configuration-for-all-vlans)

# [DNS](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md)
### 📚 DNS Configuration – Table of Contents
- [🧰 Prerequisites](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-prerequisites)
- [📦 Installation](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-installation)
- [🔍 Verifying the Installation](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-verifying-the-installation)
- [📖 DNS Configuration Overview](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-dns-configuration-overview)
  - [🧮 Server-Side Configuration](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-server-side-configuration)
  - [📡 Router-Side Configuration](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-router-side-configuration)
- [🧑‍💻 Step 1: Configuring the DNS Service on the Host Machine](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-step-1-configuring-the-dns-service-on-the-host-machine)
  - [🔌 1. Set the Listening Interface](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-1-set-the-listening-interface)
  - [🎯 2. Restrict Listening Addresses](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-2-restrict-listening-addresses)
  - [🪵 3. Enable Logging for Debugging](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-3-enable-logging-for-debugging)
  - [✅ 4. Apply the Configuration](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-4-apply-the-configuration)
- [🔧 Step 2: Configuring the Router to Use the DNS Server](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-step-2-configuring-the-router-to-use-the-dns-server)
  - [🔗 1. Connect to the Cisco Router](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-1-connect-to-the-cisco-router)
  - [🔐 2. Enter Privileged EXEC Mode](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-2-enter-privileged-exec-mode)
  - [🔧 3. Enter Global Configuration Mode](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-3-enter-global-configuration-mode)
  - [📂 4. Modify the DHCP Pool](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-4-modify-the-dhcp-pool)
  - [🌐 5. Set the DNS Server for the DHCP Pool](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-5-set-the-dns-server-for-the-dhcp-pool)
  - [💾 6. Save and Exit](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-6-save-and-exit)
- [📝 Adding DNS Entries](https://github.com/IT-LAB-UAI/Documentation/blob/main/DNS/README.md#-adding-dns-entries)

# [LAB-Control](https://github.com/IT-LAB-UAI/LAB-Control/blob/develop/README.md)

# [NETBOOT.XYZ](https://github.com/IT-LAB-UAI/Documentation/blob/main/Netboot.xyz/README.md)
<details>
<summary>📚 Click to expand Table of Contents</summary>

### Table of Contents

- [🧰 Install and Setup](#-install-and-setup)
- [🐳 Docker Compose Setup](#-docker-compose-setup)
- [📝 Docker Compose File](#-docker-compose-file)
- [🚀 Deploy the Application](#-deploy-the-application)
- [🌐 Accessing the Web Interface and PXE Boot File](#-accessing-the-web-interface-and-pxe-boot-file)
  - [📥 Downloading the Default PXE Boot File](#-downloading-the-default-pxe-boot-file)
- [📚 PXE Boot Project](#-pxe-boot-project)

</details>

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
