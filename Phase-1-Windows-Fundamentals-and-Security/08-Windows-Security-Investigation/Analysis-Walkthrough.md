# My Analysis Walkthrough

This is the procedure I used to study the sample and the checks I would repeat before presenting the result. It explains how I reached the finding so a reviewer can follow my reasoning.

## 1. Choose a dataset that can answer the question

My own Security log did not show the failed-logon records needed for this exercise. The first EVTX sample I opened showed four total events but only one 4625 failure. That was too small to support an investigation of repeated attempts. I selected the public CSV exports because the failed file contains five 4625 records and the success file contains 4624 records for the same account. I did not treat either sample as a log from my PC.

## 2. Inspect the failed-logon rows

I open `failed_logons.csv` in Excel or another spreadsheet program and check the column headings before interpreting values. I filter `event_id` to `4625`, then compare `target_user`, `timestamp`, `ip`, `logon_type`, `status`, and `sub_status`.

Questions I ask while reviewing the rows:

- Do the failures involve one account or many accounts?
- Are the timestamps close together or spread out?
- Is the source address local, private, or public?
- What kind of logon does the logon type represent?
- Do the status and substatus codes point to a wrong password or another failure reason?

In this sample, five rows target the same account, occur within about seven seconds, use `127.0.0.1`, and have logon type 2. The status pair is `0xc000006d` / `0xc000006a`, which Microsoft's status reference identifies as logon failure / wrong password.

## 3. Correlate successful logons

I open `success_logons.csv`, filter `event_id` to `4624`, then filter `target_user` to the same account and inspect the time window around the failures. I compare the source address and logon type as well as the timestamp.

In the provided rows, two matching 4624 records occur at 14:39:50.501 UTC, about 2.9 seconds after the final failed attempt. There are also earlier matching success rows. This timing could fit a person correcting a mistyped password, but it cannot prove who signed in or whether the activity was authorized.

## 4. Make a measured assessment

Five quick failures are a pattern I would investigate. The subsequent success increases the need to verify the activity, while the loopback address and interactive logon type do not point to a remote source in these records. I would not label the event a confirmed attack based on these CSV fields alone. I would verify the original event records and ask whether the account owner recognizes the sign-in.

## 5. Repeat the review with PowerShell (optional)

From the folder containing the downloaded CSV files, I can use these read-only commands to count and display the relevant rows:

```powershell
$failed = Import-Csv .\failed_logons.csv
$failed | Where-Object event_id -eq '4625' | Sort-Object timestamp | Format-Table timestamp,target_user,ip,logon_type,status,sub_status

$success = Import-Csv .\success_logons.csv
$success | Where-Object { $_.event_id -eq '4624' -and $_.target_user -eq 'Daniele Monaco' -and $_.timestamp -like '2025-11-01*' } | Sort-Object timestamp | Format-Table timestamp,target_user,ip,logon_type
```

The source account name in the command is used only to reproduce the source CSV filter locally. I replace it with `SampleUser` in the public report and selected extract. These commands read data only; they do not change the computer or network.

## 6. Capture my practical evidence

Before I publish screenshots, I will:

1. Open the source CSVs from the linked repository in Excel or a text editor.
2. Capture the header and five failed rows, with the five timestamps and event IDs visible.
3. Capture the filtered successful rows around 14:39:50 UTC, including the event ID, timestamp, source, and logon type.
4. Redact the sample account name as `SampleUser` and remove unrelated personal information.
5. Save the images as `failed-logons.png` and `related-success-logons.png` inside `evidence/`.

The report is based on the CSV records and is clearly labeled as a sample-data exercise. I should only describe the screenshots as evidence I personally reviewed after I have captured and checked them.

## References

- [Microsoft: Event 4625 — An account failed to log on](https://learn.microsoft.com/windows/security/threat-protection/auditing/event-4625)
- [Microsoft: NTSTATUS values](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/596a1078-e883-4972-9bbc-49e60bebca55)
- [Public sample dataset repository](https://github.com/Dany95047/basic-log-analysis)
