# 🖥️ Ansible – Computer Setup

This setup contains the Ansible playbooks used to initialize the configuration of all lab machines after the base operating system has been installed.

As you may remember from the [PXE Boot Project](https://github.com/IT-LAB-UAI/Documentation/blob/main/PXE/README.md), the OS installation defined **placeholder credentials** and default user accounts. This post-installation setup is responsible for finalizing that configuration.

## 🧩 Purpose

The main objectives of this setup are:

- 🔐 Change the default passwords that were set during OS provisioning
- 👤 Configure or promote users (e.g., assign sudo/admin roles)
- 🧑‍💻 Ensure all machines are reachable via SSH and ready for future automation

These tasks are designed to be executed **once**, immediately after PXE-based OS installation, and are meant to bring machines into their initial, secured, and managed state.

