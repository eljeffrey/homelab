# HOMELAB

![Status](https://img.shields.io/badge/Status-Active-success)
![Platform](https://img.shields.io/badge/Platform-Ubuntu%20%2B%20Windows-blue)
![Directory](https://img.shields.io/badge/Directory-Samba%20AD-purple)
![Automation](https://img.shields.io/badge/Automation-Ansible-orange)
![License](https://img.shields.io/badge/Open%20Source-First-brightgreen)

---

# Enterprise Infrastructure Homelab

A self-hosted enterprise infrastructure environment focused on Active Directory, identity management, Windows administration, Linux administration, networking, automation, and infrastructure engineering.

This lab is designed to simulate real-world enterprise environments using Samba Active Directory Domain Services, Windows 11, Kerberos, DNS, LDAP, and Infrastructure as Code.

---

# Open Source Philosophy

This homelab is intentionally built using free and open-source technologies whenever possible.

The goal is to develop enterprise infrastructure, identity management, and automation skills without relying on expensive commercial licensing.

## Technologies Used

| Technology | Purpose | License |
|------------|---------|---------|
| Ubuntu Server | Infrastructure Platform | Open Source |
| Samba AD DC | Active Directory Services | Open Source |
| Kerberos | Authentication | Open Source |
| LDAP | Directory Services | Open Source |
| OpenSSH | Remote Administration | Open Source |
| Ansible | Automation | Open Source |
| Git | Version Control | Open Source |
| Apache Directory Studio | LDAP Management | Open Source |
| UTM | Virtualization Platform | Open Source |

## Commercial Software Usage

The only commercial operating system currently used in the lab is:

- Windows 11 Pro (Client Workstation)

Windows is included solely to test:

- Active Directory interoperability
- Group Policy
- RSAT
- Enterprise workstation management
- Domain authentication

All core infrastructure services are provided by open-source software.

## Design Goals

- Build enterprise infrastructure using freely available tools
- Learn Active Directory concepts through Samba AD
- Automate infrastructure deployments
- Document troubleshooting and operational procedures
- Build reproducible environments
- Minimize dependency on proprietary solutions

---

# Skills Demonstrated

## Identity & Access Management

- Active Directory
- Kerberos Authentication
- LDAP
- User & Group Management
- Role-Based Access Control (RBAC)
- Domain Administration

## Windows Administration

- Windows 11 Domain Join
- RSAT Administration
- DNS Management
- Group Policy Management
- Domain Authentication

## Linux Administration

- Ubuntu Server
- Samba Active Directory Domain Services
- DNS Configuration
- Service Management
- System Troubleshooting

## Networking

- DNS
- TCP/IP
- Static Addressing
- Name Resolution
- Service Discovery
- Active Directory Networking

## Automation

- Infrastructure as Code
- Ansible (Planned)
- Configuration Management

---

# Architecture

## Domain Information

| Setting | Value |
|----------|--------|
| Domain | HOMELAB.INTERNAL |
| NetBIOS | HOMELAB |
| Network | 192.168.1.0/24 |
| Gateway | 192.168.1.1 |
| Primary DNS | 192.168.1.10 |

---

# Infrastructure Inventory

| Hostname | Role | IP Address | Operating System |
|-----------|------|------------|------------------|
| LABDC1 | Active Directory Domain Controller | 192.168.1.10 | Ubuntu Server |
| LABWS01 | Domain Workstation | 192.168.1.100 | Windows 11 Pro |
| MacBook Pro | Management Workstation / UTM Host | DHCP | macOS |

---

# Services

LABDC1 currently provides:

| Service | Port |
|----------|------|
| DNS | 53 |
| Kerberos | 88 |
| LDAP | 389 |
| SMB | 445 |
| Global Catalog | 3268 |

## Active Directory Services

- Domain Authentication
- DNS
- Kerberos
- LDAP
- SMB/CIFS
- Group Policy Infrastructure
- User and Group Management

---

# Network Diagram

```text
                          Internet
                              │
                        Gateway
                     192.168.1.1
                              │
        ┌─────────────────────┴─────────────────────┐
        │                                           │
┌─────────────────┐                     ┌─────────────────┐
│ MacBook Pro     │                     │ LABDC1          │
│ UTM Host        │ SSH / Management    │ 192.168.1.10    │
│ macOS           │────────────────────▶│ Samba AD DC     │
└─────────────────┘                     │ DNS             │
                                        │ Kerberos        │
                                        │ LDAP            │
                                        │ SMB             │
                                        └────────┬────────┘
                                                 │
                                                 │ Domain Join
                                                 │
                                        ┌────────▼────────┐
                                        │ LABWS01         │
                                        │ 192.168.1.100   │
                                        │ Windows 11 Pro  │
                                        │ RSAT Installed  │
                                        └─────────────────┘
```

---

# Current State

## Infrastructure

- [x] Ubuntu Server Installed
- [x] Static IP Configuration
- [x] Samba AD Domain Controller Provisioned
- [x] Internal DNS Configured
- [x] Kerberos Operational
- [x] LDAP Operational
- [x] SMB Operational
- [x] DNS Forwarding Operational

## Active Directory

- [x] Domain Created
- [x] Domain Administrator Account Created
- [x] Domain Authentication Validated
- [x] Windows 11 Domain Join Completed
- [x] Domain Login Tested

## Administration

- [x] RSAT Installed
- [x] DNS Management Available
- [x] Active Directory Administration Available
- [x] SSH Access Configured

---

# Administrative Accounts

| Account | Purpose |
|----------|---------|
| Administrator | Built-in Domain Administrator |
| jeffdomainadmin | Primary Administrative Account |

Authentication methods:

```text
HOMELAB\jeffdomainadmin
```

```text
jeffdomainadmin@homelab.internal
```

---

# Validation Commands

## DNS

```bash
host -t SRV _ldap._tcp.homelab.internal 127.0.0.1
host -t SRV _kerberos._tcp.homelab.internal 127.0.0.1
host google.com 127.0.0.1
```

## Kerberos

```bash
kinit jeffdomainadmin
klist
```

## Active Directory

```bash
samba-tool user list
samba-tool fsmo show
```

## Replication

```bash
samba-tool drs showrepl
```

## Health Checks

```bash
samba-tool dbcheck
```

---

# Lessons Learned

## DNS Is Active Directory

The majority of Active Directory failures originated from DNS issues rather than authentication problems.

Symptoms observed:

- Kerberos failures
- Domain join failures
- RSAT issues
- Name resolution failures
- Service discovery failures

### Lesson

Always troubleshoot DNS first.

---

## DNS Forwarder Misconfiguration

After provisioning Samba AD, DNS forwarding pointed to:

```ini
dns forwarder = 127.0.0.53
```

After disabling systemd-resolved, external lookups failed.

### Symptoms

```text
SERVFAIL
```

### Resolution

```ini
dns forwarder = 1.1.1.1
```

Restart Samba:

```bash
systemctl restart samba-ad-dc
```

Result:

- External DNS resolution restored
- Microsoft endpoints reachable
- Windows feature installation restored
- RSAT installation issues resolved

---

## Kerberos Depends on DNS

Authentication failures were frequently caused by DNS misconfiguration.

### Validation

```bash
kinit Administrator
klist
```

---

## Hostnames Must Be Consistent

The following must agree:

- Hostname
- DNS Records
- /etc/hosts
- Kerberos Realm

Working example:

```text
192.168.1.10 labdc1.homelab.internal labdc1
```

---

## Samba AD Requires Ownership of DNS Port 53

The Samba AD service could not start because another service was listening on port 53.

### Diagnosis

```bash
ss -tulpn | grep :53
```

### Resolution

```bash
systemctl stop systemd-resolved
systemctl disable systemd-resolved
```

Restart Samba:

```bash
systemctl restart samba-ad-dc
```

---

## Separate Administrative Accounts

Created dedicated administrative account:

```text
jeffdomainadmin
```

Added to:

```text
Domain Admins
```

This mirrors enterprise best practices and avoids using the built-in Administrator account for daily administration.

---

## Validate One Layer At A Time

Successful troubleshooting order:

```text
Network
  ↓
DNS
  ↓
Kerberos
  ↓
LDAP
  ↓
Domain Join
  ↓
Group Policy
  ↓
Replication
```

---

# Roadmap

## Phase 2 - High Availability

- [ ] LABDC2 Secondary Domain Controller
- [ ] Multi-DC Replication
- [ ] DNS Redundancy
- [ ] FSMO Role Testing

## Phase 3 - Enterprise Administration

- [ ] Organizational Units
- [ ] Group Policy Objects
- [ ] User Lifecycle Management
- [ ] Delegated Administration

## Phase 4 - Infrastructure Services

- [ ] LABFS01 File Server
- [ ] SMB Shares
- [ ] Backup Infrastructure

## Phase 5 - Automation

- [ ] Ansible Control Node
- [ ] Automated Domain Controller Deployment
- [ ] Automated Workstation Configuration
- [ ] Infrastructure as Code Repository

## Phase 6 - Security

- [ ] Kali Linux VM
- [ ] Vulnerability Scanning
- [ ] Security Hardening
- [ ] Audit Logging

## Phase 7 - Cloud Identity

- [ ] Microsoft Entra ID Integration
- [ ] Hybrid Identity
- [ ] Conditional Access Testing
- [ ] Intune Evaluation

---

# Repository Structure

```text
homelab/
├── README.md
├── ansible/
├── configs/
├── docs/
├── diagrams/
├── scripts/
└── screenshots/
```

---

# Author

**Jeffrey Cabrera**

Enterprise Infrastructure • Identity Management • Systems Administration • Networking • Automation • Homelab Engineering
