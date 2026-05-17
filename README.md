# 🏛️ Enterprise Homelab Core: Architectural Evolution & Hardening

<div align="center">
  <img src="https://img.shields.io/badge/Infrastructure_Type-Enterprise_SMB_Core-1F4068?style=for-the-badge&logo=proxmox&logoColor=white" alt="Type" />
  <img src="https://img.shields.io/badge/Core_Router-MikroTik_RouterOS-0052CC?style=for-the-badge&logo=mikrotik&logoColor=white" alt="Router" />
  <img src="https://img.shields.io/badge/Production_OS-Ubuntu_Server_24.04_LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="OS" />
  <img src="https://img.shields.io/badge/Security_Framework-DevSecOps_/_SDLC-D32F2F?style=for-the-badge&logo=target&logoColor=white" alt="Framework" />
</div>

---

## 🌟 1. Executive Overview

> [!IMPORTANT]
> **Enterprise Homelab Core** adalah sebuah proyek cetak biru (*living blueprint*) arsitektur jaringan tingkat perusahaan skala *Small-to-Medium Business* (SMB). Proyek ini mensimulasikan siklus hidup penuh infrastruktur teknologi informasi di dunia nyata—dimulai dari fase rentan operasional, proses audit keamanan, hingga pengerasan sistem (*hardening*) menyeluruh.

### 🎯 Pilar Utama Proyek:
* 🔹 **Infrastructure as Code (IaC) Approach:** Seluruh konfigurasi sistem dan perangkat jaringan dikelola, versi-kontrol, dan didokumentasikan layaknya kode perangkat lunak.
* 🔹 **Secured Lifecycle Execution:** Menerapkan siklus **SDLC** (*Software Development Lifecycle*) untuk melacak evolusi keamanan infrastruktur dari versi lawas ke versi stabil.
* 🔹 **Realistic Simulation:** Mengintegrasikan sistem operasi server tangguh (Ubuntu LTS) dan sistem perutean standar industri (MikroTik) dalam satu perimeter lab terkendali.

---

## 🛠️ 2. Core Network Architecture & Topology Matrix

Berikut adalah matriks interkoneksi logis dan alokasi perimeter segmen jaringan laboratorium:

| Komponen Sistem | Antarmuka Perangkat | Alokasi IP Address | Default Gateway | Fungsi & Peran Jaringan |
| :--- | :--- | :--- | :--- | :--- |
| 🛡️ **Core Router (MikroTik)** | `ether1` (WAN)<br>`ether2` (LAN) | DHCP dari ISP<br>`10.216.27.1/24` | Otomatis<br>N/A | Pusat perutean utama, manajemen NAT, dan penegak regulasi *Firewall Filter Rules*. |
| 🐧 **Web & FTP Server (Ubuntu)** | `enp0s8` (Internal LAN) | `10.216.27.100/24` | `10.216.27.1` | Host portal bisnis internal `TechSecure` serta pelayan penyimpanan berkas berbasis FTP. |
| 🐉 **Security Auditor (Kali Linux)**| `eth1` (Internal LAN) | `10.216.27.10/24` | `10.216.27.1` | Konsol pengujian perimeter, pemindaian kerentanan (*Vulnerability Assessment*), dan audit penetrasi. |

---

## 🔄 3. Security Lifecycle & Versioning Control

Proyek ini dikembangkan secara modular untuk memperlihatkan transisi keamanan yang terukur:

```diff
 ┌────────────────────────────────────────────────────────┐
 │                                                        │
 ▼                                                        │
+[Build/Upgrade Lab] ──> [Audit & Scan] ──> [Hardening] ──┘
 (Blueprints Base)       (Docs/Laporan)     (Remediasi Kode)
       │
       ▼
-[Push Secure Config] ──> v1.0.0 (Vulnerable) ──> v1.1.0 (Hardened Stable)
