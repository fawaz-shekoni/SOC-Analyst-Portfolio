# Module 2: CMD and PowerShell

## What you need to learn

Understand the difference:

- **CMD:** The traditional Windows command-line interpreter.
- **PowerShell:** A more powerful command-line shell and scripting environment. It can also work with structured objects returned by commands.

You do not need to become a programmer. The goal is to become comfortable using commands to investigate systems.

## Where to learn

- [Microsoft Learn: Get Started with PowerShell](https://learn.microsoft.com/en-us/training/paths/get-started-windows-powershell/)
- [Microsoft CMD command reference](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)

## Commands to practise

### CMD

| Command | Purpose |
|---|---|
| `ipconfig` | View network configuration. |
| `systeminfo` | View system information. |
| `whoami` | Identify the current user. |
| `tasklist` | View running processes. |
| `net user` | View local accounts. |
| `hostname` | Display the computer name. |
| `netstat -ano` | View network connections and process IDs. |
| `dir` | List files and folders. |

### PowerShell

| Command | Purpose |
|---|---|
| `Get-Process` | View processes. |
| `Get-Service` | View services and their status. |
| `Get-ComputerInfo` | View computer and operating system information. |
| `Get-LocalUser` | View local user accounts. |
| `Get-NetIPAddress` | View IP addresses assigned to network interfaces. |
| `Get-NetTCPConnection` | View TCP connections. |
| `Get-EventLog` | Query supported event logs. |
| `Get-WinEvent` | Query Windows event logs. |

## Practical assignments

1. Retrieve your computer's hostname.
2. Identify your current username.
3. Retrieve your IP address.
4. List all running processes.
5. Identify the five processes using the most memory.
6. List all running Windows services.
7. Identify listening TCP connections.
8. Find your local user accounts.

## How to prove you know it

Complete the eight tasks without copying commands from a tutorial. Be ready to explain what each command shows and what you learned from the result.

## GitHub deliverable

This module is organized as `02-CMD-and-PowerShell/` and includes:

- `CMD-Commands.md`
- `PowerShell-Commands.md`
- `System-Information-Collection.md`
- Redacted screenshots of actual command output in `evidence/`
- Short explanations of what each command reveals

## Progress so far

All eight system-information assignments and all eight CMD command practices are complete. `Get-EventLog` has been practised; `Get-WinEvent` remains to be tried.

Screenshots are reviewed individually; account and computer names are masked where appropriate. Other technical values remain visible when useful to the exercise.
