# Scenario 1: Slow after sign-in

## Problem

Practice scenario: investigate a computer that feels slow after sign-in. No continuing slowdown was confirmed during this exercise, so this is an evidence-based diagnostic example rather than a claim that the PC is currently slow.

## Investigation steps

1. Opened Task Manager and reviewed the Processes snapshot.
2. Opened **Performance → Memory** to check total memory, in-use memory, and availability.
3. Reviewed the Startup apps list without enabling or disabling entries.
4. Checked the properties and digital-signature tab for the Google updater entry to help identify it.

## Evidence

- [Task Manager memory snapshot](evidence/memory-pressure.png): 8.0 GB installed; 6.8 GB of 7.7 GB usable memory in use (88%); 971 MB available. CPU was 9% and disk was 0% in this snapshot.
- [Processes snapshot](evidence/processes-snapshot.png): overall memory was 87%; CPU 15%, disk 5%, network 0% at capture. Several visible apps/process groups included ChatGPT, WebView2 Manager, Chrome, and Grammarly.
- [Startup apps, first section](evidence/startup-apps-page1.png) and [remaining section](evidence/startup-apps-page2.png): several apps were enabled, including Chrome, GeminiAppLauncher, Grammarly, iCloud, OneDrive, SecurityHealthSystray, and an entry named “updater.” Most displayed startup impact as “Not measured.”
- The “updater” Properties view described it as Google Updater (x64) in a GoogleUpdater folder; the signature tab listed Google LLC with SHA-256. These details are consistent with a Google updater, but the screenshot alone is not a full signature-validation report.

## Findings

The strongest measured pressure in the captured snapshot was memory use: 87–88% of usable RAM, with under 1 GB available. CPU and disk were low at that moment. This can contribute to sluggishness if sustained, but a single reading cannot establish the cause of a sign-in slowdown. The startup list does not show that any enabled app caused the memory pressure; its impact was not measured.

## PowerShell checks to run

Run these in PowerShell immediately after sign-in, then run them again after startup has settled. Compare the process names and values between the two snapshots.

```powershell
Get-Process | Sort-Object WorkingSet64 -Descending | Select-Object -First 10 ProcessName, WorkingSet64

Get-Process | Sort-Object CPU -Descending | Select-Object -First 5 ProcessName, CPU
```

WorkingSet64 is the process's current memory working set, reported in bytes. CPU is cumulative processor time in seconds since that process started; it is not a live CPU percentage. Use Task Manager's CPU percentage or repeat process snapshots over a short interval when checking current CPU load. A process using a lot of memory or CPU is a lead to investigate, not proof that it caused the slowdown.

## Recommended resolution

If the slowdown is repeatable, compare Task Manager immediately after sign-in and again once startup settles. Sort Processes by Memory and CPU, note which entries remain high, and compare Startup apps with the user’s actual needs. Disable an optional startup item only after confirming it is not needed. If memory remains near capacity during normal use, review workload and hardware capacity. No startup setting was changed for this exercise.
