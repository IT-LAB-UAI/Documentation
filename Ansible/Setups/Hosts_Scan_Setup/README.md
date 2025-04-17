# 🕵️ Host Scan Playbook – Ansible

This Ansible playbook is designed to perform a **network scan on a specified subnet**, identifying reachable hosts and automatically generating an Ansible-compatible inventory file based on the discovered IPs.

If you’re setting up a complete lab deployment, **this is the first playbook you should run**, as it prepares the dynamic inventory required for future provisioning steps.


## ⚠️ Legal & Ethical Notice

This playbook uses [`nmap`](https://nmap.org/) to scan IPs within a given subnet. While `nmap` is a powerful and widely used tool, **network scanning may be considered intrusive or illegal if run in unauthorized environments.**

Please ensure that:

- You **own** or have **administrative rights** on the network you’re scanning.
- You understand any **local legal restrictions**, such as those outlined by law.
- You **do not use this playbook** on public or unapproved networks.


## 🧾 Chilean Legal Context – Cybersecurity Law

In Chile, network and cybersecurity practices are governed by the **Cybersecurity Framework Law (Ley N° 21.663)**, which came into effect on **January 1, 2025**. This law defines strict regulations for the protection of critical information systems and places legal obligations on both public and private entities that manage essential services.

Running automated tools like `nmap` to scan networks without explicit authorization could be interpreted as a legal violation — particularly in sensitive environments or on networks you do not own.

If you are operating within Chile, it is your responsibility to:

- Ensure you are **authorized** to perform scans on the target network.
- Avoid using this playbook on **public**, **external**, or **third-party** networks without consent.
- Understand the scope and limitations of your administrative access.

📄 You can read the full [legal text](https://www.bcn.cl/leychile/navegar?i=1202434) on the official Chilean **Library of Congress** website


## 🧠 What This Playbook Does

The playbook performs the following:

1. Scans a user-defined subnet using `nmap`.
2. Extracts active IP addresses that respond to ping or TCP connection checks.
3. Groups the discovered hosts into a dynamic inventory under a group name like `[lab_hosts]`.
4. Saves the inventory to a file (e.g., `hosts.ini`) for later use in other Ansible playbooks.

```bash
../Test
├── Outputs
│   └── Hosts_Scan_Output
└── Setups
    └── Hosts_Scan_Setuo
```
