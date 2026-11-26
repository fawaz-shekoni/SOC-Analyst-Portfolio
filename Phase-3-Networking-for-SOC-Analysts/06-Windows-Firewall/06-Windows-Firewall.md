# 06 — Windows Firewall

## Project status

I am restarting this module with the practicals in order so I can explain the evidence myself. I am keeping the work read-only; I will not randomly disable firewall rules or turn off the firewall.

- **Practical 1 — Check firewall status:** complete
- **Practical 2 — Inspect firewall rules:** not documented in this restart yet
- **Practical 3 — Find enabled inbound rules:** not documented in this restart yet
- **Practical 4 — Find enabled outbound rules:** not documented in this restart yet
- **Practical 5 — Investigate a simulated firewall alert:** not documented in this restart yet

## Firewall fundamentals

A firewall checks network traffic against rules.

- **Inbound traffic** is traffic trying to reach my computer.
- **Outbound traffic** is traffic leaving my computer.
- An **Allow** rule permits traffic that matches its conditions.
- A **Block** rule stops traffic that matches its conditions.
- A rule can apply to a program or service, protocol, port, address, direction, and network profile.
- A rule describes what the firewall should do if traffic matches. The rule alone does not prove that traffic occurred.

Windows has three firewall profiles:

- **Domain** is for a computer connected to a managed organization network.
- **Private** is for a network I have identified as trusted, such as my home network.
- **Public** is for a network treated as untrusted, such as public Wi-Fi.

The profile tells Windows which set of rules applies to the current network category. A Public profile does not by itself prove that the computer is reachable from the internet.

## Practical 1 — Check firewall status

I ran `Get-NetFirewallProfile` in PowerShell. I also used a selected view to compare the profile status and default action fields.

- [Practical 1 command and captured output](evidence/command-results.md)

### My result

| Profile | Enabled | Default inbound action | Default outbound action |
| --- | --- | --- | --- |
| Domain | True | NotConfigured | NotConfigured |
| Private | True | NotConfigured | NotConfigured |
| Public | True | NotConfigured | NotConfigured |

### What I understand from the result

All three firewall profiles showed as enabled at the time of the check. The default action fields showed `NotConfigured`; I do not interpret that as the firewall being off or guess what the effective defaults are from this output alone.

This command checks firewall profile configuration. It does not show that a connection was attempted, what traffic was blocked, or whether an attack happened. I made no firewall changes.

## Remaining practicals

I will add each section after I complete and understand it:

1. Inspect a sample of firewall rules.
2. Review enabled inbound rules.
3. Review enabled outbound rules.
4. Work through a clearly labeled simulated alert by identifying the port, application or service, matching rule, direction, and profile. I will not present the simulated alert as a real event from my computer.

## Evidence note

The linked transcript records the command and values shown in my PowerShell screenshots. The screenshot image files themselves are not attached to this repository version.
