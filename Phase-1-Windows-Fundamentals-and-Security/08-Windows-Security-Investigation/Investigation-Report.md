# Windows Security Investigation — Failed Logons

## Scenario

I investigated a sample Windows Security log dataset to determine whether a cluster of failed logons might indicate unauthorized access. I used Event ID 4625 for failed logons and looked for nearby Event ID 4624 successful logons for the same account.

## Data and method

I used the public CSV exports `failed_logons.csv` and `success_logons.csv` from the source listed in [Sample Data Source](Sample-Data-Source.md). The timestamps in the files include the `+00:00` UTC offset. I filtered the success records to the same account and date as the failures, then compared time, account, source address, and logon type.

I chose CSV because my own Security log did not provide the events required for this project, and the small EVTX sample I tested contained only one failed logon. The CSV sample provided five failed records and related successful records. I am identifying it as sample data rather than presenting it as evidence from my own PC.

## Evidence

| UTC time, 2025-11-01 | Event ID | Account | Source address | Logon type | Result |
|---|---:|---|---|---:|---|
| 14:39:40.579376 | 4625 | SampleUser* | 127.0.0.1 | 2 | Status `0xc000006d`; substatus `0xc000006a` |
| 14:39:43.931002 | 4625 | SampleUser* | 127.0.0.1 | 2 | Status `0xc000006d`; substatus `0xc000006a` |
| 14:39:45.380770 | 4625 | SampleUser* | 127.0.0.1 | 2 | Status `0xc000006d`; substatus `0xc000006a` |
| 14:39:46.617188 | 4625 | SampleUser* | 127.0.0.1 | 2 | Status `0xc000006d`; substatus `0xc000006a` |
| 14:39:47.608610 | 4625 | SampleUser* | 127.0.0.1 | 2 | Status `0xc000006d`; substatus `0xc000006a` |
| 14:39:50.501045 | 4624 | SampleUser* | 127.0.0.1 | 2 | Successful logon |
| 14:39:50.501057 | 4624 | SampleUser* | 127.0.0.1 | 2 | Successful logon |

\* `SampleUser` is a privacy label substituted for the source dataset's account name. The underlying data values and timestamps were not altered.

The sample also contains four earlier 4624 records for this account on the same date, at 13:52:05.769497, 13:52:05.769514, 13:52:55.079567, and 13:52:55.079580 UTC. The two records at each timestamp pair are only microseconds apart. Since the CSV does not include Windows event record IDs, I cannot determine from this dataset alone whether paired records represent distinct events, duplicate extraction, or another logging detail; I preserve the rows as provided.

## Findings

I found five failed logon records for the same account within about seven seconds. Microsoft documents `0xc000006d` as a logon failure and `0xc000006a` as a wrong-password status. Logon type 2 means an interactive sign-in. The source value `127.0.0.1` is the loopback address, so these sample records do not identify a remote source host. I used Microsoft's [Event 4625 reference](https://learn.microsoft.com/windows/security/threat-protection/auditing/event-4625) and [NTSTATUS reference](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/596a1078-e883-4972-9bbc-49e60bebca55) to interpret these fields.

Two successful 4624 rows for the same account, loopback address, and logon type appear about 2.9 seconds after the final failure. This sequence could be consistent with a user entering the wrong password several times and then signing in successfully. It could also warrant further investigation if the account owner did not initiate the sign-in. The CSV alone cannot establish who was at the device or their intent.

I therefore assess this as a repeated failed-password pattern followed by a local successful sign-in. It is worth checking with the account owner and validating against the original Security log, but this dataset does not support a conclusion that a remote attacker was responsible.

## Recommended next steps

If I encountered this pattern on a real device, I would:

1. Confirm with the account owner whether they were signing in at that time and whether they had mistyped or recently changed their password.
2. Review the original 4625 and 4624 event details, including record IDs, logon process, authentication package, workstation, process, and any available source information.
3. Check surrounding Security events for other accounts, additional failure bursts, account lockouts, privilege changes, or unusual activity after the successful logon.
4. Compare the activity with other endpoint or identity-provider telemetry before deciding whether to escalate.
5. If the owner does not recognize the activity or corroborating evidence appears, follow the organization’s incident process: secure the account, preserve logs, and investigate the device and related sign-ins.

I would not block `127.0.0.1` or treat these sample rows as proof of a remote attack. I would make containment decisions only after validating the original event context and checking whether the activity was authorized.

## Limitations

- This is a public sample dataset, not a capture from my own computer.
- The files are parsed CSVs, not raw EVTX, and omit fields needed for full event correlation.
- The account name and timestamps are reproduced from the sample; they are not claims about my own account or activity.
- The source repository’s summary says there was no successful logon after failures within its threshold, while the supplied success CSV has two same-account 4624 rows about 2.9 seconds after the last failure. I report the rows I inspected and flag this discrepancy rather than adopting the summary’s conclusion without checking.
- The sample is useful for practicing analysis, but an operational investigation should use preserved original logs and corroborating evidence.
