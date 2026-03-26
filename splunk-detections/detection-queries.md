## Splunk Detection Queries

The following queries were used to detect simulated attacks based on pfSense firewall logs.

### Nmap Port Scan Detection
```spl
index=firewall sourcetype=pfsense
| stats count by src_ip, dest_ip, dest_port
| sort - count
```

---

### SMB Brute Force / SMB Activity Detection
```spl
index=firewall ("445" OR "137")
| stats count by src_ip, dest_ip
| sort - count
```

---

### Suspicious SMB Exploitation (EternalBlue)
```spl
index=firewall sourcetype=pfsense source=udp:514
| stats count by src_ip, dest_ip, action
```

---

## Detection Approach

All detections are based on analyzing raw pfsense logs for:

- High number of connection attempts from a single source IP  
- Repeated access to critical ports (e.g., 445)  
- Unusual traffic patterns indicating scanning or exploitation  
