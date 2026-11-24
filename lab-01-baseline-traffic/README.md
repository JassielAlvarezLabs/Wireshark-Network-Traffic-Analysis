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
but I cant see ACK packets since the SYN flag is turned off.

Every TCP (Transmission Control Protocol) connection starts with 3 packets:
 
1. **SYN** – my computer: "I want to connect"
2. **SYN/ACK** – the server: "OK, ready"
3. **ACK** – my computer: "Connected"
