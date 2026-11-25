# Scenario 2: An application keeps crashing

## Problem

Practice scenario: examine repeated application crashes in the Application log. The records below are historical and do not show whether the application is still crashing.

## Investigation steps

1. Opened Event Viewer → Windows Logs → Application.
2. Filtered for Error events with Event ID 1000.
3. Compared timestamps and application names in the filtered records.
4. Opened a MiracastPowerManager.exe event and recorded the faulting module and exception code.

## Evidence

- [Event Viewer crash records](evidence/application-crash.png): three MiracastPowerManager.exe Event ID 1000 errors are visible on 5 September 2026 at 10:38:05, 10:38:14, and 10:41:04.
- The selected record lists ntdll.dll as the faulting module and exception code 0xc0000374.
- The filtered view contained 124 Event ID 1000 errors across the log. This is the total for the filter, not a count of MiracastPowerManager crashes.

## Findings

The screenshot supports a cluster of three MiracastPowerManager crashes within about three minutes. Microsoft defines 0xc0000374 as STATUS_HEAP_CORRUPTION. A faulting-module field naming ntdll.dll does not by itself prove that Windows or that DLL caused the corruption; the fault may have originated elsewhere. See Microsoft's [application/service crash guidance](https://learn.microsoft.com/en-us/troubleshoot/windows-server/performance/troubleshoot-application-service-crashing-behavior) and [NTSTATUS reference](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/596a1078-e883-4972-9bbc-49e60bebca55).

## Recommended resolution

If the same crash recurs, record its time and application version, check Reliability Monitor and the corresponding Event Viewer details, and identify the Miracast/wireless display software and related display or network drivers. Then check the PC or software vendor's supported updates and retest. Avoid concluding that ntdll.dll is the root cause from this event alone. No software or driver was changed for this exercise.
