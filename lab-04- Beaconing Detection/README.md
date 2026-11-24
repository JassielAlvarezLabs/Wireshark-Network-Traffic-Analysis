# Lab 4: Beaconing Detection
 
## Goal
Catch fake malware "checking in" with an attacker's server on a regular schedule.
 
Infected computers often contact their C2 (Command and Control) server at regular times. This is called **beaconing**.

**MITRE ATT&CK:** T1071.001 – Application Layer Protocol: Web Protocols
 
---

## My Setup
 
| Role | IP |
|---|---|
| Computer A ("infected" computer + Wireshark) | 192.168.50.142 |
| Computer B (fake C2 server) | 192.168.50.216 |
 
---
