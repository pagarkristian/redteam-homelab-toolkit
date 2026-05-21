# 🛡️ Comprehensive Security Audit & Penetration Testing Report: v1.0.0

<p align="center">
  <a href="https://github.com/pagarkristian">
    <img src="https://img.shields.io/badge/Auditor-Pagar%20Kristian%20-blue?style=for-the-badge&logo=github" alt="Auditor Profile" />
  </a>
  <img src="https://img.shields.io/badge/Lab_Version-v1.0.0-darkgreen?style=for-the-badge" alt="Lab Version" />
  <img src="https://img.shields.io/badge/Target_OS-Linux%20Ubuntu-orange?style=for-the-badge&logo=ubuntu" alt="Target OS" />
  <img src="https://img.shields.io/badge/Status-PWNED%20/%20ROOTED-red?style=for-the-badge" alt="Exploit Status" />
</p>

---

##  Project Overview & Directory Structure

This document serves as a comprehensive technical writeup detailing the security audit and exploitation simulation performed on the **PT. TechSecure Indonesia v1.1.0** local staging infrastructure. This assessment was conducted to validate network configurations, discover web application vulnerabilities, execute dictionary attacks, and chain exploits to achieve full system compromise (acquiring verification flags).

###  Network Infrastructure Topology

```text
                  [ INTERNET / WAN ]
                           │
                           │ (DHCP / NAT)
                           ▼
                  ┌─────────────────┐
                  │   Kali Linux    │ (Auditor Node)
                  │  (IP: 10.216.27.10)│
                  └────────┬────────┘
                           │
                           │ (Interface: eth1)
                           │
                           ▼
  ================== [ HOST-ONLY NETWORK ] ==================
         (Subnet: 10.216.27.0/24 | Gateway: 10.216.27.1)
                           │
                           ├───► [ Gateway / MikroTik Router ]
                           │          (IP: 10.216.27.1)
                           │
                           ▼
              ┌─────────────────────────┐
              │  Ubuntu Target Server   │ (TechSecure Portal)
              │    (IP: 10.216.27.100)  │
              └────────────┬────────────┘
                           │
         ┌─────────────────┼─────────────────┐
         ▼                 ▼                 ▼
   [ Port 21: FTP ]  [ Port 22: SSH ]  [ Port 80: HTTP ]
     (vsftpd 3.0.5)   (OpenSSH 9.6p1)   (Apache 2.4.58)
         │                 │                 │
         ▼                 ▼                 ▼
  Anonymous Login     Hydra Brute      SQLi & RCE
    (User Flag)      Force (Foothold)  (www-data Shell)

```

---

## Phase 1: Network Configuration & Interface Verification
The assessment initiated with aligning the network configuration on the Auditor Node (Kali Linux) to ensure it shared the same subnet as the target server over the private Host-Only segment.


### 1. Host Interface Alignment
The interface configuration file located at /etc/network/interfaces was modified to configure a static allocation using the Nano text editor:

```bash
sudo nano /etc/network/interfaces
```


### Network Configuration Blueprint `(/etc/network/interfaces)`:

```YAML
# Internet WAN (NAT)
auto eth0
iface eth0 inet dhcp

# Internal DMZ Segment (Host-Only)
auto eth1
iface eth1 inet static
    address 10.216.27.10
    netmask 255.255.255.0
    gateway 10.216.27.1
```


### To apply the changes directly into kernel space without bouncing the physical device hardware, the networking system daemon was restarted:

```bash
sudo systemctl restart networking
```


## 2. IP Address & DNS Resolution Verification
The ip a utility was executed to confirm that the interface eth1 successfully bound the required static IP addres


```bash
ip a
```

### The upstream DNS nameservers were verified inside /etc/resolv.conf to guarantee domain mapping consistency within the local virtual network environment:

```bash
sudo nano /etc/resolv.conf
```

## 3. ICMP Route Validation (Ping Testing)
ICMP Echo Request sequences were issued targeting the MikroTik Gateway Router (10.216.27.1) and the Ubuntu Target Server (10.216.27.100) to test node-to-node connectivity:


# Testing connection to the Router Gateway
```bash
ping -c 3 10.216.27.1
```


# Testing connection to the Target Web Server
```bash
ping -c 3 10.216.27.100
```

# Analysis: Both hosts responded cleanly with a metric of 0% packet loss, proving the physical-to-virtual routing lane was highly stable and ready for exploitation.
































































































