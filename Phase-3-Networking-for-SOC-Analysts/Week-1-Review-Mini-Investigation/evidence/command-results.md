# Week 1 mini-investigation — command evidence

The alert in this exercise is simulated. The PowerShell results below are real snapshots from my main Windows PC. I transcribed the relevant fields from the screenshots I shared.

For privacy, my local Wi-Fi IPv4 address `192.168.0.121` is shown as `192.168.0.200` in this report. This is an anonymized display value, not the address assigned during the checks.

## 1. IP configuration

Command:

```powershell
ipconfig /all
```

Relevant fields from the active Wi-Fi adapter:

```text
Adapter: Intel(R) Wireless-AC 9462
DHCP Enabled: Yes
IPv4 Address: 192.168.0.200 (anonymized display value)
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.0.1
DHCP Server: 192.168.0.1
DNS Server: 192.168.0.1
```

VMware virtual adapters also appeared in the output; they are separate from the active Wi-Fi adapter.

## 2. DNS lookup for Facebook

Command:

```powershell
nslookup facebook.com
```

Captured output:

```text
Server:  UnKnown
Address: 192.168.0.1

Non-authoritative answer:
Name:    facebook.com
Addresses:
         2a03:2880:f310:1:face:b00c:0:25de
         57.144.38.1
```

The DNS lookup returned IPv6 and IPv4 addresses. The resolver name was shown as Unknown, but it still returned an answer.

## 3. General external IP connectivity

Command:

```powershell
ping 1.1.1.1
```

Captured summary:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Minimum = 136ms, Maximum = 155ms, Average = 145ms
```

## 4. Local ARP table

Command:

```powershell
arp -a
```

Relevant Wi-Fi interface entries (interface address anonymized):

```text
Interface: 192.168.0.200

Internet Address  Physical Address  Type
192.168.0.1       0c-61-f9-86-7b-08 dynamic
192.168.0.107     18-4f-32-8d-94-7b dynamic
```

The output also included static broadcast and multicast entries and separate VMware interfaces.

## 5. Established TCP connections

Command:

```powershell
Get-NetTCPConnection -State Established
```

Representative rows from the captured output (local Wi-Fi address anonymized):

```text
LocalAddress   LocalPort  RemoteAddress    RemotePort  State        OwningProcess
192.168.0.200  65473      52.110.18.77     443         Established  12348
192.168.0.200  64475      104.18.39.21     443         Established  14436
192.168.0.200  62711      172.64.155.209   443         Established  15224
192.168.0.200  59998      20.44.8.229      8883        Established  6628
192.168.0.200  57137      17.57.146.184    5223        Established  20888
192.168.0.200  54666      192.168.0.107    8009        Established  11304
127.0.0.1      52629      127.0.0.1        52628       Established  21252
```

This is a point-in-time excerpt. The screenshot contained additional connections. The owning process IDs were not mapped to process names during this mini-investigation.

## 6. Facebook HTTPS-port connectivity

Command:

```powershell
Test-NetConnection facebook.com -Port 443
```

Captured output:

```text
ComputerName     : facebook.com
RemoteAddress    : 57.144.38.1
RemotePort       : 443
InterfaceAlias   : Wi-Fi
SourceAddress    : 192.168.0.200 (anonymized display value)
TcpTestSucceeded : True
```

## Evidence limitations

- The simulated alert did not provide the exact unfamiliar external IP, timestamp, port, or process, so I could not correlate it to a specific connection.
- An established connection and its IP/port do not identify the owning application or establish that it is malicious.
- `ping 1.1.1.1` checks reachability to that IP, not Facebook specifically.
- DNS success and a successful TCP connection to port 443 do not prove that Facebook's page rendered in a browser.
- These results are point-in-time snapshots. No actual alert or Facebook outage was confirmed in this exercise.
