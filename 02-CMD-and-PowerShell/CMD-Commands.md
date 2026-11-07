# CMD Commands

**Module:** 2 — CMD and PowerShell  
**Status:** Practised `hostname`, `whoami`, and `ipconfig`. Other listed CMD commands remain to be practised.

| Command | Purpose | Practice status |
|---|---|---|
| `ipconfig` | Displays network adapter configuration, including IP settings. | Practised. The visible output included two disconnected wireless virtual adapters and a VMware VMnet1 adapter. The screenshot cuts off before the full output; exact IP addresses are redacted. |
| `systeminfo` | Displays operating system and hardware information. | To practise |
| `whoami` | Displays the current account identity. | Practised. It returned a computer-and-account identity; both values are redacted. |
| `tasklist` | Lists running processes. | To practise |
| `net user` | Lists local user accounts. | To practise |
| `hostname` | Displays the computer name. | Practised. The computer name is redacted. |
| `netstat -ano` | Displays network connections/listeners and associated process IDs. | To practise |
| `dir` | Lists files and folders in the current directory. | To practise |

## Notes

`ipconfig` can list several adapters, including virtual adapters. An address shown under a VMware adapter belongs to that virtual network interface; it should not automatically be treated as the laptop's Wi-Fi address or public internet address. This screenshot shows only the beginning of the complete `ipconfig` output.

## Evidence

![Redacted Command Prompt output for hostname, whoami, and ipconfig](evidence/cmd-hostname-whoami-ipconfig-redacted.jpg)

The account name, computer name, and visible IP addresses have been redacted.
