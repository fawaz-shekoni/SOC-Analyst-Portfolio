# Windows Firewall practicals — evidence transcript

I ran the PowerShell commands below on my main Windows PC. The output excerpts are transcribed from the screenshots I shared. They are real configuration results from my computer; the alert example in Practical 5 is synthetic and is clearly labeled.

## Practical 1 — Check firewall status

Commands:

```powershell
Get-NetFirewallProfile
Get-NetFirewallProfile | Select-Object Name, Enabled, DefaultInboundAction, DefaultOutboundAction
```

Captured summary:

```text
Name     Enabled DefaultInboundAction DefaultOutboundAction
----     ------- -------------------- ---------------------
Domain   True    NotConfigured        NotConfigured
Private  True    NotConfigured        NotConfigured
Public   True    NotConfigured        NotConfigured
```

The full output also showed `LogAllowed: False` and `LogBlocked: False` for each profile.

## Practical 2 — Inspect a sample of firewall rules

Command:

```powershell
Get-NetFirewallRule | Select-Object -First 20 DisplayName, Enabled, Direction, Action
```

Captured output:

```text
DisplayName                                                        Enabled Direction Action
-----------                                                        ------- --------- ------
Network Discovery (UPnP-Out)                                       False   Outbound  Allow
Wi-Fi Direct Spooler Use (Out)                                     True    Outbound  Allow
Remote Assistance (TCP-Out)                                        False   Outbound  Allow
Network Discovery (SSDP-Out)                                       False   Outbound  Allow
Network Discovery (WSD Events-Out)                                 False   Outbound  Allow
Remote Event Log Management (NP-In)                                False   Inbound   Allow
Remote Scheduled Tasks Management (RPC)                            False   Inbound   Allow
Wi-Fi Direct Spooler Use (In)                                      True    Inbound   Allow
Remote Assistance (TCP-Out)                                        True    Outbound  Allow
Distributed Transaction Coordinator (TCP-Out)                      False   Outbound  Allow
Routing and Remote Access (L2TP-Out)                               False   Outbound  Allow
Core Networking - Packet Too Big (ICMPv6-Out)                      True    Outbound  Allow
Connected Devices Platform (UDP-Out)                               True    Outbound  Allow
Windows Collaboration Computer Name Registration Service (PNRP-In) False   Inbound   Allow
Network Discovery (NB-Name-Out)                                    False   Outbound  Allow
Network Discovery (NB-Datagram-Out)                                False   Outbound  Allow
Windows Peer to Peer Collaboration Foundation (WSD-In)             False   Inbound   Allow
Remote Event Log Management (RPC)                                  False   Inbound   Allow
Core Networking - IPv6 (IPv6-In)                                   True    Inbound   Allow
Connected Devices Platform - Wi-Fi Direct Transport (TCP-Out)     True    Outbound  Allow
```

Seven of these 20 displayed rules were enabled. The others were disabled even though their configured Action column said Allow.

## Practical 3 — Find enabled inbound rules

Command used:

```powershell
Get-NetFirewallRule -Direction Inbound -Enabled True |
    Select-Object Name, Profile, Action
```

Representative rows visible in my screenshot:

```text
Name                                      Profile          Action
----                                      -------          ------
WFDPRINT-SPOOL-In-Active                 Public           Allow
CoreNet-IPv6-In                          Any              Allow
DeliveryOptimization-UDP-In              Any              Allow
CoreNet-ICMP6-RA-In                      Any              Allow
CoreNet-ICMP4-DUFRAG-In                  Any              Allow
CoreNet-DHCP-In                          Any              Allow
CoreNet-DHCPV6-In                        Any              Allow
WFDPRINT-SCAN-In-Active                  Public           Allow
CoreNet-ICMP6-NDS-In                     Any              Allow
RemoteAssistance-DCOM-In-TCP-NoScope-Active Domain         Allow
```

The screenshot shows a portion of a longer output. `Name` is the internal rule name, so some entries are less readable than their DisplayName.

## Practical 4 — Find enabled outbound rules

Command:

```powershell
Get-NetFirewallRule -Enabled True -Direction Outbound |
    Select-Object DisplayName, Action, Profile
```

Representative rows visible in my screenshot:

```text
DisplayName                                         Action Profile
-----------                                         ------ -------
Wi-Fi Direct Spooler Use (Out)                     Allow  Public
Remote Assistance (TCP-Out)                        Allow  Domain, Private
Core Networking - Packet Too Big (ICMPv6-Out)       Allow  Any
Connected Devices Platform (UDP-Out)               Allow  Domain, Private
Connected Devices Platform - Wi-Fi Direct Transport (TCP-Out) Allow Public
mDNS (UDP-Out)                                      Allow  Domain
Core Networking - Parameter Problem (ICMPv6-Out)    Allow  Any
```

These are examples from the displayed output, not a claim that every outbound rule on the computer was listed here.

## Practical 5 — Simulated firewall alert

**Training data only — this alert was created for practice. It did not occur on my computer.**

Example alert fields used in the exercise:

```text
Direction: Inbound
Protocol: TCP
Remote address: 198.51.100.25 (documentation-only example address)
Local destination port: 50000 (simulated example)
Process/service: spoolsv.exe / Spooler
Network profile: Public
Alert claim: connection permitted
```

I compared the simulated alert with a real rule I had previously inspected on my PC:

```text
Rule: Wi-Fi Direct Spooler Use (In)
Enabled: True
Direction: Inbound
Action: Allow
Profile: Public
Program: C:\WINDOWS\system32\spoolsv.exe
Service: Spooler
Protocol / local port / remote port / remote address filters inspected: Any
```

The visible fields are consistent with a rule that could match the simulated scenario. I did not inspect every possible rule condition, so I cannot attribute even the simulated alert to that exact rule with certainty. The example does not prove a real connection occurred.

## What the evidence supports

- All three firewall profiles showed enabled in Practical 1.
- Practical 2 showed that an Allow action does not mean a rule is active when `Enabled` is False.
- Practicals 3 and 4 showed enabled inbound and outbound rule examples and their profiles.
- A rule applies only when traffic matches its conditions. Rule listings are configuration, not records of connections.
- I did not disable the firewall or change any rules.

## Evidence note

The transcript records commands and output values visible in my screenshots. The screenshot image files themselves are not attached to this repository version. The Practical 5 alert data is simulated and is not presented as a finding from my computer.
