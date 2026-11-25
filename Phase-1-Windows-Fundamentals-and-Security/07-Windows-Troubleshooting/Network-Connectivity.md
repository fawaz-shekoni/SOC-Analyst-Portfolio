# Scenario 3: Network connectivity problem

## Problem

Practice scenario: check whether basic web connectivity is available. This was a connectivity check on the main PC, not a speed or reliability test.

## Investigation steps

1. Reviewed Windows quick settings for Wi-Fi and Airplane mode.
2. Opened a Microsoft webpage in the browser.
3. Compared the result with Task Manager's network activity snapshot, noting that it was idle at capture.

## Evidence

- [Wi-Fi status (network name removed)](evidence/wifi-status-redacted.png): the Wi-Fi quick setting was active and Airplane mode was off.
- [Microsoft webpage loaded](evidence/connectivity-page.png): the Microsoft home page content rendered in the browser.
- The Task Manager snapshot showed 0 Kbps at the instant it was captured, which is normal while the connection is idle and does not indicate a fault by itself.

## Findings

The evidence shows that Wi-Fi was enabled and one Microsoft webpage loaded at the time of the check, so basic connectivity was working then. It does not assess slow speeds, intermittent drops, latency, packet loss, DNS problems affecting other sites, or connectivity at other times. The earlier report of a slow internet experience during a VMware update is not treated as a test of the main PC.

## Recommended resolution

If I run into a connectivity problem again, I can investigate it without disconnecting Wi-Fi or changing network settings. I would work through these read-only checks in CMD and PowerShell, recording the results and the time of the issue.

### 1. Check the network configuration

In CMD, I would run:

```cmd
ipconfig /all
```

I would focus on the active Wi-Fi adapter and check whether it has an IPv4 address, subnet mask, default gateway, and DNS servers. If I save or share a screenshot, I would redact IP addresses and other network details.

### 2. Test each part of the connection

I would run these checks:

```cmd
ping 127.0.0.1
ping 1.1.1.1
nslookup microsoft.com
```

Then, in PowerShell, I would check an HTTPS connection:

```powershell
Test-NetConnection microsoft.com -Port 443
```

### 3. Interpret the results

| Check | What it tells me |
| --- | --- |
| `ipconfig /all` | Shows adapter configuration, including the assigned IP address, gateway, and DNS servers. |
| `ping 127.0.0.1` | Checks the local TCP/IP loopback path on my computer. |
| `ping 1.1.1.1` | Checks whether an external IP address responds to ICMP. A failed ping alone does not prove internet access is down because ICMP may be filtered. |
| `nslookup microsoft.com` | Checks whether DNS can resolve a domain name to an IP address. |
| `Test-NetConnection microsoft.com -Port 443` | Checks whether the hostname resolves and a TCP connection to HTTPS port 443 succeeds; I would look at `TcpTestSucceeded`. |

If the local loopback test works and the PC has an address and gateway, but external tests fail, I would investigate the path beyond the PC and compare with another device on the same Wi-Fi. If an external IP test succeeds but name lookup fails, I would treat DNS as a possible issue and confirm with more than one lookup before changing anything. If name lookup succeeds but the port 443 test fails, I would check whether the issue affects other websites or only that destination. I would not treat one failed ping as conclusive because some networks block ping replies.

### 4. Example of how I would reason through a possible DNS issue

This is a simulated example, not a result from my current connection:

- Wi-Fi is connected.
- The active adapter has an IPv4 address and a default gateway.
- An external IP responds to ping.
- `nslookup microsoft.com` fails.

That pattern would make DNS a possible area to investigate, rather than indicating a complete loss of network connectivity. I would repeat the lookup and compare another domain or DNS server as a diagnostic step before recommending a change.

For command details, I would refer to Microsoft's documentation for [ipconfig](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig), [ping](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ping), [nslookup](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/nslookup), and [Test-NetConnection](https://learn.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection).

I have not changed network settings for this investigation.
