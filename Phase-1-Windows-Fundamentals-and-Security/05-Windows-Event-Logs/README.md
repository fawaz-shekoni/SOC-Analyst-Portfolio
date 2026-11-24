# Module 5: Windows Event Viewer

This folder records a read-only review of the Security log on my personal Windows PC.

## Files

- [Event ID Reference](Event-ID-Reference.md)
- [Successful Login Analysis](Successful-Login-Analysis.md)
- [Failed Login Analysis](Failed-Login-Analysis.md)
- [Filtered event screenshots](evidence/README.md)

## Summary

I examined a failed interactive logon for the host PC account MCB and a successful interactive logon for that same account about seven minutes later. I reported that I was attempting to sign in to MCB around that time. This is consistent with my activity, but the event records alone cannot identify the person at the keyboard.

The screenshots also include a separate SYSTEM service logon (Logon Type 5), which is background activity and not an MCB sign-in. My VMware practice account is separate and does not appear in these host PC screenshots. No logs or audit settings were changed.
