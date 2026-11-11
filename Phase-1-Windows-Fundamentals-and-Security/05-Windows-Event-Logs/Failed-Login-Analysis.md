# Failed Login Analysis

## Observation

| Field | Value observed |
|---|---|
| Log | Security |
| Event ID | 4625 |
| Result | Audit Failure |
| Date and time | November 9, 2026, 09:28:46 (local time shown in Event Viewer) |
| Account for which logon failed | MCB |
| Logon type | 2 — interactive logon at the computer |
| Failure reason shown | Unknown user name or bad password |
| Filtered result count | Three 4625 events were listed; I examined the selected event |

## Interpretation

The selected event records an unsuccessful interactive sign-in attempt for MCB. I was trying to sign in to MCB around this time. A successful 4624 event for MCB appears at 09:36:06 on the same date, seven minutes and twenty seconds later. This sequence is consistent with my sign-in attempts, but the event records alone do not prove who entered the credentials.

I have not examined the other two 4625 events, so I am not assuming they involve MCB or have the same cause.

## What an analyst would investigate further

- Confirm whether the attempt was expected with the account owner.
- Compare the account, time, and logon type in surrounding 4624 and 4625 events.
- Review the 4625 failure reason, status/substatus, caller process, and source network address when present.
- Check whether repeated failures or unfamiliar remote sources appear.
- Avoid treating one failed logon as proof of an intrusion.

See the filtered event screenshots in [evidence](evidence/README.md).
