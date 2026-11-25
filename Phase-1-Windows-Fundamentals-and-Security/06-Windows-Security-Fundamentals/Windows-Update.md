# Windows Update

## Why it matters

Windows Update delivers operating-system and driver updates. Updates can fix security problems, so a list of pending or old updates is a useful finding even when the device currently reports no threats.

## VM findings

The Windows Update page showed:

- **Last checked:** “Yesterday, 11:40 PM” (as displayed by the VM).
- Two Broadcom system driver updates (versions 9.8.30.0 and 9.8.18.1): **Pending install**.
- A .NET cumulative update for Windows 10 Version 1809 (KB5001879): **Pending restart**.
- A Windows 10 Version 1809 cumulative update (KB5003171): **Installing — 44%** in the screenshot.
- A January 2021 Windows 10 Version 1809 update (KB4589208): **Pending restart**.

I did not start a new update check or install anything during the review. The screenshot captures an update already in progress. Its completion was not verified.

The page identifies Windows 10 Version 1809. The edition/channel is not visible, and support periods differ by edition. This is an old VM image; keep it for isolated lab practice and check the edition and support lifecycle before using it for real accounts or everyday browsing. Microsoft states that Windows 10 Home and Pro reached end of support on October 14, 2025; version 1809 also has a separate lifecycle for LTSC editions. See [Windows 10 support status](https://support.microsoft.com/en-us/windows/deployment/updates-lifecycle/windows-10-support-has-ended-on-october-14-2025) and [supported Windows client versions](https://learn.microsoft.com/en-us/windows/release-health/supported-versions-windows-client).

## How I would check it

Open **Settings → Update & Security → Windows Update**. Record the last check time, pending updates, current progress, and restart requirement. Do not interrupt an installation. A security review should distinguish updates that are installed from updates that are only pending.
