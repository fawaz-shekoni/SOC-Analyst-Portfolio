# Module 6: Windows security fundamentals

This module records a read-only review of Windows Security and Windows Update inside my VMware practice machine. The screenshots show the state at the time of review; they are not a guarantee of ongoing protection.

## Notes

- [Windows Defender](Windows-Defender.md)
- [Windows Firewall](Windows-Firewall.md)
- [Windows Update](Windows-Update.md)
- [UAC and Access Control](UAC-and-Access-Control.md)
- [Screenshots and captions](evidence/README.md)

## Findings summary

At the time reviewed, Windows Security reported no current threats, and its Virus & threat protection settings said no action was needed. The Defender page had briefly reported a stopped threat service; I restarted it and the later page showed no current threats. The firewall was on for Domain, Private, and the active Public profile. SmartScreen choices shown were set to Warn. Core isolation reported that virtualization-based security was running, while standard hardware security was not supported in this VM. The device health report showed no issues in storage capacity, apps and software, or Windows Time.

Windows Update showed several pending updates and one update installing at 44%; completion was not verified. The VM reports Windows 10 Version 1809, so keep it isolated for lab work until its edition and support status are checked.

No security settings were changed during the review. I cropped the UAC screenshot to remove the VM name visible in the background.
