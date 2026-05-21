# 🛡️ Comprehensive Security Audit & Penetration Testing Report: v1.1.0

<p align="center">
  <a href="https://github.com/pagarkristian">
    <img src="https://img.shields.io/badge/Auditor-Pagar%20Kristian%20Panjaitan-blue?style=for-the-badge&logo=github" alt="Profil Auditor" />
  </a>
  <img src="https://img.shields.io/badge/Lab_Version-v1.1.0-darkgreen?style=for-the-badge" alt="Versi Lab" />
  <img src="https://img.shields.io/badge/Target_OS-Linux%20Ubuntu-orange?style=for-the-badge&logo=ubuntu" alt="Sistem Operasi Target" />
  <img src="https://img.shields.io/badge/Status-PWNED%20/%20ROOTED-red?style=for-the-badge" alt="Status Eksploitasi" />
</p>

---

## 📌 Proyek Overview & Struktur Direktori

Dokumen ini berisi laporan teknis komprehensif mengenai audit keamanan dan simulasi eksploitasi yang dilakukan pada infrastruktur laboratorium lokal **PT. TechSecure Indonesia v1.1.0**. Simulasi ini dirancang untuk memvalidasi konfigurasi jaringan, menguji kerentanan aplikasi web, melakukan serangan kamus (*dictionary attack*), hingga mengeksploitasi sistem untuk mendapatkan bendera verifikasi (*CTF Flags*).

### 📐 Topologi Infrastruktur Jaringan Lab

```text
                  [ INTERNET / WAN ]
                           │
                           │ (DHCP / NAT)
                           ▼
                  ┌─────────────────┐
                  │   Kali Linux    │ (Node Auditor)
                  │  (IP: 10.216.27.10)│
                  └────────┬────────┘
                           │
                           │ (Interface: eth1)
                           │
                           ▼
  ================== [ HOST-ONLY NETWORK ] ==================
         (Subnet: 10.216.27.0/24 | Gateway: 10.216.27.1)
                           │
                           ├───► [ Gateway / Router MikroTik ]
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
     (User Flag)       Force (Foothold)  (www-data Shell)
```




On The Way
