# 05 — DHCP and ARP

## Objective

I reviewed my Wi-Fi IP configuration and ARP cache to understand two different parts of local networking: how DHCP supplies network settings to a device, and how ARP finds the link-layer address for a device on the local IPv4 network.

## What I checked

1. I reviewed the `ipconfig /all` output from my earlier IP-addressing check.
2. I ran `arp -a` to display the ARP cache for all network interfaces.
3. I identified the table for my Wi-Fi interface and checked the entry for my default gateway.

The results transcribed from my screenshots are in [evidence/command-results.md](evidence/command-results.md). The local Wi-Fi address is shown as `192.168.0.200` in the public transcript; my screenshot showed `192.168.0.121`. The gateway MAC address and other output are preserved as displayed.

## DHCP findings

My earlier `ipconfig /all` screenshot showed that DHCP was enabled on Wi-Fi. It listed `192.168.0.1` as the DHCP server and default gateway, and also showed my Wi-Fi IPv4 address, subnet mask, DNS server, and lease information.

DHCP is used to provide a device with network configuration, such as an IP address, subnet mask, default gateway, and DNS server. The output shows that my adapter was configured to use DHCP at the time of capture. It does not, by itself, show the entire DHCP exchange between my PC and the router.

## ARP findings

In the `arp -a` output, the interface corresponding to my Wi-Fi adapter was `192.168.0.121` (shown as `192.168.0.200` in this public transcript). Its table included:

| Internet address | Physical address | Type | Interpretation |
|---|---|---|---|
| `192.168.0.1` | `0c-61-f9-86-7b-08` | Dynamic | The gateway's local IPv4 address was associated with a link-layer/MAC address in the ARP cache. |
| `192.168.0.255` | `ff-ff-ff-ff-ff-ff` | Static | Subnet broadcast mapping. |
| `224.0.0.2`, `224.0.0.22`, `224.0.0.251`, `224.0.0.252`, `239.255.255.250`, `255.255.255.255` | Multicast/broadcast MAC mappings | System protocol entries, not additional individual devices. |

The `Dynamic` gateway entry indicates it was learned and stored in the cache, rather than manually configured as a permanent entry. ARP resolves local IPv4 neighbors to link-layer addresses. For traffic to an off-network destination, my PC sends frames to its next-hop gateway, so I would expect to resolve the gateway on the local network rather than directly ARP for a public server address.

The output also showed separate tables for VMware interfaces `192.168.196.1` and `192.168.206.1`. These are virtual network interfaces on the PC and are distinct from the Wi-Fi interface. Their presence in this output is expected given that VMware adapters are installed.

## DHCP and ARP compared

| DHCP | ARP |
|---|---|
| Provides network configuration to a client, often including its IPv4 address, gateway, DNS server, and lease. | Resolves an IPv4 address on the local network to a link-layer/MAC address. |
| My `ipconfig /all` output showed DHCP enabled and listed the DHCP server. | My `arp -a` output showed a dynamic mapping from the gateway IP to its MAC address. |

## Conclusion

My evidence shows that Wi-Fi was configured to use DHCP and that the ARP cache had a dynamic mapping for the default gateway. The ARP output contained multiple interfaces because this PC has VMware virtual adapters. These observations are consistent with normal local networking; they do not show a fault or suspicious activity by themselves.

## Recommended next steps

If I were troubleshooting a device that could not reach the local network, I would first check whether it has a valid IPv4 address, subnet mask, gateway, and DHCP server in `ipconfig /all`. I would then check whether the gateway appears in the ARP table. If the gateway mapping were missing or incomplete, I would repeat the observation and compare it with connectivity symptoms before recommending any change. I would not clear the ARP cache or release/renew the DHCP lease during this evidence-gathering exercise.

## Sources

- Microsoft Learn, [`arp` command](https://learn.microsoft.com/windows-server/administration/windows-commands/arp) — describes `arp -a` and the per-interface cache of IP-to-physical-address mappings.
- Microsoft Learn, [`ipconfig` command](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig) — describes the Windows IP configuration display command.

## Evidence

- [Command results transcribed from my screenshots](evidence/command-results.md)
- [Evidence notes](evidence/README.md)

