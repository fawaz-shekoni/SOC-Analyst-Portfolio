# Windows Firewall

## What it does

Windows Firewall filters network traffic entering and leaving a device. Windows uses different profiles for Domain, Private, and Public networks; the active profile should match the network context.

## VM findings

The Windows Security page reported **Firewall is on** for all three profiles:

| Profile | Status |
|---|---|
| Domain network | On |
| Private network | On |
| Public network | On — active |

I reviewed the statuses and did not change rules or turn the firewall off.

## How I would check it

Open **Windows Security → Firewall & network protection**. Confirm that the firewall is on for each profile and note which profile is active. If a profile is off, find out why before changing anything; do not disable the firewall just to make an application work.

Microsoft: [Windows Firewall overview](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/)
