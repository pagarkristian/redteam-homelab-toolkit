# 🎯 Red Team Arsenal: Targeted Auditing Suite for Enterprise Homelab

<div align="center">
  <img src="https://img.shields.io/badge/Operation-Red_Team_Engagement-D32F2F?style=for-the-badge&logo=kali-linux&logoColor=white" alt="Operation" />
  <img src="https://img.shields.io/badge/Target_Infrastruktur-Enterprise_Homelab_Core-1F4068?style=for-the-badge&logo=target&logoColor=white" alt="Target" />
  <img src="https://img.shields.io/badge/Focus-Web_To_RCE_/_Bypass-000000?style=for-the-badge&logo=python&logoColor=white" alt="Focus" />
</div>

---

## 💀 1. Executive Mission

> [!WARNING]
> **LEGAL DISCLAIMER:** Seluruh payload, skrip otomatisasi, dan metodologi eksploitasi di dalam repositori ini dikembangkan **MURNI UNTUK TUJUAN PENDIDIKAN, AUDIT KEAMANAN INTERNAL, DAN VALIDASI PERTAHANAN**. Pengujian dilakukan pada lingkungan laboratorium virtual terisolasi yang sah (`enterprise-homelab-core`).

Repositori ini berfungsi sebagai **buku catatan taktik (Playbook) dan gudang senjata digital (Arsenal)** sisi *Offensive Security* (Red Team). Fokus utamanya adalah mendokumentasikan bagaimana kelemahan arsitektur pada infrastruktur target versi `v1.0.0-vulnerable` dieksploitasi hingga mencapai kontrol penuh (*Remote Code Execution*).

---

## 🏛️ 2. Target Environment Mapping

Senjata di dalam repositori ini dikalibrasi secara spesifik untuk menyerang target dengan parameter jaringan berikut (Sisi Korporat):

* 🎯 **Target IP Host (Ubuntu Server):** `10.216.27.100` (Segmen Internal LAN)
* 🌐 **Layanan Target:** Portal Bisnis `TechSecure` (HTTP Port 80) & FTP Server (Port 21)
* 🛡️ **Perimeter Gateway:** MikroTik RouterOS v7 (`10.216.27.1`)
* 🐉 **Attacker Node:** Kali Linux Platform (`10.216.27.10`)

---

## 🎒 3. Exploit Toolkit & Attack Vectors

Berikut adalah pemetaan skrip dan *payload* yang digunakan dalam siklus serangan (*Attack Lifecycle*):

| Vektor Serangan | Lokasi Berkas / Skrip | Mekanisme Eksploitasi | Dampak Terhadap Target |
| :--- | :--- | :--- | :--- |
| 🌐 **Web Intrusion** | `payloads/reverse-webshell.php` | Mengunggah kode via celah *Unrestricted File Upload*. | Mendapatkan akses eksekusi perintah sistem (`www-data`). |
| 🤖 **Exploit Automation** | `exploits/rce_uploade_exploiter.py` | Otomatisasi request HTTP POST untuk menembus form upload. | Penanaman *backdoor* instan tanpa interaksi browser manual. |
| ⚙️ **Local Enumeration** | `recon/local_enum.sh` | Skrip shell otomatis untuk memetakan hak akses internal Ubuntu. | Mengidentifikasi celah *Misconfiguration* untuk eskalasi hak akses. |


🔗 3. Hubungan Antar-Proyek (Cross-Project Linkage)
Repositori ini adalah bagian ofensif yang tidak terpisahkan dari proyek pertahanan utama. Untuk melihat bagaimana arsitektur jaringan dibangun dan bagaimana serangan dari repo ini akhirnya diblokir total, silakan kunjungi tautan berikut:

[!TIP]

🏛️ Arsitektur Jaringan & Pertahanan: Tinjau cetak biru konfigurasi router dan pengamanan server di (https://github.com/pagarkristian/enterprise-homelab-core).

📄 Laporan Audit Resmi: Baca analisis lengkap kegagalan serangan (Re-Exploit) setelah proses hardening dilakukan di Dokumen Audit & Validasi Keamanan v1.1.0.
