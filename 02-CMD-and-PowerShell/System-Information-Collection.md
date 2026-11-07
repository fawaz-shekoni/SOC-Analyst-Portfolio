# System Information Collection

**Module:** 2 — CMD and PowerShell  
**Purpose:** Record the practical results without publishing personal device identifiers or network addresses.

## Assignment progress

| # | Task | Status | Notes |
|---:|---|---|---|
| 1 | Retrieve the computer hostname. | Completed | Used `hostname`. The value is omitted from this public write-up. |
| 2 | Identify the current username. | Completed | Used `whoami`. The value is omitted from this public write-up. |
| 3 | Retrieve the IP address. | Completed | Used `Get-NetIPAddress`. The output contained multiple interfaces and address families; exact addresses are omitted. |
| 4 | List running processes. | Completed | Used `Get-Process`; the output can be compared with Task Manager. |
| 5 | Identify the five processes using the most memory. | Completed | Sorted by `WorkingSet64`; values were converted from bytes to MiB. The result is recorded in `PowerShell-Commands.md`. |
| 6 | List running Windows services. | Completed | Filtered `Get-Service` to `Status -eq 'Running'`. The screenshot captures the beginning of the results. |
| 7 | Identify listening TCP connections. | Completed | `Get-NetTCPConnection -State Listen` returned four rows using `127.0.0.1` for local and remote addresses, all in the `Listen` state. Port values are omitted from public evidence. |
| 8 | Find local user accounts. | Not yet completed | Use `Get-LocalUser`. |

## Remaining assignment

Use this in PowerShell when ready:

```powershell
Get-LocalUser
```

This command only retrieves information. Review the output before sharing screenshots, and cover account names, device identifiers, IP addresses, and other private details.

## Evidence

- Identity and process list: [PowerShell identity and processes](evidence/powershell-identity-and-processes-redacted.png)
- Top five processes by memory: [PowerShell memory ranking](evidence/powershell-top-memory-processes-redacted.png)
- Running services: [PowerShell running services](evidence/powershell-running-services-redacted.png)
- Full service list: [PowerShell services](evidence/powershell-services-redacted.png)
- Network adapters: [PowerShell network addresses](evidence/powershell-network-addresses-redacted.png)
- System information: [PowerShell computer information](evidence/powershell-computer-info-redacted.png)
- Listening TCP connections: [PowerShell TCP listeners](evidence/powershell-tcp-listeners-simple-redacted.jpg)
