# 🌐 Netboot.xyz Setup Documentation


This is the documentation for setting up the **netboot.xyz** service on our IT-LAB-UAI. This service provides a quick and easy way to perform PXE booting over our network and install operating systems from a wide variety of Linux distributions.

For this functionality, we are going to use and configure the **netboot.xyz** service. This tool supports multiple ways to deliver PXE functionality within a network, and we will explore some of these options throughout this documentation.

To install **netboot.xyz**, we’ll follow the guidance provided by [linuxserver.io](https://docs.linuxserver.io/images/docker-netbootxyz/), which recommends using Docker to run the service as a containerized instance.

> ⚠️ **Note:** Before setting up netboot.xyz, you should already have a **TFTP server** installed and running.  
> Here you can find the documentation for setting up the [TFTP](https://github.com/IT-LAB-UAI/Documentation/blob/main/TFTP/README.md) server.


## ⚙️ Install and Setup

The first step in this installation is to ensure that Docker is installed and properly configured on your machine. If Docker is not yet installed, you can follow the official [Docker documentation](https://docs.docker.com/get-docker/) for detailed instructions specific to your operating system.

Once Docker is ready to use, we’ll create a working directory for the **netboot.xyz** service.

Inside this directory, we will organize the following structure:

- `assets/` – where static assets (like downloaded boot files) may be stored.
- `config/` – for configuration files.
- `docker-compose.yml` – to define and run the Docker container.

Your folder should look like this:
```bash
netboot_xyz/
├── assets
├── config
└── docker-compose.yml
```
> **Note:** `netboot_xyz/` is the root directory of this project.  
You can place it in your working directory or wherever it fits best in your setup.


## 🐳 Docker Compose Setup

After your directory is set up, the next step is to create the `docker-compose.yml` file. This file will define how to run the **netboot.xyz** container using Docker.

We’ll follow the basic setup recommended by [linuxserver.io](https://docs.linuxserver.io/images/docker-netbootxyz/), with a small but important difference:  
Instead of running the TFTP server from inside the container, we will use an **external TFTP server** — the one we previously set up — to serve the files.

This means:
- Netboot.xyz will generate the necessary PXE boot files.
- Our external TFTP server will handle transferring those files over the network.
- You should later **update your TFTP server's root directory** to point to where netboot.xyz stores the generated files (usually inside the `assets/` folder).

We'll define the actual contents of `docker-compose.yml` in the next step.
## 📝 Docker Compose File

Below is the `docker-compose.yml` file we’ll use to deploy the **netboot.xyz** container:

```yaml
---
services:
  netbootxyz:
    image: lscr.io/linuxserver/netbootxyz:latest
    container_name: netbootxyz
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - MENU_VERSION=1.9.9 #optional
      - PORT_RANGE=30000:30010 #optional
      - SUBFOLDER=/ #optional
      - NGINX_PORT=80 #optional
      - WEB_APP_PORT=3000 #optional
    volumes:
      - /root/netboot_xyz/config:/config
      - /root/netboot_xyz/assets:/assets #optional
    ports:
      - 3000:3000
      - 8080:80 #optional
    restart: unless-stopped
```

We leverage Docker's ability to run containerized applications, meaning there’s no need for additional manual service configuration. It simplifies setup and makes updates easier in the future.

A few important notes on this configuration:

-   The `volumes` section maps the `config` and `assets` folders from your **host machine** into the container.  
    The example here uses `/root/netboot_xyz/`, but you should update these paths to match your actual project directory.
    
-   Unlike the default file shown in [linuxserver.io’s documentation](https://docs.linuxserver.io/images/docker-netbootxyz/), we **do not map UDP port 69**.  
    That port is already being used by our standalone **TFTP server**, and mapping it here would cause a conflict.
    

This setup allows netboot.xyz to create and manage boot files, while the actual file transfer is handled by your external TFTP server.

## 🚀 Deploy the Application

Now that the `docker-compose.yml` file is ready, you can deploy the application using the following command:

```bash
docker compose up
```
This will:

-   Pull the latest **netboot.xyz** Docker image (if not already pulled).
    
-   Create and start the container based on the configuration.
    
-   Run the service interactively in your terminal.
    

If you prefer to run the container in the background (detached mode), use:
```bash
docker compose up -d
```
This will free up your terminal while keeping the service running in the background.

To check the logs and ensure everything is running smoothly, use:
```bash
docker logs netbootxyz
```
If the logs show no errors and the service is up, you're good to go!

## 🌐 Accessing the Web Interface and PXE Boot File

Once the container is running, you can access the **netboot.xyz** web menu by navigating to:
http://YourHostMachineIP:3000

From there, you’ll be able to:

- View and edit the **iPXE configuration files**
- Customize boot menus and entries
- Explore different PXE booting options available in netboot.xyz

### 📥 Downloading the Default PXE Boot File

As part of its standard setup, **netboot.xyz** provides a precompiled `kpxe` PXE boot file. This file can be fetched using:

```bash
wget https://boot.netboot.xyz/ipxe/netboot.xyz.kpxe
```
This file is ready to use out of the box and can be served by your TFTP server to PXE-boot clients.

> ⚠️ **Note:** The downloaded `kpxe` file is **precompiled**. This means it cannot be customized. If you want to create a fully customized PXE experience, use the configuration tools available through the netboot.xyz web interface instead.

## 📚  PXE Boot Project

To understand the full PXE setup, including how all components interact — such as DHCP, TFTP, iPXE, and netboot.xyz — check out the [PXE documentation](#pxe-documentation).

There, you’ll find a detailed, step-by-step breakdown of the entire PXE boot process and the system architecture behind it. It’s a great resource for seeing how everything works together to enable PXE booting in the LAB environment.
