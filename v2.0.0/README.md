<div align="center">
<img src="https://img.shields.io/badge/Project-Redteam_Homelab_Toolkit-D32F2F?style=for-the-badge&logo=hackaday&logoColor=white" />
<img src="https://img.shields.io/badge/Version-2.0.0-0052CC?style=for-the-badge" />
<img src="https://img.shields.io/badge/Status-Done-1F883D?style=for-the-badge" />
</div>

# v2.0.0: Attack Simulation

Offensive counterpart to [enterprise-homelab-core v2.0.0](https://github.com/pagarkristian/enterprise-homelab-core/tree/main/v2.0.0). Two intentionally vulnerable features were deployed there; this phase attacks them and checks whether Wazuh notices.

## Contents

| # | Document | Status |
|---|----------|--------|
| 1 | [Attack Simulation](./v2.2.1-attack-simulation.md) | Done — IDOR, SQL injection, and command injection confirmed; none caught by the default Wazuh ruleset |

## How this connects to the defender repo

| Phase | Where |
|---|---|
| Business context, vulnerable code | `enterprise-homelab-core/v2.0.0/v2.1.0`, `v2.1.1` |
| Monitoring setup (Wazuh) | `enterprise-homelab-core/v2.0.0/v2.2.0` |
| **Attack simulation** | **this repo, v2.2.1** |
| Detection engineering, retesting, report | `enterprise-homelab-core/v2.0.0/v2.3.0`, `v2.3.1`, `v2.4.0` |

---

<div align="center">
<sub>Redteam Homelab Toolkit v2.0.0 · Maintained by <b>pagarkristian</b></sub>
</div>
