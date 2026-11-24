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
 
## What I Did
I turned on a simple web server on Computer B to act as the C2 server. Then I started a Wireshark capture on Computer A and ran `curl` about every 10 seconds to act like malware checking in. After that, I stopped the capture and looked at the timing.

---
 
## 1. Start the Fake C2 Server (Computer B)
```
mkdir C:\beacon
cd C:\beacon
py -m http.server 80
```
These make an empty folder, go into it, and turn on a simple web server on port 80 (HTTP).
 
![C2 server](screenshots/01-c2-server.png)
 
---

## 2. Fake Malware Check-Ins (Computer A)
```
curl 192.168.50.216
```
I ran this about every 10 seconds. Each time, it asks Computer B's web server for a page, like malware checking in/beaconing
 
![Check-ins](screenshots/02-check-ins.png)
 
---
