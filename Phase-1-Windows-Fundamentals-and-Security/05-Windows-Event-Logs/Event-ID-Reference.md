# Windows Security Event ID Reference

| Event ID | Meaning |
|---:|---|
| 4624 | An account successfully logged on. |
| 4625 | An account failed to log on. |
| 4634 | An account logged off. |
| 4648 | A logon was attempted using explicit credentials. |
| 4688 | A new process was created. |
| 4720 | A user account was created. |
| 4722 | A user account was enabled. |
| 4740 | A user account was locked out. |

## Fields to examine

- **Event ID and Keywords:** identify the event and whether the audit result was success or failure.
- **Logged / Date and Time:** when Windows recorded the event.
- **Account Name:** distinguish the account reporting the event from the account involved in the logon; for 4624, inspect **New Logon**.
- **Logon Type:** describes the kind of logon. Type 2 is interactive at the computer; type 5 is a service logon.
- **Failure Information:** on 4625, record the reported reason and any status details.
- **Process and Network Information:** may help explain which process requested the logon and whether a remote source was involved.

Windows may not record every event ID unless the relevant audit policy is enabled. An event missing from a log does not by itself prove that something is wrong.

## References

- [Microsoft: Event 4624, successful logon](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4624)
- [Microsoft: Event 4625, failed logon](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4625)
- [Microsoft: Security auditing documentation](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/)
