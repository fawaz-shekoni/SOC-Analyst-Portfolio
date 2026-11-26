# Phase 3 — Networking for SOC Analysts

I am building on the networking knowledge I gained through Cisco NetAcad Junior Cybersecurity and Packet Tracer. This phase focuses on using network information as investigation evidence: understanding endpoints and services, tracing name resolution, and analyzing traffic without claiming more than the evidence shows.

## Progress

### 01 — IP addressing

- [IP Addressing Investigation](01-IP-Addressing/01-IP-Addressing.md)
- [Evidence screenshots](01-IP-Addressing/evidence/README.md)

The IP addressing check used `ipconfig`, `ipconfig /all`, and three ping tests. It showed successful replies to loopback, the local gateway, and `1.1.1.1` at the time of testing. The Wi-Fi device IPv4 value in the published screenshots is changed to `192.168.0.200` for privacy; this is identified as an anonymized display value, not the actual assigned address.

### 02 — Ports and protocols

- [Ports and Protocols Investigation](02-Ports-and-Protocols/02-Ports-and-Protocols.md)
- [Evidence screenshots](02-Ports-and-Protocols/evidence/README.md)

I reviewed five established TCP connections and mapped their owning process IDs. The snapshot showed Chrome, ChatGPT, and OneDrive connections to remote port 443, plus an `APSDaemon` connection to port 5223 that is consistent with Apple's documented APNs traffic. The local address in the published connection screenshot is anonymized; the report explains the limits of interpreting a single connection snapshot.

### 03 — OSI and TCP/IP communication

- [OSI and TCP/IP Communication Investigation](03-OSI-and-TCP-IP/03-OSI-and-TCP-IP.md)
- [Command evidence transcript](03-OSI-and-TCP-IP/evidence/command-results.md)

I traced a Microsoft website request from DNS lookup to a successful TCP connection on port 443, then followed its HTTP-to-HTTPS redirects to a final `200 OK`. The report maps each observation to the OSI and TCP/IP models and explains what the commands did not verify.

## Planned topics

01. IP addressing and subnetting
02. Ports and protocols
03. OSI and TCP/IP communication
04. DNS investigation
05. DHCP and ARP
06. Windows Firewall
07. Network traffic and Wireshark
08. TCP analysis
09. DNS packet analysis
10. HTTP, HTTPS, and TLS
11. Suspicious traffic investigation

I will add each investigation after completing the practical work and reviewing what the evidence means. Training captures and simulated scenarios will be labeled as such.

