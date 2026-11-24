# Lab 1: Normal traffic

## Goal
learn what normal traffic looks like, so I can spot bad traffic later.

---

## What I Did
I started a Wireshark capture on Computer A ('JayPC' , 192.168.40.142), browsed 2-3 websites for about 60 seconds, then stopped the capture.

---
 
## 1. Only My Computer's Traffic
```
ip.addr == 192.168.50.142
```
This shows only Computer A's traffic.

---
 
## 2. DNS Lookups
```
dns
```
DNS (Domain Name System) is how my computer asks for a website's IP address.
 
<!-- TODO: name 2–3 websites you saw in the DNS lookups -->
I saw lookups for: Discord.com, Claude.ai, Spotify.com, mozilla.cloudflare-dns.com as im using Firefox as a browser
 
![DNS lookups](screenshots/01-dns-lookups.png)

## 3. TCP Handshake
```
tcp.flags.syn == 1
```
This filter allows me to see the SYN or SYN-ACK packets that were sent.
but I can't see ACK packets since the SYN flag is turned off.

Every TCP (Transmission Control Protocol) connection starts with 3 packets:
 
1. **SYN** – my computer: "I want to connect"
2. **SYN/ACK** – the server: "OK, ready"
3. **ACK** – my computer: "Connected"
   
I found all 3 between my computer (192.168.50.142:53680) and a server (172.64.154.50) on port 443 (HTTPS):
 
| Packet | From → To | What it is |
|---|---|---|
| 370 | 192.168.50.142 → 172.64.154.50 | **SYN** |
| 377 | 172.64.154.50 → 192.168.50.142 | **SYN/ACK** (about 9 ms later) |
| 378 | 192.168.50.142 → 172.64.154.50 | **ACK** – connection is open |
| 379 | 192.168.50.142 → 172.64.154.50 | **Client Hello** for `media.licdn.com` – the HTTPS part starts |

![TCP handshake](screenshots/02-tcp-handshake.png)

## 4. Website Names on HTTPS
```
tls.handshake.type == 1
```
Even though HTTPS is encrypted, the **Client Hello** packet shows the website name (called the **SNI**, Server Name Indication).
 
I found the name **media.licdn.com** and **static.licdn.com** (LinkedIn's content server).
 
![TLS server name](screenshots/03-tls-server-name.png)

---
 
## 5. Statistics → Conversations
This shows who talked to whom, and how much.
 
I used the **IPv4 tab**, which shows one row per server my computer talked to. My computer talked to **45 servers** (72 TCP connections and 32 UDP conversations).
 
My top 3 by **Total Packets**:
 
| Server | Likely owner | Total packets | Duration |
|---|---|---|---|
| 209.85.231.177 | Google | 52,840 | ~116 s |
| 18.64.174.117 | Amazon AWS | 28,078 | ~37 s |
| 74.125.7.227 | Google | 18,122 | ~115 s |

The servers I checked belonged to big, well-known companies like **Google**, and **Amazon AWS**.

Went to **www.abuseipdb.com** to confirm these are valid IP addresses

all 3 were from either Google server or Amazon server.

![IP lookup](screenshots/05-GoogleIP.png)
![IP lookup 2](screenshots/06-AmazonIP.png)
![IP lookup 3](screenshots/07-GoogleIP2.png)

## What Normal Looks Like
- DNS lookup first, then a connection to that website
- Every connection starts with SYN → SYN/ACK → ACK
- Traffic goes to well-known companies on port 443 (HTTPS)

## What I Learned
- How to filter traffic in Wireshark
- How DNS, the TCP handshake, and the TLS Client Hello work
- How to find the busiest connections with Statistics → Conversations
- How to verify IP addresses using **www.abuseipdb.com**
