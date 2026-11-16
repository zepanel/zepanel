# zPanel v2 — Next-Generation Linux Web Hosting & Server Control Panel

High-performance, resource-efficient Linux server control panel natively integrated with **[zMaxServer](https://github.com/zmaxserver/zmaxserver)** and OpenLiteSpeed.

Official Website: **[https://www.zepanel.com](https://www.zepanel.com)** • Documentation: **[https://www.zepanel.com/wiki](https://www.zepanel.com/wiki)** • Downloads: **[https://www.zepanel.com/downloads](https://www.zepanel.com/downloads)**

---

## ⚡ Overview

**zPanel v2** is a modern, enterprise-grade Linux server and web hosting management control panel designed for hosting providers, data centers, system administrators, and software developers.

Engineered with an ultra-lightweight Go and C core architecture, zPanel delivers sub-millisecond panel latency and consumes **up to 40% less memory (RAM)** compared to legacy hosting panels, eliminating server resource bloat.

### Key Highlights
- **zMaxServer Native Integration:** Fully integrated with **[zMaxServer](https://github.com/zmaxserver/zmaxserver)**, an event-driven, asynchronous web server engine based on OpenLiteSpeed featuring native HTTP/3 QUIC, built-in LSCache, and high-concurrency request processing.
- **LiteSpeed SAPI (LSAPI) Multi-PHP:** High-speed PHP execution through native LSAPI and isolated LSPHP worker pools (supporting PHP 7.4 through PHP 8.4) per virtual host.
- **Enterprise Mail Service:** Robust Postfix & Dovecot backbone integrated with modern Roundcube Webmail, automated SPF, 2048-bit DKIM, and DMARC wizard alignment.
- **Unified Database Hub:** Fast MariaDB / MySQL management with remote IP whitelist access and one-click import/export.
- **Bulletproof Security:** Automated Let's Encrypt Wildcard SSL, real-time Fail2ban brute-force shield, and directory chroot isolation.

---

## 📦 Distribution Packages (`PAKET/`)

This repository contains official pre-compiled distribution packages inside the [`PAKET/`](PAKET/) directory:

### 1. AlmaLinux / Rocky Linux / RHEL (.rpm)
Directory: [`PAKET/ALMALINUX/`](PAKET/ALMALINUX/)
- `zpanel-1.0.09.x86_64.rpm` (Master Metapackage)
- `zpanel-core-1.0.09.x86_64.rpm` (Core System Service)
- `zpanel-agent-1.0.09.x86_64.rpm` (Node & Agent Daemon)
- `zpanel-ui-1.0.09.x86_64.rpm` (Control Panel Interface)

Installation:
```bash
sudo rpm -Uvh zpanel-1.0.09.x86_64.rpm
```

### 2. Debian Linux (.deb)
Directory: [`PAKET/DEBIAN/`](PAKET/DEBIAN/)
- `zpanel_1.0.09_amd64.deb` (Master Metapackage)
- `zpanel-core_1.0.09_amd64.deb` (Core System Service)
- `zpanel-agent_1.0.09_amd64.deb` (Node & Agent Daemon)
- `zpanel-ui_1.0.09_amd64.deb` (Control Panel Interface)

Installation:
```bash
sudo dpkg -i zpanel_1.0.09_amd64.deb
```

### 3. Ubuntu Linux (.deb)
Directory: [`PAKET/UBUNTU/`](PAKET/UBUNTU/)
- `zpanel_1.0.09_amd64.deb` (Master Metapackage)
- `zpanel-core_1.0.09_amd64.deb` (Core System Service)
- `zpanel-agent_1.0.09_amd64.deb` (Node & Agent Daemon)
- `zpanel-ui_1.0.09_amd64.deb` (Control Panel Interface)

Installation:
```bash
sudo dpkg -i zpanel_1.0.09_amd64.deb
```

### 4. Interactive Installer
Directory: [`PAKET/INSTALLER/`](PAKET/INSTALLER/)
- `install` (Automated dependency installer)
- `zpanel-installer` (Interactive terminal setup wizard)
- `zpanel-uninstaller` (Clean removal utility)

Installation:
```bash
chmod +x install
sudo ./install
```

---

## 🌐 Supported Linux Distributions

As documented on **[zepanel.com/downloads](https://www.zepanel.com/downloads)**:

| Distribution | Supported Versions | Architecture | Package Format | Status |
|---|---|---|---|---|
| **Ubuntu Linux** | 24.04 LTS (Noble), 22.04 LTS (Jammy), 20.04 LTS (Focal) | x86_64, aarch64 (ARM64) | `.deb` | ✅ Full Native Support |
| **Debian Linux** | Debian 12 (Bookworm), Debian 11 (Bullseye) | x86_64, aarch64 (ARM64) | `.deb` | ✅ Full Native Support |
| **AlmaLinux** | AlmaLinux 9.x, AlmaLinux 8.x | x86_64 | `.rpm` | ✅ 1:1 RHEL Binary Compatible |
| **Rocky Linux** | Rocky Linux 9.x, Rocky Linux 8.x | x86_64 | `.rpm` | ✅ 1:1 RHEL Binary Compatible |

> [!IMPORTANT]
> **Clean OS Policy:** zPanel v2 must be deployed on a clean, minimal Linux installation without pre-existing Apache, Nginx, or third-party hosting managers.

---

## ⚙️ Hardware & System Requirements

| Component | Minimum Specification | Recommended (Production) |
|---|---|---|
| **Processor (vCPU)** | 1 Core (1.5 GHz+) | 2+ Cores (2.4 GHz+) |
| **Memory (RAM)** | 1 GB RAM (with 512 MB Swap) | 4 GB+ RAM |
| **Storage** | 10 GB Free SSD | 40 GB+ NVMe SSD (RAID10) |
| **Network & IP** | 1x Static Public IPv4 | 1x Static IPv4 + /64 IPv6 Subnet (1 Gbps) |
| **Port Access** | 22, 80, 443 | 8026 (Client UI), 8036 (Admin & Reseller UI) |

---

## 🔒 Standardized Port Matrix

zPanel enforces strict port boundaries to guarantee clean separation between management layers:

- **8026 / TCP (HTTPS):** End-User Client Control Panel (Hosting accounts, DB, Mail, Files)
- **8036 / TCP (HTTPS):** Reseller & Root Administrator Console
- **8046 / TCP (HTTPS):** Webmail Interface (Roundcube)
- **8047 / TLS:** Setup Wizard & Emergency Recovery Console (`zpanel-installer`, internal port 8789)
- **8056 / TCP (HTTPS):** External REST API Gateway (internal port 8055)
- **22 / TCP (SSH):** Secure Shell Console
- **80 / TCP (HTTP):** Web Traffic & ACME HTTP-01 Validation
- **443 / TCP (HTTPS):** Production TLS Traffic (HTTP/2 & HTTP/3 / QUIC)

---

## 🛠️ Command-Line & Service Management

zPanel provides dedicated system utilities and systemd daemons for server operations:

### 1. Panel CLI Utility (`zpanel`)
The core administrative binary located at `/opt/zpanel/bin/zpanel`:

```bash
# Check and install signed updates from the central repository
zpanel update

# Renew all Let's Encrypt and TLS certificates across domains
zpanel ssl renew

# Automated WordPress deployment for a specific domain
zpanel wp install --domain example.com --title "My Site" --admin admin --email admin@example.com
```

### 2. Systemd Service Control
Manage the underlying zPanel daemons directly via systemctl:

```bash
# Check status of core panel services
systemctl status zpanel-api zpanel-worker zpanel-agent

# Restart core API and background worker
systemctl restart zpanel-api zpanel-worker

# Restart the node agent service
systemctl restart zpanel-agent

# Restart the emergency maintenance / installer service (port 8047)
systemctl restart zpanel-installer

# Restart zMaxServer web engine
systemctl restart zmaxserver

# Probe internal API health endpoint
curl -s http://127.0.0.1:8055/v1/health

# Inspect real-time service logs
journalctl -u zpanel-api -n 50 --no-pager
journalctl -u zpanel-agent -n 50 --no-pager
```

---

## 🔌 Billing & Automation Extensions

Official provisioning modules for commercial hosting platforms:

### WHMCS Modules
- [zpanel-vps-whmcs](https://github.com/zepanel/zpanel-vps-whmcs) — WHMCS Cloud / VPS Provisioning Module
- [zpanel-hosting-whmcs](https://github.com/zepanel/zpanel-hosting-whmcs) — WHMCS Shared & Reseller Web Hosting Module
- [zpanel-license-whmcs](https://github.com/zepanel/zpanel-license-whmcs) — WHMCS zPanel License Sales & Distribution Module

### WISECP Modules
- [zpanel-vps-wisecp](https://github.com/zepanel/zpanel-vps-wisecp) — WISECP VPS & Cloud Automation
- [zpanel-hosting-wisecp](https://github.com/zepanel/zpanel-hosting-wisecp) — WISECP Web Hosting Provisioning
- [zpanel-license-wisecp](https://github.com/zepanel/zpanel-license-wisecp) — WISECP License Provisioning Module

### PNLCS Modules
- [zpanel-vps-pnlcs](https://github.com/zepanel/zpanel-vps-pnlcs) — PNLCS Cloud / Virtualization Module
- [zpanel-hosting-pnlcs](https://github.com/zepanel/zpanel-hosting-pnlcs) — PNLCS Hosting & Domain Integration
- [zpanel-license-pnlcs](https://github.com/zepanel/zpanel-license-pnlcs) — PNLCS License Automation Module

---

## 📄 License & Terms

This project is licensed under the **[MIT License](LICENSE)**.

- **Official Website:** [https://www.zepanel.com](https://www.zepanel.com)
- **Web Server Engine:** [https://github.com/zmaxserver/zmaxserver](https://github.com/zmaxserver/zmaxserver)
- **Documentation:** [https://www.zepanel.com/wiki](https://www.zepanel.com/wiki)
- **Technical Support:** [destek@zepanel.com](mailto:destek@zepanel.com)
- **Copyright:** © 2026 zPanel.
