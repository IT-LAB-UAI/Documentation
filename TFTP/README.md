# 📄 TFTP Server Documentation

This is the documentation of the TFTP server that is used on the LAB-IT-UAI. The functionality of this server is to provide lightweight file transfer capabilities over the network. It will be later used by other services — for example, PXE boot — inside the LAB.

This documentation provides a quick installation guide for the TFTP server using Peter's TFTP server `hpa-tftp`, as recommended in the Debian documentation.

## 📚 Table of Contents

- [🧰 Installation and Setup](#️installation-and-setup)
  - [🔄 1. Update the System](#1-update-the-system)
  - [📦 2. Install the TFTP Server Package](#2-install-the-tftp-server-package)
  - [📡 3. Check the TFTP Service Status](#3-check-the-tftp-service-status)
  - [🛠️ 4. Configure the TFTP Server](#4-configure-the-tftp-server)
  - [✅ 5. Restart the Service and Verify](#5-restart-the-service-and-verify)
- [📁 What Goes in the TFTP Directory?](#-what-goes-in-the-tftp-directory)
- [📚 PXE Boot Project](#--pxe-boot-project)

## 🧰 Installation and Setup

### 1. Update the System
First, make sure your package list is up to date and that the system is upgraded:
```bash
sudo apt update && sudo apt upgrade -y
```
### 2. Install the TFTP Server Package
Install the required packages for the TFTP server to work:
```bash
sudo apt install -y tftpd-hpa
```
### 3. Check the TFTP Service Status 
Once installed, the TFTP service should start automatically. You can verify its status with:
```bash
sudo systemctl status tftpd-hpa
```


### 4. Configure the TFTP Server

To configure the TFTP server, we need to edit its main configuration file located at:
```bash
/etc/default/tftpd-hpa
```
By default, the file might look something like this:
```bash
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/var/lib/tftpboot"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="--secure"
```
We’re going to stick with most of these defaults but clarify a few things:
-   The `TFTP_DIRECTORY` is where the server will serve files from. We'll set this to a sample directory for now.
    
-   The `TFTP_ADDRESS` is left as-is, which listens on all interfaces on the standard TFTP port (UDP 69).
    
-   The `TFTP_OPTIONS="--secure"` setting ensures the server is restricted to the specified directory for security purposes.

### 5. Restart the Service and Verify

After making any changes to the configuration file, it's important to restart the TFTP service to apply them:

```bash
sudo systemctl restart tftpd-hpa
```
Then, check the status to make sure everything is running correctly:
```bash
sudo systemctl status tftpd-hpa
```

## 📁 What Goes in the TFTP Directory?

You might be wondering — what kind of files go in the TFTP directory?

Basically, any file placed here can be transferred over the network via TFTP. In our case, we’ll use this server to serve PXE boot files. But that’s the work for another project — this setup lays the foundation for it.

## 📚  PXE Boot Project

To understand the full PXE setup, including how all components interact — such as DHCP, TFTP, iPXE, and netboot.xyz — check out the [PXE documentation](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md).

There, you’ll find a detailed, step-by-step breakdown of the entire PXE boot process and the system architecture behind it. It’s a great resource for seeing how everything works together to enable PXE booting in the LAB environment.
