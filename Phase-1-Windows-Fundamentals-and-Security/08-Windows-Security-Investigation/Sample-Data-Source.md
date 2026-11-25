# Sample Data Source and Integrity Notes

## Source

I used two CSV files from the public GitHub repository [Dany95047/basic-log-analysis](https://github.com/Dany95047/basic-log-analysis):

- [`data/failed_logons.csv`](https://github.com/Dany95047/basic-log-analysis/blob/main/data/failed_logons.csv)
- [`data/success_logons.csv`](https://github.com/Dany95047/basic-log-analysis/blob/main/data/success_logons.csv)

The repository describes its data as extracted from Windows Security event logs and processed into CSV files. I chose it because the files contain multiple Event ID 4625 records and related 4624 records needed for the exercise. The small EVTX sample I had opened contained only one failed event, and my own available Security log did not provide enough matching events.

## Downloaded file hashes

I recorded SHA-256 hashes for the copies used during this analysis so the exact files can be checked:

| File | SHA-256 |
|---|---|
| `failed_logons.csv` | `9F12351822CD5D5D087CDB03AE878C52738A214E347D7BF4E3C0989495E94FAA` |
| `success_logons.csv` | `40DABED6B9EEA9B351C9D4DF63B2A455997BE16F54E70E58496A173D2130F8FB` |

## Limits and source discrepancy

These are parsed CSV exports rather than the original EVTX. They do not include all Windows event fields, including record IDs, so I cannot independently establish whether rows separated by microseconds are separate underlying events or duplicate extraction.

The repository’s written summary says it found no successful logon after the failures within its threshold. However, the supplied `success_logons.csv` includes two Event ID 4624 rows for the same account, source address, and logon type about 2.9 seconds after the last 4625 row. I used the rows in the downloaded files for the report and disclosed this mismatch. I did not silently resolve it or treat the repository summary as stronger evidence than the records.

I have not copied the source CSVs into this portfolio folder. The source links and hashes are provided so the project remains traceable without republishing the dataset.
