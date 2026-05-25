# HOMELAB – Samba Active Directory Lab

Built on Ubuntu Server running in UTM on Apple Silicon.

## Environment

Domain: HOMELAB.INTERNAL  
Hostname: labdc1  
IP: 192.168.64.10  
Gateway: 192.168.64.1  

Services:
- DNS (53)
- Kerberos / KDC (88)
- LDAP (389)
- SMB (445)

## Network Diagram

```text
                HOMELAB.INTERNAL
               192.168.64.0/24
                       │
               Gateway: .1
                       │
      ┌────────────────┴──────────────┐
      │                               │
┌─────────────┐             ┌────────────────┐
│ MacBook Pro │             │ Ubuntu Server │
│ UTM Host    │ SSH ------> │ labdc1        │
│             │             │ 192.168.64.10 │
└─────────────┘             │ Samba AD DC   │
                            │ DNS/KDC/LDAP  │
                            └────────────────┘
```

## Future

- [ ] Windows 11 client
- [ ] File server
- [ ] SIEM
- [ ] Kali VM
