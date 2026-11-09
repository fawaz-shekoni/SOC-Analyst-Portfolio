# Module 3: Windows Users and Permissions

This module records user, group, and NTFS permission exercises performed in a Windows 10 virtual machine.

## Deliverables

- [User accounts](User-Accounts.md)
- [Groups and privileges](Groups-and-Privileges.md)
- [NTFS permissions](NTFS-Permissions.md)
- [Evidence screenshots](evidence/)

## Lab summary

A local standard account named **ANJI** was used as the test user. In `C:\Users\Public\ANJI-Permission-Lab`, ANJI could create a file before the permission change. An explicit **Deny: Write** entry was then added for ANJI on this lab folder. Afterward, ANJI could list the folder contents, while a PowerShell file-creation attempt on the exact lab path returned **Access is denied**. The VM administrator could create a file in the same folder.

The explicit Deny entry is documented as a controlled exercise. In normal permission design, prefer granting only the required access and removing unnecessary broad Allow permissions; use explicit Deny entries only when a specific exception is needed.

Windows 10 reached end of support on October 14, 2025. This installation was used as a virtual lab: [Microsoft support notice](https://support.microsoft.com/en-us/windows/deployment/updates-lifecycle/windows-10-support-has-ended-on-october-14-2025).


