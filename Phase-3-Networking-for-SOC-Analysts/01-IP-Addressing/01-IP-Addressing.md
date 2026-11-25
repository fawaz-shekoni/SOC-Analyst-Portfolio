# 01 — IP Addressing Investigation

## Purpose

I checked how my Windows PC was configured on its active Wi-Fi network and tested basic IP connectivity. This is a normal connectivity check, not an investigation of a suspected incident.

## Steps I performed

I ran these commands in PowerShell:

```cmd
ipconfig
ipconfig /all
ping 127.0.0.1
ping 192.168.0.1
ping 1.1.1.1
```

`ipconfig` showed the available adapters. In `ipconfig /all`, I used the **Wireless LAN adapter Wi-Fi** section for the active wireless connection. VMware network adapters were also listed, so I distinguished those virtual interfaces from the Wi-Fi adapter.

## Wi-Fi configuration I observed

| Field | Observed value | What it means |
|---|---|---|
| IPv4 address | `192.168.0.200`* | A private IPv4 address in the `192.168.0.0/16` range. |
| Subnet mask | `255.255.255.0` (`/24`) | The local IPv4 subnet is `192.168.0.0/24`; `.0` is the network address and `.255` is the broadcast address for this subnet. |
| Default gateway | `192.168.0.1` | The local next hop used to reach other networks. |
| DNS server | `192.168.0.1` | Windows is configured to send DNS queries to the router address. This output does not show which upstream resolver the router may use. |
| DHCP | Enabled; DHCP server shown as `192.168.0.1` | The address configuration is assigned automatically through DHCP. |

\* **Privacy edit:** I changed only the Wi-Fi device IPv4 value in the two published configuration screenshots from the exact address shown on my PC to `192.168.0.200`. This keeps the screenshot readable while avoiding publication of my exact device address. The replacement is an anonymized display value, not a claim about the exact address currently assigned to my PC. Other screenshot content is unchanged.

## Connectivity test results

| Test | Result | Interpretation |
|---|---|---|
| `ping 127.0.0.1` | 4 sent, 4 received, 0% loss; 0 ms average | The local TCP/IP loopback test replied successfully. |
| `ping 192.168.0.1` | 4 sent, 4 received, 0% loss; 10 ms average | The PC received replies from its configured local gateway. |
| `ping 1.1.1.1` | 4 sent, 4 received, 0% loss; 170 ms average | The PC reached this external IP address using ICMP at the time of the test. |

## Findings

My Wi-Fi adapter had a private IPv4 configuration, a `/24` subnet mask, and the same router address listed as both the default gateway and DNS server. DHCP was enabled. The VMware adapters were separate virtual interfaces and were not the active Wi-Fi interface I used for the connectivity check.

All three ping tests received four replies with no packet loss in this short test. This supports that the local stack, gateway path, and reachability to `1.1.1.1` were working when I ran the commands. The average response to `1.1.1.1` was 170 ms in this sample of four pings; this alone is not enough to diagnose a persistent latency problem.

## Limits and next checks

These pings do not test DNS name resolution, HTTPS access on port 443, browsing to multiple sites, throughput, or reliability over a longer period. A ping can also be blocked by a firewall or remote host, so a timeout would not by itself prove that internet access is down. I will investigate DNS separately in the DNS module with `nslookup`.

## Evidence

- [`ipconfig` screenshot](evidence/ipconfig.png)
- [`ipconfig /all` screenshot](evidence/ipconfig-all.png)
- [Ping results screenshot](evidence/ping-tests.png)

The two configuration screenshots show the anonymized Wi-Fi address `192.168.0.200`; the ping screenshot is an unchanged capture of the tests I ran.

