# Open Source Philosophy

This homelab is intentionally built using free and open-source technologies whenever possible.

The goal is to develop enterprise infrastructure, identity management, and automation skills without relying on expensive commercial licensing.

## Technologies Used

| Technology | Purpose | License |
|------------|---------|---------|
| Ubuntu Server | Infrastructure Platform | Open Source |
| Samba AD DC | Active Directory Services | Open Source |
| Kerberos | Authentication | Open Source |
| OpenLDAP Components | Directory Services | Open Source |
| OpenSSH | Remote Administration | Open Source |
| Ansible | Automation | Open Source |
| Git | Version Control | Open Source |
| VS Code | Development | Free |
| Apache Directory Studio | LDAP Management | Open Source |

## Commercial Software Usage

The only commercial operating system currently used in the lab is:

- Windows 11 Pro (client workstation)

Windows is included solely to test interoperability with Active Directory, Group Policy, RSAT, and enterprise endpoint management.

All core infrastructure services are provided by open-source software.

## Design Goals

- Learn enterprise infrastructure using freely available tools
- Build reproducible environments
- Automate deployments using Infrastructure as Code
- Document implementation and troubleshooting processes
- Minimize dependency on proprietary solutions
