# PowerShell Commands

**Module:** 2 — CMD and PowerShell  
**Environment:** Windows PowerShell 5.1

## Commands practised

| Command | What it shows | Observation / status |
|---|---|---|
| `whoami` | The current account identity. | Observed a `COMPUTER\USER`-style result. The exact value is redacted in the shared evidence. |
| `hostname` | The computer's network name. | Returned the device name. The exact value is redacted in the shared evidence. |
| `Get-Process` | Running processes and fields such as process name, process ID, CPU time, and memory. | Similar to Task Manager. One application can appear on multiple rows because it can run several processes. |
| `Get-Service` | Windows services and their current states. | The output included both `Running` and `Stopped` services; it did not filter to running services only. |
| `Get-NetIPAddress` | IP address details for network interfaces. | The output included Wi-Fi, loopback, virtual VMware adapters, Bluetooth, and other interfaces, with IPv4 and IPv6 records. Exact addresses are redacted. |
| `Get-ComputerInfo` | Computer, operating system, and firmware information. | The output contains many system details. The registered owner and Windows Product ID are redacted in the shared screenshot. |
| `Get-LocalUser` | Local user accounts. | Not yet practised. |
| `Get-NetTCPConnection` | TCP connections and their states. | Not yet practised. |
| `Get-WinEvent` | Windows event log records. | Not yet practised. |
| `Get-EventLog` | Entries from supported event logs. | Not yet practised. |

## Evidence

The screenshots below show actual PowerShell output. Identifying values have been covered in the shared copies.

### Identity and process listing

![Redacted whoami, hostname, and Get-Process output](evidence/powershell-identity-and-processes-redacted.png)

### Service list

![Redacted Get-Service output](evidence/powershell-services-redacted.png)

### Network interfaces

![Redacted Get-NetIPAddress output](evidence/powershell-network-addresses-redacted.png)

### Computer information

![Redacted Get-ComputerInfo output](evidence/powershell-computer-info-redacted.png)

