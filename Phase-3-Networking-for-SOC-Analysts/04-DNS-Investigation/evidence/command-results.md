# DNS command results transcribed from screenshots

These are observations from my PowerShell screenshots. DNS answers can change over time. The query to `1.1.1.1` was sent directly to that resolver for comparison; I did not change my device DNS settings.

## MX record

Command: `Resolve-DnsName microsoft.com -Type MX`

```text
microsoft.com  MX  TTL 3599  NameExchange microsoft-com.mail.protection.outlook.com  Preference 10
```

## NS records

Command: `Resolve-DnsName microsoft.com -Type NS`

```text
microsoft.com NS ns1-39.azure-dns.com   TTL 172799
microsoft.com NS ns2-39.azure-dns.net   TTL 172799
microsoft.com NS ns3-39.azure-dns.org   TTL 172799
microsoft.com NS ns4-39.azure-dns.info  TTL 172799
```

## DMARC TXT record

Command: `Resolve-DnsName _dmarc.microsoft.com -Type TXT`

```text
v=DMARC1; p=reject; pct=100; rua=mailto:itex-rua@microsoft.com; ruf=mailto:itex-ruf@microsoft.com; fo=1
```

## Root TXT query through configured resolver

Commands: `Resolve-DnsName microsoft.com -Type TXT` and `nslookup -type=TXT microsoft.com`

```text
Resolve-DnsName: Bad DNS packet (BAD_PACKET)
nslookup resolver: 192.168.0.1 (server name displayed as Unknown)
nslookup result: DNS request timed out after 2 seconds; no TXT answer was displayed
```

## Root TXT query through comparison resolver

Command: `nslookup -type=TXT microsoft.com 1.1.1.1`

```text
Resolver: one.one.one.one
Address: 1.1.1.1
Result: Multiple TXT records returned for microsoft.com
```

The screenshot showed several unrelated domain verification strings. They are not transcribed here because they were not needed for the SPF investigation.

## Root SPF record

Command: filtered `Resolve-DnsName microsoft.com -Type TXT -Server 1.1.1.1` output for strings beginning with `v=spf1`.

```text
v=spf1 include:_spf-a.microsoft.com include:_spf-b.microsoft.com include:_spf-ssg-a.msft.net include:_spf1-meo.microsoft.com -all
```

## First included SPF policy

Command: `Resolve-DnsName _spf-a.microsoft.com -Type TXT -Server 1.1.1.1 | Select-Object -ExpandProperty Strings`

```text
v=spf1 ip4:216.99.5.67 ip4:216.99.5.68 ip4:202.177.148.100 ip4:203.122.32.250 ip4:202.177.148.110 ip4:213.199.128.139 ip4:213.199.128.145 ip4:207.46.50.72 ip4:207.46.50.82 ip4:65.55.42.224/28 include:spf.protection.outlook.com ~all
```

## Nested Outlook SPF policy

Command: `Resolve-DnsName spf.protection.outlook.com -Type TXT -Server 1.1.1.1 | Select-Object -ExpandProperty Strings`

```text
v=spf1 ip4:40.92.0.0/15 ip4:40.107.0.0/16 ip4:52.100.0.0/15 ip4:52.102.0.0/16 ip4:52.103.0.0/17 ip4:104.47.0.0/17 ip6:2a01:111:f400::/48 ip6:2a01:111:f403::/49 ip6:2a01:111:f403:8000::/51 ip6:2a01:111:f403:f000::/52 -all
```

