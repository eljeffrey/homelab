# HOMELAB

## Environment

Domain: HOMELAB.INTERNAL  
Hostname: labdc1  
IP: 192.168.64.10  
Gateway: 192.168.64.1  

Services:
- DNS (53)
- Kerberos (88)
- LDAP (389)
- SMB (445)

## Diagram

```text
                HOMELAB.INTERNAL
               192.168.64.0/24
                       │
               Gateway: .1
                       │
      ┌────────────────┴──────────────┐
      │                               │
┌─────────────┐             ┌────────────────┐
│ MacBook Pro │ SSH ------> │ labdc1         │
│ UTM Host    │             │ 192.168.64.10  │
└─────────────┘             │ Samba AD DC    │
                            │ DNS/KDC/LDAP   │
                            └────────────────┘
```

## Future
- [ ] Windows 11 client
- [ ] File server
- [ ] Kali VM
