# Windows Services Investigation

**System examined:** Personal Windows PC  
**Scope:** Read-only inspection. No services were started, stopped, or reconfigured.

## Five running services observed

| Service name | Display name | What it does |
|---|---|---|
| Appinfo | Application Information | Helps Windows run applications that require additional permission, including handling elevation prompts. |
| AudioEndpointBuilder | Windows Audio Endpoint Builder | Manages Windows audio endpoints and helps audio devices work with applications. |
| Audiosrv | Windows Audio | Manages audio for Windows applications. |
| BDESVC | BitLocker Drive Encryption Service | Provides BitLocker drive-encryption support. |
| BFE | Base Filtering Engine | Supports Windows filtering and firewall features. |

These were the first five services returned by the running-services query at the time of collection. The exact list can change as Windows starts or stops services.

## One stopped service investigated

| Service name | Display name | State | Start mode | Description observed |
|---|---|---|---|---|
| AppIDSvc | Application Identity | Stopped | Manual | “Determines and verifies the identity of an application. Disabling this service will prevent AppLocker from being enforced.” |

The service was stopped and configured for Manual start in the observed output. This does not by itself indicate a problem: a manually started service may remain stopped until Windows or an administrator needs it. No configuration changes were made.

## Commands used

Show five running services:

```powershell
Get-Service |
  Where-Object Status -eq 'Running' |
  Select-Object -First 5 Status, Name, DisplayName
```

Show five stopped services:

```powershell
Get-Service |
  Where-Object Status -eq 'Stopped' |
  Select-Object -First 5 Status, Name, DisplayName
```

Inspect the Application Identity service:

```powershell
Get-CimInstance Win32_Service -Filter "Name='AppIDSvc'" |
  Select-Object Name, DisplayName, State, StartMode, Description
```

