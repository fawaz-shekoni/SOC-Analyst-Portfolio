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
| 5 | Identify the five processes using the most memory. | Not yet completed | Sort process objects by `WorkingSet64`, then select the first five. |
| 6 | List running Windows services. | Partially completed | `Get-Service` listed all services and their states. The output has not yet been filtered to running services only. |
| 7 | Identify listening TCP connections. | Not yet completed | Use `Get-NetTCPConnection -State Listen`. |
| 8 | Find local user accounts. | Not yet completed | Use `Get-LocalUser`. |

## Commands for the remaining checks

Use these in PowerShell when ready:

```powershell
Get-Process | Sort-Object WorkingSet64 -Descending | Select-Object -First 5 ProcessName, Id, WorkingSet64
Get-Service | Where-Object Status -eq 'Running'
Get-NetTCPConnection -State Listen
Get-LocalUser
```

These commands only retrieve information. Review the output before sharing screenshots, and cover account names, device identifiers, IP addresses, and other private details.

## Evidence

- Identity and process list: [PowerShell identity and processes](evidence/powershell-identity-and-processes-redacted.png)
- Services: [PowerShell services](evidence/powershell-services-redacted.png)
- Network adapters: [PowerShell network addresses](evidence/powershell-network-addresses-redacted.png)
- System information: [PowerShell computer information](evidence/powershell-computer-info-redacted.png)

