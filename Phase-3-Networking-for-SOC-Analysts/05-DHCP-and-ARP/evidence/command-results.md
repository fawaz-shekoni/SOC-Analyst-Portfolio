# Command results transcribed from screenshots

These results come from my earlier `ipconfig /all` screenshot and my `arp -a` screenshot. The Wi-Fi interface address is changed to `192.168.0.200` in this public transcript; the original screenshot showed `192.168.0.121`. The gateway MAC address is retained as shown.

## Wi-Fi configuration from `ipconfig /all`

Relevant values shown in the earlier screenshot:

```text
Description: Intel(R) Wireless-AC 9462
DHCP Enabled: Yes
IPv4 Address: 192.168.0.121 (anonymized here as 192.168.0.200)
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.0.1
DHCP Server: 192.168.0.1
DNS Servers: 192.168.0.1
```

## ARP table from `arp -a`

The screenshot showed three interface tables. The relevant Wi-Fi table was:

```text
Interface: 192.168.0.121 (anonymized here as 192.168.0.200)
Internet Address  Physical Address     Type
192.168.0.1       0c-61-f9-86-7b-08    dynamic
192.168.0.255     ff-ff-ff-ff-ff-ff    static
224.0.0.2         01-00-5e-00-00-02    static
224.0.0.22        01-00-5e-00-00-16    static
224.0.0.251       01-00-5e-00-00-fb    static
224.0.0.252       01-00-5e-00-00-fc    static
239.255.255.250   01-00-5e-7f-ff-fa    static
255.255.255.255   ff-ff-ff-ff-ff-ff    static
```

Other interface headers visible in the screenshot were VMware adapters `192.168.196.1` and `192.168.206.1`. The `192.168.206.1` table included a dynamic entry for `192.168.206.254`; these virtual network entries are separate from the Wi-Fi table.

