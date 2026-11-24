# Wireshark Network Traffic Analysis
 
A home lab I built to learn how a SOC analyst reads network traffic. I used two computers on my home network: one captures traffic with Wireshark, the other acts as the "attacker" or server. For each lab I create the activity, find it in Wireshark, and write up what I found.
 
> I'm new to security and built this to practice real skills: reading packets, learning what normal traffic looks like, and spotting traffic that isn't normal.

---
 
## My Setup
 
| Role | Name | IP |
|---|---|---|
| Wireshark computer | Computer A (`JayPC`) | 192.168.50.142 |
| "Attacker" / server | Computer B | 192.168.50.216 |

```
 Computer B  (scan, FTP login, check-ins) | Computer A (Wireshark)(web traffic) Internet
 192.168.50.216                                  192.168.50.142
```
 
---
 
## Labs

### [Lab 1: Normal Traffic](lab-01-baseline-traffic/)
I captured 60 seconds of normal browsing to learn what normal looks like: DNS lookups, the TCP handshake, website names in HTTPS traffic, and who my computer talked to the most. I checked the top IPs on AbuseIPDB.

### [Lab 2: Port Scan](lab-02-port-scan-detection/)
I scanned Computer A with Nmap from Computer B and caught it in Wireshark: one IP sending SYN packets to hundreds of different ports. Nmap found 4 open ports (135, 2179, 5357, 8000).
**MITRE ATT&CK:** T1046 – Network Service Discovery

### [Lab 3: Stolen Passwords](lab-03-cleartext-credentials/)
I logged in to a test FTP server with a fake password and found the password in plain text in Wireshark, then read the whole login with Follow → TCP Stream.
**MITRE ATT&CK:** T1040 – Network Sniffing

### [Lab 4: Beaconing Detection](lab-04-%20Beaconing%20Detection/)
I made Computer A "check in" with a fake C2 (Command and Control) server about every 10 seconds, like malware does, then caught the regular timing and the non-browser User-Agent in Wireshark.
**MITRE ATT&CK:** T1071.001 – Application Layer Protocol: Web Protocols
