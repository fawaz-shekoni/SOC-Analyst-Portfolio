# 06 — Windows Firewall review

## Purpose

I reviewed the Windows Firewall configuration on my main Windows PC using read-only PowerShell commands. I wanted to understand which firewall profile was active and what the inbound rules showed. I did not change firewall settings.

This is a configuration review, not proof that a remote connection was attempted or that an application was listening.

## Investigation steps

1. I checked the Domain, Private, and Public firewall profile settings with `Get-NetFirewallProfile`.
2. I checked the active connection with `Get-NetConnectionProfile`.
3. I counted enabled inbound rules applying to the Public profile or to all profiles.
4. I reviewed the enabled inbound block rules and inspected their program, protocol, port, address, and service filters.
5. I sampled enabled inbound allow rules and inspected two Wi-Fi Direct rules that applied to the Public profile.

The first attempt to use `-Profile` directly with `Get-NetFirewallRule` returned a parameter error. I corrected the query by retrieving the active rules and filtering their `Profile` property with `Where-Object`.

## Evidence

### Firewall profiles and active connection

- Domain, Private, and Public profiles each showed `Enabled: True`.
- `DefaultInboundAction` and `DefaultOutboundAction` showed `NotConfigured` for all three profiles. This output did not establish the effective default actions, so I did not infer them.
- The active connection was the `Wi-Fi` interface, with `NetworkCategory: Public` and `IPv4Connectivity: Internet`.

### Inbound rule counts

For enabled inbound rules whose profile was Public or Any, the query returned:

| Action | Count |
| --- | ---: |
| Allow | 149 |
| Block | 3 |

These are counts of configured rules, not connection attempts or active network sessions.

### Inbound block rules

The three block entries were:

- Two `VLC media player` rules, both enabled and scoped to the Public profile.
- One `codex_sandbox_offline_block_inbound` rule, enabled and scoped to Any.

The two VLC rules came from the local persistent policy store. Their filters identified:

- Program: `C:\Program Files\VideoLAN\VLC\vlc.exe`
- Direction and action: inbound block
- Protocols: one TCP rule and one UDP rule
- Local and remote ports: Any

The Codex-named rule appeared in the active policy output. Its name suggests an environment-specific rule, but I did not independently verify its origin. I recorded it as a separate observation rather than treating it as suspicious evidence.

### Inbound allow rules

The sample included Windows networking rules for features such as DHCP, ICMP, Delivery Optimization, media services, and Wi-Fi Direct. Two enabled rules were explicitly scoped to the Public profile:

| Rule | Program | Service | Protocol / ports / remote address |
| --- | --- | --- | --- |
| Wi-Fi Direct Spooler Use (In) | `C:\Windows\System32\spoolsv.exe` | `Spooler` | Any / Any / Any |
| Wi-Fi Direct Scan Service Use (In) | `C:\Windows\System32\svchost.exe` | `stisvc` | Any / Any / Any |

The service filter ties each rule to a named Windows service. Microsoft identifies `stisvc` as the Windows Image Acquisition service used for scanners and cameras.

## Findings

The firewall profiles were enabled, and the active Wi-Fi connection was categorized as Public. The reviewed configuration included two Public-profile inbound allow rules for Wi-Fi Direct printing and scanning, along with separate Public-profile inbound block rules for VLC over TCP and UDP.

The Wi-Fi Direct rule filters showed Any for protocol/ports/address, but the rules were also tied to specific programs and services. I did not inspect every other possible rule condition, check whether these services were running, confirm whether a matching socket was listening, or review firewall traffic logs. Therefore, this evidence does not establish that the computer was reachable through those rules or that any inbound traffic occurred.

The rule names and paths are consistent with Windows printing and imaging features, but that alone does not prove the rules are required or appropriate for every Public network.

## Recommended resolution

I left the firewall configuration unchanged. If I needed to assess these rules further, I would confirm whether I use Wi-Fi Direct printing or scanning, inspect the remaining rule conditions and service state, and review firewall or endpoint logs for matching traffic. I would ask an administrator to narrow or disable a rule only if its purpose is not needed and the change is approved. I would not treat rule counts or broad filter fields alone as evidence of an attack.

## Evidence source and limitations

This report is based on the PowerShell outputs shown during my investigation. I transcribed the relevant values into this document; the screenshot files were not included as repository attachments in this submission. The observations are a point-in-time snapshot of this computer's configuration.
