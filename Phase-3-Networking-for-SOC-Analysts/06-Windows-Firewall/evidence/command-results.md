# Practical 1 — firewall profile evidence

I ran these read-only PowerShell commands on my main Windows PC.

## Command

```powershell
Get-NetFirewallProfile
```

I also used this shorter view to make the three profile settings easier to compare:

```powershell
Get-NetFirewallProfile | Select-Object Name, Enabled, DefaultInboundAction, DefaultOutboundAction
```

## Captured output

```text
Name     Enabled DefaultInboundAction DefaultOutboundAction
----     ------- -------------------- ---------------------
Domain   True    NotConfigured        NotConfigured
Private  True    NotConfigured        NotConfigured
Public   True    NotConfigured        NotConfigured
```

## What this shows

- Windows Firewall was enabled for the Domain, Private, and Public profiles when I checked.
- The displayed default action fields were `NotConfigured`. I do not treat that as meaning the firewall is disabled; this output alone does not tell me the effective default actions.
- This is a configuration snapshot. It does not show a connection attempt, a blocked event, or an attack.
- I did not change any firewall settings.

This transcript is typed from the PowerShell screenshots shared during the investigation. The screenshots themselves are not attached here.
