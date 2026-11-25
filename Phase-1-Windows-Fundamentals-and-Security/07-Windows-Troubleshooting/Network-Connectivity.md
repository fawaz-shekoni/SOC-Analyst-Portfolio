# Scenario 3: Network connectivity problem

## Problem

Practice scenario: check whether basic web connectivity is available. This was a connectivity check on the main PC, not a speed or reliability test.

## Investigation steps

1. Reviewed Windows quick settings for Wi-Fi and Airplane mode.
2. Opened a Microsoft webpage in the browser.
3. Compared the result with Task Manager's network activity snapshot, noting that it was idle at capture.

## Evidence

- [Wi-Fi status (network name removed)](evidence/wifi-status-redacted.png): the Wi-Fi quick setting was active and Airplane mode was off.
- [Microsoft webpage loaded](evidence/connectivity-page.png): the Microsoft home page content rendered in the browser.
- The Task Manager snapshot showed 0 Kbps at the instant it was captured, which is normal while the connection is idle and does not indicate a fault by itself.

## Findings

The evidence shows that Wi-Fi was enabled and one Microsoft webpage loaded at the time of the check, so basic connectivity was working then. It does not assess slow speeds, intermittent drops, latency, packet loss, DNS problems affecting other sites, or connectivity at other times. The earlier report of a slow internet experience during a VMware update is not treated as a test of the main PC.

## Recommended resolution

If the problem happens again, record the time, affected apps/sites, and whether other devices on the same network are affected. Check signal strength and router status, then compare a known website with a basic latency test. If only one app or site fails, investigate that service separately. Avoid changing network settings until the failure is reproducible and the evidence points to a specific cause. No network settings were changed for this exercise.
