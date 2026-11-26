# Windows Firewall — command evidence

I ran these read-only commands in PowerShell on my main Windows PC. The result transcripts below are transcribed from the screenshots I shared during this investigation. I have preserved the values relevant to the analysis and omitted no finding-changing details.

## 1. Review firewall profile settings

Command:

```powershell
Get-NetFirewallProfile | Select-Object Name, Enabled, DefaultInboundAction, DefaultOutboundAction
```

Captured output:

```text
Name     Enabled DefaultInboundAction DefaultOutboundAction
----     ------- -------------------- ---------------------
Domain   True    NotConfigured        NotConfigured
Private  True    NotConfigured        NotConfigured
Public   True    NotConfigured        NotConfigured
```

## 2. Identify the active network profile

Command:

```powershell
Get-NetConnectionProfile | Select-Object InterfaceAlias, NetworkCategory, IPv4Connectivity
```

Captured output:

```text
InterfaceAlias NetworkCategory IPv4Connectivity
-------------- --------------- ----------------
Wi-Fi          Public          Internet
```

## 3. Count enabled inbound rules applying to Public or Any

My first attempt used `-Profile Public` directly with `Get-NetFirewallRule`, which returned a parameter error because that parameter is not supported by this cmdlet. I corrected the query by filtering the retrieved rule objects' `Profile` property.

Command:

```powershell
Get-NetFirewallRule -Direction Inbound -Enabled True |
    Where-Object Profile -match 'Any|Public' |
    Group-Object Action -NoElement
```

I reran the filter with this shorter command. It returned the same counts as my earlier ActiveStore query.

Captured output:

```text
Count Name
----- ----
149   Allow
3     Block
```

## 4. Review enabled inbound block rules

Command:

```powershell
Get-NetFirewallRule -PolicyStore ActiveStore -Direction Inbound -Enabled True |
    Where-Object {
        ($_.Profile -eq 'Any' -or $_.Profile -match 'Public') -and
        $_.Action -eq 'Block'
    } |
    Select-Object DisplayName, Profile, Action, Direction, Enabled
```

Captured output:

```text
DisplayName                         Profile Action Direction Enabled
-----------                         ------- ------ --------- -------
VLC media player                    Public  Block  Inbound   True
VLC media player                    Public  Block  Inbound   True
codex_sandbox_offline_block_inbound Any     Block  Inbound   True
```

## 5. Check the source of the VLC block rules

Command:

```powershell
Get-NetFirewallRule -PolicyStore ActiveStore -DisplayName "VLC media player" |
    Select-Object DisplayName, Name, Profile, Direction, Action, Enabled,
                  PolicyStoreSource, PolicyStoreSourceType
```

Captured output (rule names shortened to the protocol and executable path; the original screenshot showed separate per-rule identifiers):

```text
DisplayName: VLC media player
Name:        TCP Query User {rule identifier} C:\program files\videolan\vlc\vlc.exe
Profile:     Public
Direction:   Inbound
Action:      Block
Enabled:     True
PolicyStoreSource:     PersistentStore
PolicyStoreSourceType: Local

DisplayName: VLC media player
Name:        UDP Query User {rule identifier} C:\program files\videolan\vlc\vlc.exe
Profile:     Public
Direction:   Inbound
Action:      Block
Enabled:     True
PolicyStoreSource:     PersistentStore
PolicyStoreSourceType: Local
```

## 6. Inspect protocol and port filters for VLC

Command:

```powershell
Get-NetFirewallRule -PolicyStore ActiveStore -DisplayName "VLC media player" |
    ForEach-Object {
        $rule = $_
        $ports = $rule | Get-NetFirewallPortFilter
        [pscustomobject]@{
            Rule       = $rule.Name
            Protocol   = $ports.Protocol
            LocalPort  = $ports.LocalPort -join ','
            RemotePort = $ports.RemotePort -join ','
        }
    } |
    Format-List
```

Captured output:

```text
Rule:       TCP Query User {rule identifier} C:\program files\videolan\vlc\vlc.exe
Protocol:   TCP
LocalPort:  Any
RemotePort: Any

Rule:       UDP Query User {rule identifier} C:\program files\videolan\vlc\vlc.exe
Protocol:   UDP
LocalPort:  Any
RemotePort: Any
```

## 7. Sample enabled inbound allow rules applying to Public or Any

Command:

```powershell
Get-NetFirewallRule -PolicyStore ActiveStore -Direction Inbound -Enabled True |
    Where-Object {
        ($_.Profile -eq 'Any' -or $_.Profile -match 'Public') -and
        $_.Action -eq 'Allow'
    } |
    Select-Object -First 15 DisplayName, Profile, Action, PolicyStoreSourceType
```

Captured output:

