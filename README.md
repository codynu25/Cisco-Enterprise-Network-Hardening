# Enterprise Network Hardening & Compliance Framework
### Securing Cisco Infrastructure in Compliance with NIS2, ISO 27001, and DNSC Alerts

---

## 📌 Project Overview
This project demonstrates the practical implementation of network segmentation and device hardening on Cisco infrastructure. It serves as a direct technical response to critical vulnerabilities (CVEs) flagged by the Romanian National Cyber Security Directorate (**DNSC**) in 2026. 

The primary objective is to transition from a vulnerable **flat network** architecture to a highly secure, segmented environment governed by the principle of **Least Privilege**.

---

## 🗺️ Regulatory & Framework Mapping

The architecture directly implements and satisfies the following security standards:
*   **NIS2 Directive (Cyber Hygiene):** Network segregation to mitigate lateral movement and blast radius in case of a workstation breach.
*   **ISO/IEC 27001:2022 Control A.8.20 (Network Security):** Segregation of network management, public services, and internal user zones into distinct logical domains.
*   **ISO/IEC 27001:2022 Control A.8.22 (Segregation in Networks):** Enforcing traffic filtering between users and critical assets using Access Control Lists (ACLs).

---

## 🛡️ Threat Landscape & Vulnerability Analysis

This deployment enforces strict Layer 3 isolation for two critical management servers exposed to catastrophic remote exploits (CVSS 9.8 - 10.0):

| Critical Asset | Vulnerability | CVSS Score | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **Cisco SD-WAN Controller** | CVE-2026-20127 | **10.0** | Isolated in **VLAN 30**. Prevented all lateral traffic from standard user zones using implicit/explicit ACL denials via NETCONF ports. |
| **Cisco SSM On-Prem** | CVE-2026-20160 | **9.8** | Restricted L3 access. Only the dedicated Administrator Workstation can initiate SSH/HTTPS handshakes. |

---

## 🏗️ Technical Architecture & Cisco IOS Hardening

### Network Segmentation (Layer 3 Vlan Mapping)
Configured on a Multilayer Core Switch (Cisco 3650 Catalyst):
*   **VLAN 10 (USERS):** `192.168.10.0/24` — Standard corporate workstations.
*   **VLAN 20 (MGMT):** `192.168.20.0/24` — Dedicated Secure Admin Workstation.
*   **VLAN 30 (SERVERS):** `10.0.30.0/24` — Critical Infrastructure / Management Servers.

### Baseline Device Hardening
To eliminate infrastructure overhead, prevent CLI freezes during typos, and mitigate unauthorized DNS spoofing/reconnaissance vectors, DNS lookup was globally disabled:

```vlan
Switch# configure terminal
Switch(config)# no ip domain-lookup
