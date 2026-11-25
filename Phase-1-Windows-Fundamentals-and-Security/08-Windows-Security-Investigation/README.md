# Module 8 — Windows Security Investigation

## Project overview

For this investigation, I used a public sample dataset of Windows Security log events to examine failed logons (Event ID 4625) and check for related successful logons (Event ID 4624).

## Why I used CSV sample data

I first checked the Security log available to me, but it did not contain the failed-logon events needed for this exercise. I then opened a small public EVTX sample in Event Viewer. It contained only one 4625 event, so it was not enough to investigate a pattern of repeated failures.

I chose the CSV sample because it contains five 4625 records and related 4624 records for the same account. That gives me enough rows to compare the account, timestamps, logon type, source address, failure codes, and nearby successful logons. CSV also lets me inspect and sort the records clearly without claiming they came from my own computer. The data is labeled throughout this project as a public sample, not my personal Windows log.

The sample is a parsed/exported dataset rather than a raw EVTX file. It does not include every original event field, such as record IDs or all logon-process details. I treat the CSV rows as the evidence available for this exercise and note those limitations rather than filling gaps with assumptions.

## Files to include in the portfolio

- [Investigation report](Investigation-Report.md) — scope, method, evidence, findings, and next steps.
- [Analysis walkthrough](Analysis-Walkthrough.md) — how I reviewed and interpreted the sample data.
- [Sample data source and integrity notes](Sample-Data-Source.md) — source links, file hashes, and dataset limitations.
- [Selected event extract](evidence/selected-events.csv) — the relevant rows, with the account name replaced by `SampleUser`.
- [Evidence guide](evidence/README.md) — what I reviewed and the screenshots I plan to add.

## What I will capture as practical evidence

After I personally repeat the checks in the walkthrough, I will add two screenshots to `evidence/`: one showing the five 4625 rows in the failed-logon CSV, and one showing the same account's 4624 rows around 14:39:50 UTC. I will crop or redact unrelated personal details before publishing. The existing EVTX screenshot is not used as proof of repeated failures because that file had only one 4625 event.

## Privacy and handling

The sample includes a named account. I have not included copies of the CSV files in this portfolio folder. The report uses the name because it is part of the evidence being analyzed; if I publish this project publicly, I can replace it with a consistent label such as `SampleUser` while preserving the analysis.
