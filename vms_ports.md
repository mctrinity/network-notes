## Commonly Used Ports for Network Connections

### **1. Remote Access Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 22    | SSH      | Secure Shell for remote login (Linux & Windows if enabled) |
| 3389  | RDP      | Remote Desktop Protocol (Windows) |
| 23    | Telnet   | Unsecure remote login (deprecated) |
| 5985  | WinRM    | Windows Remote Management (HTTP) |
| 5986  | WinRM    | Windows Remote Management (HTTPS) |
| 3128  | Squid Proxy | Used for proxy connections |
```

### **2. Web Traffic Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 80    | HTTP     | Standard web browsing (insecure) |
| 443   | HTTPS    | Secure web browsing (TLS/SSL) |
| 8080  | HTTP-Alt | Alternative web traffic port |
| 8443  | HTTPS-Alt | Alternative secure web traffic |
```

### **3. File Transfer Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 21    | FTP      | File Transfer Protocol (control channel) |
| 20    | FTP-Data | File Transfer Protocol (data channel) |
| 22    | SFTP     | Secure FTP (via SSH) |
| 989/990 | FTPS   | Secure FTP over SSL/TLS |
| 69    | TFTP     | Trivial File Transfer Protocol (used in PXE boot) |
```

### **4. Email Communication Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 25    | SMTP     | Sending emails (unsecure, often blocked) |
| 465   | SMTPS    | Secure SMTP with SSL |
| 587   | SMTP     | Secure email submission |
| 110   | POP3     | Retrieving emails (unsecure) |
| 995   | POP3S    | Secure POP3 |
| 143   | IMAP     | Retrieving emails (multi-device support) |
| 993   | IMAPS    | Secure IMAP |
```

### **5. Database Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 1433  | MS SQL   | Microsoft SQL Server |
| 3306  | MySQL    | MySQL Database Server |
| 1521  | Oracle DB | Oracle Database |
| 5432  | PostgreSQL | PostgreSQL Database |
| 27017 | MongoDB  | MongoDB NoSQL Database |
| 6379  | Redis    | Redis Database |
| 50000 | DB2      | IBM DB2 Database |
```

### **6. Directory Services & Authentication Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 389   | LDAP     | Lightweight Directory Access Protocol |
| 636   | LDAPS    | Secure LDAP over SSL |
| 88    | Kerberos | Kerberos Authentication |
| 1812/1813 | RADIUS | Authentication and accounting |
| 49    | TACACS+  | Cisco authentication protocol |
```

### **7. Networking, DNS, & VPN Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 53    | DNS      | Domain Name System (UDP & TCP) |
| 67/68 | DHCP     | Dynamic Host Configuration Protocol |
| 1194  | OpenVPN  | OpenVPN service |
| 500   | IPsec    | VPN with IPsec/IKE |
| 4500  | NAT-T    | IPsec NAT Traversal |
| 1723  | PPTP     | Point-to-Point Tunneling Protocol |
| 1701  | L2TP     | Layer 2 Tunneling Protocol |
```

### **8. Messaging, Streaming, & VoIP Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 5060/5061 | SIP | VoIP signaling (5060 for unencrypted, 5061 for TLS) |
| 5004/5005 | RTP | Real-time Transport Protocol (audio/video streaming) |
| 1935  | RTMP    | Real-Time Messaging Protocol (streaming) |
| 3478/5349 | STUN/TURN | WebRTC services for VoIP |
| 5222/5223 | XMPP | Jabber messaging |
| 1863  | MSN Messenger | Old messaging service |
```

### **9. IoT & Industrial Communication Ports**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 1883  | MQTT     | Message Queue Telemetry Transport (IoT messaging) |
| 8883  | MQTT over TLS | Secure MQTT |
| 502   | Modbus   | Modbus Protocol for industrial communication |
| 47808 | BACnet   | Building Automation & Control Networks |
```

### **10. Miscellaneous & Other Protocols**
```markdown
| Port  | Protocol | Usage                        |
|-------|----------|----------------------------|
| 123   | NTP      | Network Time Protocol |
| 161/162 | SNMP  | Simple Network Management Protocol |
| 2049  | NFS      | Network File System |
| 445   | SMB      | Server Message Block (file sharing, Windows networking) |
| 9100  | JetDirect | Network printing (HP printers) |
```

### **Remote Connection Options for Azure VMs**
```markdown
| Method | OS | Public IP Required? | Description |
|--------|----|--------------------|-------------|
| SSH    | Linux & Windows (if enabled) | ✅ Yes | Secure shell remote login |
| RDP    | Windows | ✅ Yes | Remote Desktop Protocol |
| Azure Bastion | Linux & Windows | ❌ No | Secure browser-based access |
| Jumpbox VM | Linux & Windows | ❌ No | Intermediary VM for secure access |
| PowerShell Direct | Windows | ❌ No | Direct PowerShell connection in Azure |
| VPN/ExpressRoute | Linux & Windows | ❌ No | Private network access |
```
