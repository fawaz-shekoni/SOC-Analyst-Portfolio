# Evidence — 04 DNS Investigation

`command-results.md` transcribes the PowerShell results shown in my screenshots: MX, NS, DMARC TXT, the root TXT lookup through the configured resolver, the comparison lookup through `1.1.1.1`, and the SPF include chain.

The TXT lookup through `1.1.1.1` returned many additional public verification strings. I recorded only the SPF result needed for this investigation rather than copying unrelated values. The source screenshots were supplied in chat but were not available as local image files, so this evidence folder contains a transcript, not screenshot images.

