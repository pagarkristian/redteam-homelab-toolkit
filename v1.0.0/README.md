<div align="center">
  <img src="https://img.shields.io/badge/Phase-Exploitation_%26_Retest-D32F2F?style=for-the-badge&logo=kali-linux&logoColor=white" alt="Phase" />
  <img src="https://img.shields.io/badge/Target-Enterprise_Homelab_Core-1F4068?style=for-the-badge&logo=target&logoColor=white" alt="Target" />
  <img src="https://img.shields.io/badge/Status-Done-1F883D?style=for-the-badge" />
</div>

# v1.0.0: Exploitation & Retest

> Every attack here was run against a server I built myself, on an isolated host-only network with no internet-facing exposure.

## What this covers

Full exploitation chain against the original vulnerable server from [enterprise-homelab-core, v1.0.0](https://github.com/pagarkristian/enterprise-homelab-core/tree/main/v1.0.0): port scanning and enumeration, SQL injection to bypass the admin login, an uploaded PHP web shell for remote code execution, SSH credential brute-forcing, and flag capture at two privilege levels. The same attacks are then re-run against the hardened server to confirm every path was actually closed — see [v1.0.0-audit_report.md](./v1.0.0-audit_report.md).

## How this connects to the defender repo

| Phase | Where |
|---|---|
| Vulnerable server built | `enterprise-homelab-core`, [`v1.0.0/v1.0.0-vulnerable.md`](https://github.com/pagarkristian/enterprise-homelab-core/blob/main/v1.0.0/v1.0.0-vulnerable.md) |
| **Full exploitation chain + flag capture** | **this repo, [`v1.0.0-audit_report.md`](./v1.0.0-audit_report.md)** |
| Server hardened | `enterprise-homelab-core`, [`v1.0.0/v1.1.0-hardened.md`](https://github.com/pagarkristian/enterprise-homelab-core/blob/main/v1.0.0/v1.1.0-hardened.md) |
| Re-attack confirming the hardening holds | this repo, [`v1.0.0-audit_report.md`](./v1.0.0-audit_report.md) (retest section) |

---

<div align="center">
  <sub>Redteam Homelab Toolkit v1.0.0 · Maintained by <b>pagarkristian</b></sub>
</div>
