# 📄 TFTP Server Documentation

This is the documentation of the TFTP server that is used on the LAB-IT-UAI. The functionality of this server is to provide lightweight file transfer capabilities over the network. It will be later used by other services — for example, PXE boot — inside the LAB.

This documentation provides a quick installation guide for the TFTP server using Peter's TFTP server `hpa-tftp`, as recommended in the Debian documentation.

## ⚙️ Installation and Setup

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

## 📁 What Goes in the TFTP Directory?

You might be wondering — what kind of files go in the TFTP directory?

Basically, any file placed here can be transferred over the network via TFTP. In our case, we’ll use this server to serve PXE boot files. That means in a later phase of the project, we’ll configure PXE to allow mass network booting of machines inside the LAB.

But that’s the work for another project — this setup lays the foundation for it.
