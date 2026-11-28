<div align="center">
<img src="https://img.shields.io/badge/Project-Redteam_Homelab_Toolkit-D32F2F?style=for-the-badge&logo=kali-linux&logoColor=white" />
<img src="https://img.shields.io/badge/Role-Red_Team_/_Attacker-D32F2F?style=for-the-badge" />
<img src="https://img.shields.io/badge/Status-In_Progress-D29922?style=for-the-badge" />
</div>

<h1 align="center">Redteam Homelab Toolkit</h1>
<p align="center"><sub>Offensive counterpart to <a href="https://github.com/pagarkristian/enterprise-homelab-core">enterprise-homelab-core</a> — attacking the server built and defended there.</sub></p>

---

## ⚠️ Scope

> Every attack documented here was run against a server I built myself, inside an isolated VirtualBox host-only network with no path to the internet. Nothing here was run against a system I don't own or control.

## 🌟 What this is

This repo is the attacker's side of a two-repo project. The defender side (`enterprise-homelab-core`) builds a small-business-style network and server; this repo documents attacking it, twice:

- **v1.0.0** — a full exploitation chain against the original, deliberately weak server: port scanning, SQL injection to bypass login, an uploaded PHP web shell for remote code execution, SSH brute-forcing, and flag capture. Re-run against the hardened v1.1.0 server afterward to confirm every one of those paths was actually closed.
- **v2.0.0** — two new, more subtle application-logic flaws (IDOR and SQL injection on a payslip page, command injection via an upload filename), attacked while a Wazuh SIEM watches, specifically to test whether a SIEM's *default* ruleset catches attacks that don't look like errors.

## 🗂️ Chapters

| Version | What happens | Where |
|---|---|---|
| **v1.0.0** | Full exploitation chain against the original vulnerable server (FTP/Samba enumeration, SQLi auth bypass, file-upload RCE, SSH brute force, flag capture), then the same attacks re-run against the hardened server to confirm they fail. | [`v1.0.0/`](./v1.0.0) |
| **v2.0.0** | IDOR, SQL injection, and command-injection-via-upload against the two new TechSecure features, checked against Wazuh at each step. | [`v2.0.0/`](./v2.0.0) |

## 🔗 How this connects to the defender repo

| Phase | Where |
|---|---|
| Vulnerable server built, hardened, re-tested | `enterprise-homelab-core`, `v1.0.0/` |
| Business context, new vulnerable features | `enterprise-homelab-core`, `v2.0.0/v2.1.0`, `v2.1.1` |
| Monitoring setup (Wazuh) | `enterprise-homelab-core`, `v2.0.0/v2.2.0` |
| **Attack simulation** | **this repo, `v2.0.0/v2.2.1`** |
| Detection engineering, retesting, incident report | `enterprise-homelab-core`, `v2.0.0/v2.3.0`, `v2.3.1`, `v2.4.0` |

## ✅ What this is meant to show

- Finding and chaining real vulnerabilities (not just running a scanner and reading its output) — including a case where a rough test was knowingly run against the wrong feature first, caught, and corrected before trusting the result.
- Telling the difference between "an attack is possible in theory" and "an attack is confirmed, with evidence" — v2.0.0 is explicit about which paths were actually exploited versus only found in code review.
- Thinking about detection as a defender would while attacking as a red teamer — the whole point of v2.0.0 is testing what a SIEM notices, not just what breaks.

## 🤖 Note on AI assistance

The structure, wording, and formatting of this documentation were drafted with AI assistance. The attacks themselves — reconnaissance, exploitation, and verifying each result against real server logs — were carried out hands-on.

---

<div align="center">
<sub>Redteam Homelab Toolkit · Maintained by <b>pagarkristian</b></sub>
</div>