```text
DisplayName                                                       Profile Action PolicyStoreSourceType
-----------                                                       ------- ------ ---------------------
Wi-Fi Direct Spooler Use (In)                                     Public  Allow  Local
Core Networking - IPv6 (IPv6-In)                                  Any     Allow  Local
Delivery Optimization (UDP-In)                                    Any     Allow  Local
Core Networking - Router Advertisement (ICMPv6-In)                Any     Allow  Local
Core Networking - Destination Unreachable Fragmentation Needed
  (ICMPv4-In)                                                     Any     Allow  Local
Core Networking - Dynamic Host Configuration Protocol (DHCP-In)   Any     Allow  Local
Core Networking - Dynamic Host Configuration Protocol for IPv6
  (DHCPV6-In)                                                     Any     Allow  Local
Wi-Fi Direct Scan Service Use (In)                                Public  Allow  Local
Core Networking - Neighbor Discovery Solicitation (ICMPv6-In)     Any     Allow  Local
Microsoft Media Foundation Network Source IN [UDP 5004-5009]      Any     Allow  Local
Delivery Optimization (TCP-In)                                    Any     Allow  Local
Core Networking - Router Solicitation (ICMPv6-In)                 Any     Allow  Local
Wireless Display Infrastructure Back Channel (TCP-In)             Any     Allow  Local
Core Networking - Neighbor Discovery Advertisement (ICMPv6-In)    Any     Allow  Local
Microsoft Media Foundation Network Source IN [TCP 554]             Any     Allow  Local
```

## 8. Inspect Wi-Fi Direct allow-rule filters

Command:

```powershell
Get-NetFirewallRule -PolicyStore ActiveStore -DisplayName "Wi-Fi Direct Spooler Use (In)",
    "Wi-Fi Direct Scan Service Use (In)" |
    Where-Object {
        $_.Enabled -eq 'True' -and
        $_.Direction -eq 'Inbound' -and
        $_.Action -eq 'Allow'
    } |
    ForEach-Object {
        $rule = $_
        $ports = $rule | Get-NetFirewallPortFilter
        $app = $rule | Get-NetFirewallApplicationFilter
        $addresses = $rule | Get-NetFirewallAddressFilter
        [pscustomobject]@{
            DisplayName   = $rule.DisplayName
            Profile       = $rule.Profile
            Program       = $app.Program
            Protocol      = $ports.Protocol
            LocalPort     = $ports.LocalPort -join ','
            RemotePort    = $ports.RemotePort -join ','
            RemoteAddress = $addresses.RemoteAddress -join ','
        }
    } |
    Format-List
```

Captured output:

```text
DisplayName:   Wi-Fi Direct Spooler Use (In)
Profile:       Public
Program:       C:\WINDOWS\system32\spoolsv.exe
Protocol:      Any
LocalPort:     Any
RemotePort:    Any
RemoteAddress: Any

DisplayName:   Wi-Fi Direct Scan Service Use (In)
Profile:       Public
Program:       C:\WINDOWS\system32\svchost.exe
Protocol:      Any
LocalPort:     Any
RemotePort:    Any
RemoteAddress: Any
```

## 9. Inspect the services tied to those rules

Command:

```powershell
Get-NetFirewallRule -PolicyStore ActiveStore -DisplayName "Wi-Fi Direct Spooler Use (In)",
    "Wi-Fi Direct Scan Service Use (In)" |
    ForEach-Object {
        $rule = $_
        $service = $rule | Get-NetFirewallServiceFilter
        [pscustomobject]@{
            DisplayName = $rule.DisplayName
            Program     = ($rule | Get-NetFirewallApplicationFilter).Program
            Service     = $service.Service
        }
    } |
    Format-List
```

Captured output:

```text
DisplayName: Wi-Fi Direct Spooler Use (In)
Program:     C:\WINDOWS\system32\spoolsv.exe
Service:     Spooler

DisplayName: Wi-Fi Direct Scan Service Use (In)
Program:     C:\WINDOWS\system32\svchost.exe
Service:     stisvc
```

## How I interpret this evidence

- The profiles were enabled; the displayed default action fields were `NotConfigured`, so I did not infer effective defaults from these fields alone.
- The rule counts describe configured rules, not network connections.
- The VLC block rules apply to the VLC executable for inbound TCP and UDP on the Public profile, with the port fields set to Any.
- The two Public-profile Wi-Fi Direct allow rules have Any values in the displayed protocol, port, and remote-address filters, but are associated with specific Windows programs and services.
- The `codex_sandbox_offline_block_inbound` rule was visible in ActiveStore. I did not independently verify its origin.
- These commands do not show whether a service was running, whether an application was listening, whether a matching connection occurred, or every possible rule condition. I made no firewall changes.
