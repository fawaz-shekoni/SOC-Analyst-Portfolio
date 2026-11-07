# Windows Architecture

**Module:** 1 — Windows operating system fundamentals  
**System explored:** Windows 11

## What an operating system does

An operating system (OS) manages the computer's hardware and software and provides the interface people use to work with the computer. Windows provides a graphical user interface, and it also manages files, memory, processor time, applications, devices, and security.

## Hardware and software

- **Hardware** is the physical equipment, such as the processor, memory, storage drive, keyboard, and display.
- **Software** is Windows, applications, and the instructions that run on the computer.

## What happens when an application opens

Windows loads the program and the resources it needs into memory, creates one or more running processes, and schedules their work on the processor. Some applications use background services, but not every application starts a separate service when it opens.

## Processes and services

A **process** is a running instance of a program. A **service** is a background program that provides a function to Windows or other applications, often without a visible window. Services run as processes, but not every process is a service.

## Resource management

Windows shares processor time among running processes and gives them memory for their work. This allows multiple programs to run at once. Resource use changes over time.

The Task Manager screenshot showed **8% CPU** and **90% memory** at that moment. This is a snapshot, not a permanent reading.

## Account type and everyday security

Settings identified my account as an **Administrator**. Administrator accounts can make system-wide changes. A standard account has fewer permissions, which limits what malware or someone using that account can change without administrator approval. Using a standard account for routine work reduces exposure to administrator privileges.

## Windows folders explored

- **`C:\Program Files`** — Contains folders and files for installed applications. The screenshot showed folders including Bonjour, BraveSoftware, CapCut, Cisco Packet Tracer 9.0.0, Dell, Git, and Google.
- **`C:\Users`** — Contains user profile folders on this PC. The screenshot showed `Anon`, `MCB`, and `Public`. A profile usually contains a user's files and settings; this list may not represent every account that exists on the computer.
- **`C:\Windows`** — Contains Windows system files and folders used to run and maintain the operating system. The screenshot showed folders including `Logs`, `Microsoft`, `Microsoft.NET`, `Migration`, and `Minidump`.

The contents of the individual `C:\Users\Anon` profile were intentionally left unexplored.

## Evidence

- Task Manager process list: [task-manager-processes.png](evidence/task-manager-processes.png)
- Task Manager startup apps: [task-manager-startup-apps.png](evidence/task-manager-startup-apps.png)
- `C:\Users` folder: [users-folder.png](evidence/users-folder.png)
- `C:\Program Files` folder: [program-files-folder.png](evidence/program-files-folder.png)
- `C:\Windows` folder: [windows-folder.png](evidence/windows-folder.png)

## Reflection

I learned that Windows does more than provide a graphical interface: it manages hardware resources, files, applications, processes, and security. Applications can have multiple processes, and services run in the background. Windows also uses separate folders for installed applications, user profiles, and operating system files.

