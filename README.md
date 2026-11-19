# cisco-enterprise-access-control-lab
Final Year Assignment

# Enterprise Access Control & Network Services Lab

![Cisco Packet Tracer](https://img.shields.io/badge/Tool-Cisco%20Packet%20Tracer-blue)
![Network Security](https://img.shields.io/badge/Domain-Network%20Security%20%26%20IAM-green)
![Protocols](https://img.shields.io/badge/Protocols-AAA%2FRADIUS%20%7C%20WPA2--Enterprise%20%7C%20SMTP%20%7C%20POP3%20%7C%20FTP-orange)

## 📌 Project Overview

This lab project simulates an enterprise **Identity and Access Management (IAM)** infrastructure and core network services using Cisco Packet Tracer. The scenario focuses on securing enterprise network access and operational services across multiple headquarters and branch departments.

Key engineering objectives achieved in this project:
1. **Wireless Network Security**: Configured central **AAA / RADIUS server authentication** and **WPA2-Enterprise** security for mobile client endpoints receiving dynamic IP addressing via DHCP.
2. **Enterprise Messaging Infrastructure**: Provisioned **SMTP** and **POP3** email services under a custom domain (`mail.cyberhq.com`), enabling encrypted cross-departmental communication.
3. **Role-Based File Access Control (RBAC)**: Configured an **FTP server** with granular user permission levels (Read, Write, Delete, Rename, List) and verified security policies via Command Line Interface (CLI) testing.

---

## 📐 Network Architecture & Topology

The enterprise topology consists of a **Headquarters (HQ)** site housing the core server rack (Wiring Closet) and multiple department workstations, connected via enterprise routing/switching backbones:

```
                          +-------------------------------+
                          |        AAA RADIUS Server      |
                          +---------------+---------------+
                                          |
+---------------------+   +---------------+---------------+   +----------------------+
|  HQ Laptops / PCs   |---|  Central Switch / Access Pt   |---|  Mail & FTP Servers  |
|  (Dynamic DHCP)     |   +-------------------------------+   | (192.168.75.0/24)    |
+---------------------+                                       +----------------------+
```

### Core Services Summary

| Service | Server / Domain | Subnet / IP | Security Protocol |
| :--- | :--- | :--- | :--- |
| **AAA Authentication** | RADIUS Server | Internal HQ | WPA2-Enterprise (802.1X) |
| **DHCP Services** | Router / AP | `192.168.50.0/24` | Dynamic Assignment |
| **Email Services** | `mail.cyberhq.com` | Internal HQ | SMTP & POP3 |
| **FTP Repository** | FTP Server | `192.168.75.2` | Privilege-based Access Control |

---

## ⚙️ Implementation Details

### Part 1: AAA/RADIUS Authentication & WPA2-Enterprise Wireless Setup

- Enabled **AAA / RADIUS** service on the centralized authentication server.
- Configured user identity records (`user1`, `user2`) with strong credentials.
- Provisioned enterprise wireless Access Points with SSID **HQ-INT** utilizing WPA2 Enterprise security.
- Standardized wireless endpoints (`HQ-Laptop-1`, `HQ-Laptop-2`) to authenticate against the AAA server before obtaining IP configuration from the `192.168.50.0/24` DHCP pool.

### Part 2: Enterprise Email Deployment (SMTP & POP3)

- Activated **SMTP** (mail sending) and **POP3** (mail retrieval) services on the enterprise mail server.
- Configured domain identity `mail.cyberhq.com` and user accounts for multi-site users (`HQuser1`, `HQuser2`, `BRuser1`, `BRuser2`).
- Configured mail clients across desktop endpoints (`PC 1-1`, `PC 2-3`, `HQ-Laptop-1`, `Net-Admin`).
- Validated end-to-end messaging by composing, routing, and receiving test emails between headquarters and branch offices.

### Part 3: FTP Server & Role-Based Access Control (RBAC)

- Deployed centralized FTP repository on host IP `192.168.75.2`.
- Enforced strict privilege profiles per user role:

| Username | Read (R) | Write (W) | Delete (D) | Rename (N) | List (L) | Assigned Role |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **`sukyi`** | ✅ | ✅ | ✅ | ✅ | ✅ | Network Administrator |
| **`ajulo`** | ✅ | ✅ | ✅ | ✅ | ✅ | Systems Administrator |
| **`malia`** | ✅ | ✅ | ❌ | ✅ | ✅ | Read-Write Operator (Restricted Delete) |

---

## 🧪 Verification & Security Testing

### 1. FTP Administrative File Transfer (Full Access Verification)
Using the administrative profile (`sukyi`), connected via CLI to perform directory listings, file downloads, and uploads:

```bash
# Connect to internal FTP Server
ftp 192.168.75.2
User: sukyi
Password: ********

# Inspect remote files and download
ftp> dir
ftp> get aMessage.txt

# Create and upload updated configuration/text file
ftp> put aMessage_new.txt
250 File transfer successful.
ftp> quit
```

### 2. Access Control & Privilege Limitation Verification
Tested restriction policies using the read-only/restricted user profile (`malia`):

```bash
ftp 192.168.75.2
User: malia
Password: ********

# Test 1: Attempt file deletion (Expected Result: Denied / Failed)
ftp> delete aMessage_new.txt
550 Command failed: File deletion not permitted for account level.

# Test 2: Attempt file renaming (Expected Result: Allowed)
ftp> rename aMessage_new.txt aMessage_rename.txt
350 File renamed successfully.
```

**Security Audit Result:** Verified that user privileges were strictly enforced according to access control rules, successfully preventing unauthorized file deletion while preserving necessary write and rename permissions.

---

## 🛠️ Key Technical Skills Demonstrated

- **Identity & Access Management (IAM)**: AAA server configuration, RADIUS integration, and enterprise authentication workflow.
- **Wireless Infrastructure Security**: WPA2-Enterprise integration with centralized identity stores.
- **Application Protocols**: Hands-on administration of SMTP, POP3, FTP, and DHCP protocols.
- **Role-Based Access Control (RBAC)**: Implementation and security validation of least-privilege access rules.
- **Troubleshooting & Testing**: CLI-based verification (`ftp`, `dir`, `get`, `put`, `delete`, `rename`) and converged network testing.

---

## 📁 Recommended Repository Layout

```text
├── README.md                          # Project Documentation
├── screenshots/                       # Lab Evidence & Topology Visuals
│   ├── network_topology.png           # Packet Tracer Topology Layout
│   ├── aaa_radius_configuration.png   # AAA User Configuration
│   ├── email_verification.png          # Successful Email Routing Test
│   └── ftp_privilege_cli_test.png     # CLI Output showing Privilege Audit
```

---
*Created as part of the BSc Applied Computing Practical Network Engineering Portfolio.*
