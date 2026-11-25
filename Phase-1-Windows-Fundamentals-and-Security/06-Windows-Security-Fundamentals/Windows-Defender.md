# Windows Defender

## What it does

Microsoft Defender Antivirus helps detect and block malware and other threats. Its status, recent scans, threat history, and protection updates are shown in **Windows Security → Virus & threat protection**. Another antivirus provider can change whether Defender is active.

## VM findings

- The page first showed **No current threats**, a quick scan with **0 threats found**, and **25,737 files scanned**. The scan shown was dated November 9, 2026.
- The protection settings section said **No action needed**.
- Security definitions were reported up to date; the page showed a last update of November 9, 2026 at 3:41 AM.
- The page briefly reported that the threat service had stopped. I restarted it. The later screenshot returned to the normal status page with no current threats and no action needed.
- Ransomware protection offered OneDrive setup for file recovery. I did not configure it.

These are observations from the VM screen, not a guarantee that the device is free from malware. A scan only describes what the tool found at that time.

## How I would check it

Open **Windows Security → Virus & threat protection**. Check the current protection status, recent scan result and time, protection definition update time, and threat history. If the page says protection is stopped or needs attention, investigate the cause before relying on the device.

Microsoft: [Microsoft Defender Antivirus](https://learn.microsoft.com/en-us/windows/security/operating-system-security/virus-and-threat-protection/microsoft-defender-antivirus)
