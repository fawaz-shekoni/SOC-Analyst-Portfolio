# Process Investigation

**System examined:** Personal Windows PC  
**Sample size:** Five processes  
**Note:** The module worksheet suggests ten processes. This record intentionally documents five, as agreed. PIDs are observations from the time of collection and can change when processes restart.

## Process observations

| Process | PID observed | Executable path | Purpose / observation |
|---|---:|---|---|
| AppActions.exe | 24448 | `C:\Windows\SystemApps\MicrosoftWindows.Client.CBS_cw5n1h2txyewy\AppActions.exe` | Windows component in the Client CBS system app folder. Its exact role was not confirmed during this exercise. |
| ApplePhotoStreams.exe | 22692 | `C:\Program Files\WindowsApps\AppleInc.iCloud_15.10.39.0_x64__nzyj5cx40ttqa\iCloud\ApplePhotoStreams.exe` | Apple iCloud component associated with Photo Streams. Its location is within the installed iCloud package. |
| ApplicationFrameHost.exe | 26224 | `C:\Windows\System32\ApplicationFrameHost.exe` | Windows process that hosts frames for certain Windows applications. |
| APSDaemon.exe | 4200 | `C:\Program Files\WindowsApps\AppleInc.iCloud_15.10.39.0_x64__nzyj5cx40ttqa\iCloud\APSDaemon.exe` | Apple component located in the installed iCloud package. |
| audiodg.exe | 10776 | `C:\Windows\System32\audiodg.exe` | Windows Audio Device Graph Isolation process, used in Windows audio processing. |

These names and paths were observed with PowerShell and Task Manager. The Windows system files were under `C:\Windows\System32`; the Apple components were under the iCloud package directory.

## Parent process example

For AppActions.exe, PowerShell reported:

| Child process | PID | Parent process | Parent PID |
|---|---:|---|---:|
| AppActions.exe | 24448 | svchost.exe | 1296 |

A parent process is the process that started another process. A `svchost.exe` parent is common for Windows components that are started through Windows services. This observation alone is not a security verdict.

## Commands used

List processes:

```powershell
Get-Process | Select-Object -First 10
```

Inspect executable paths for selected processes:

```powershell
Get-Process | Sort-Object ProcessName | Select-Object -First 10 ProcessName, Id, Path
```

Check a selected process's parent PID:

```powershell
Get-CimInstance Win32_Process -Filter "ProcessId=24448" |
  Select-Object Name, ProcessId, ParentProcessId
```

Look up the parent process name:

```powershell
Get-Process -Id 1296 | Select-Object ProcessName, Id
```

## What could make a process suspicious?

A process deserves further investigation if it has an unexpected name or publisher, runs from an unusual folder (for example, a temporary or user-writable folder when that is unexpected), behaves differently from its normal role, or makes unexplained network connections or resource demands. One indicator by itself does not prove malware. Verify the executable path, publisher/signature, and behavior before drawing a conclusion. Do not terminate unfamiliar system processes just to experiment.
