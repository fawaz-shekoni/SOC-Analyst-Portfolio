# Windows Commands: System Inspection

**Module:** 2 — Windows commands and PowerShell  
**Environment:** Windows PowerShell 5.1

## Commands practised

| Command | What it shows | What I observed |
|---|---|---|
| `whoami` | The account identity used by the current PowerShell session. | The output used a `COMPUTER\USER`-style format. On this PC, the prefix matched the computer name and the final part identified the user account. |
| `hostname` | The computer's network name. | It returned the device name, matching the prefix shown in the `whoami` output. |
| `Get-Process` | Processes currently running, with details such as process name, ID, CPU time, and memory use. | Like Task Manager, it showed running processes. Some applications appeared in multiple rows because one app can use several processes. |
| `Get-Service` | Windows services and their current status. | The output included both `Running` and `Stopped` services. A stopped service can be normal if it is only needed on demand. |
| `Get-NetIPAddress` | IP address information for network interfaces, including interface name, address family, prefix length, and address state. | The output included Wi-Fi, loopback, virtual VMware adapters, Bluetooth, and other interfaces, with both IPv4 and IPv6 entries. It showed that this command reports addresses across interfaces, not only the currently used Wi-Fi connection. |

## What I learned

PowerShell can inspect account identity, the computer name, active processes, services, and network interface configuration directly from the command line. `Get-Process` complements Task Manager, while `Get-Service` and `Get-NetIPAddress` provide structured details that can help an analyst understand the host.

## Privacy note

The original console output contained the computer/account name and local network addresses. Those exact values are omitted from this portfolio note.

