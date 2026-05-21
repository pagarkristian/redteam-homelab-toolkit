# 🎯 Red Team Arsenal: Targeted Auditing Suite for Enterprise Homelab

<div align="center">
  <img src="https://img.shields.io/badge/Operation-Red_Team_Engagement-D32F2F?style=for-the-badge&logo=kali-linux&logoColor=white" alt="Operation" />
  <img src="https://img.shields.io/badge/Target_Infrastructure-Enterprise_Homelab_Core-1F4068?style=for-the-badge&logo=target&logoColor=white" alt="Target" />
  <img src="https://img.shields.io/badge/Focus-Full--Chain_Vulnerability_Validation-000000?style=for-the-badge&logo=python&logoColor=white" alt="Focus" />
</div>

---

## 💀 1. Executive Mission

> [!WARNING]
> **LEGAL DISCLAIMER:** All payloads, automation scripts, and exploitation methodologies within this repository are developed **STRICTLY FOR EDUCATIONAL PURPOSES, INTERNAL SECURITY AUDITS, AND DEFENSIVE VALIDATION**. Testing was conducted exclusively within a legally authorized, isolated virtual laboratory environment (`enterprise-homelab-core`).

This repository serves as an **Offensive Security (Red Team) Playbook and Digital Weapon Arsenal**. The primary focus is to document how architectural flaws within the **designated target version baselines (e.g., `v1.0.0-vulnerable` and subsequent unhardened releases)** were exploited to achieve full control (*Remote Code Execution*).

---

## 🏛️ 2. Target Environment Mapping

The tools in this repository are specifically calibrated to attack targets within the following corporate network parameters:

* 🎯 **Target Host IP (Ubuntu Server):** `10.216.27.100` (Internal LAN Segment)
* 🌐 **Target Services:** `TechSecure` Business Portal (HTTP Port 80) & FTP Server (Port 21)
* 🛡️ **Perimeter Gateway:** MikroTik RouterOS v7 (`10.216.27.1`)
* 🐉 **Attacker Node:** Kali Linux Platform (`10.216.27.10`)

---



## 🔗 3. Cross-Project Linkage

This repository is an inseparable offensive counterpart to the main defensive project. To examine how the network architecture was built and how attacks from this arsenal were ultimately **blocked completely**, please visit the following links:

> [!TIP]
> * 🏛️ **Network Architecture & Defense:** Review the router configurations and server hardening blueprints at the [Enterprise Homelab Core Repository](https://github.com/pagarkristian/enterprise-homelab-core).
> * 📄 **Official Retesting Report:** Read the comprehensive analysis of the failed attack (*Re-Exploit*) post-hardening in the [v1.1.0 Security Retesting Document](https://github.com/pagarkristian/enterprise-homelab-core/blob/main/v1.1.0%20Retesting.md).

---

<div align="center">
  <sub>Maintained by <b>pagarkristian</b> for Cyber Security & Red Team Portfolio Standardization.</sub>
</div>
