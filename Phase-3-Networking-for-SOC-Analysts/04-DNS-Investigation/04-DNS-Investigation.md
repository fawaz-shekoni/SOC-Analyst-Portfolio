# 04 — DNS Investigation

## Objective

I used PowerShell to examine several DNS record types for `microsoft.com` and to follow the SPF policy's `include` references. I also compared a TXT lookup through my configured DNS resolver with a lookup sent directly to `1.1.1.1`. This helped me distinguish a record result from a resolver timeout and understand how DNS information can support email-security investigations.

## Investigation steps

1. I queried the domain's MX and NS records with `Resolve-DnsName`.
2. I looked up the DMARC TXT record at `_dmarc.microsoft.com`.
3. A TXT lookup for the root domain through my configured resolver at `192.168.0.1` returned `Bad DNS packet`; the equivalent `nslookup` request timed out.
4. I repeated the TXT lookup against `1.1.1.1`. It returned TXT records, confirming that the earlier timeout did not prove the domain had no TXT records.
5. I filtered the result for the SPF record, then queried the referenced `_spf-a.microsoft.com` and `spf.protection.outlook.com` policies to inspect the nested `include` chain.

The output transcribed from my screenshots is in [evidence/command-results.md](evidence/command-results.md). I did not change my DNS settings. These results describe the records returned at the time of the checks; DNS records can change.

## Findings

### DNS records

| Query | Observed result | What it tells me |
|---|---|---|
| `microsoft.com` A / AAAA | `150.171.109.1` / `2603:1061:14:1::1` | IPv4 and IPv6 addresses returned in the earlier lookup. |
| `microsoft.com` MX | `microsoft-com.mail.protection.outlook.com`, preference `10` | The published mail exchanger target. A lower preference value takes priority if multiple MX records are present; this query returned one. |
| `microsoft.com` NS | `ns1-39.azure-dns.com`, `ns2-39.azure-dns.net`, `ns3-39.azure-dns.org`, `ns4-39.azure-dns.info` | The name servers listed for the domain. |
| `_dmarc.microsoft.com` TXT | `v=DMARC1; p=reject; pct=100; rua=...; ruf=...; fo=1` | A published DMARC policy and reporting requests. The screenshot includes the reporting addresses; see the transcript for the observed value. |
| `microsoft.com` TXT (configured resolver) | `Resolve-DnsName` returned `Bad DNS packet`; `nslookup` timed out | The configured resolver did not provide a usable answer to this query at that time. This does not prove that the TXT record is absent. |
| `microsoft.com` TXT (1.1.1.1) | Multiple TXT records returned | The domain has TXT records; a query through another resolver succeeded. |

The DNS server name displayed as `Unknown` for `192.168.0.1` means the tool did not display a hostname for that resolver address. The address still answered other queries in this investigation.

### SPF policy chain

The root SPF record returned by `1.1.1.1` was:

```text
v=spf1 include:_spf-a.microsoft.com include:_spf-b.microsoft.com include:_spf-ssg-a.msft.net include:_spf1-meo.microsoft.com -all
```

An `include` tells the SPF evaluator to check the referenced domain's SPF policy as part of evaluating the sending IP. The root policy ends in `-all`, a Fail result for a sender that matches none of the preceding mechanisms. The first included policy, `_spf-a.microsoft.com`, listed IPv4 mechanisms, an IPv4 `/28` range, another include for `spf.protection.outlook.com`, and ended in `~all` (SoftFail). The nested Outlook policy listed IPv4 and IPv6 ranges and ended in `-all`.

These are policy definitions, not a test of any particular email. I did not send or analyze a message, so I cannot say whether a specific sender passes SPF or DMARC. SPF qualifiers and includes are interpreted as part of the full policy chain; the child record's `~all` is not the same thing as the root record's `-all`. [RFC 7208](https://www.rfc-editor.org/rfc/rfc7208.html) defines SPF mechanisms, includes, and qualifiers.

The DMARC record's `p=reject` requests rejection of mail that fails DMARC validation, subject to receiving systems' own handling. `rua` requests aggregate reports; `ruf` requests message-specific failure reports; and `fo=1` requests a failure report if any underlying authentication mechanism does not produce an aligned pass. The visible `pct=100` is a legacy tag; the current DMARC specification has removed `pct`, and 100 had represented full policy application in the earlier specification. [RFC 9989](https://www.rfc-editor.org/rfc/rfc9989.html)

## Resolver comparison

The lookup through `192.168.0.1` failed or timed out for this root TXT query, while the request to `1.1.1.1` returned records. This points to a difference in the resolver path or response handling for this query. It does not identify the exact cause, and it does not show that my general internet connection or all DNS lookups are failing. Microsoft documents malformed DNS packets and large or unsupported DNS responses as troubleshooting areas, but I did not perform a packet capture to determine the cause. [Microsoft DNS error codes](https://learn.microsoft.com/en-us/windows/win32/debug/system-error-codes--9000-11999-), [Microsoft troubleshooting for unsuccessful DNS queries](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/dns-name-queries-not-success)

## My conclusion

I successfully identified the domain's mail exchanger, listed name servers, DMARC policy, and SPF policy chain using DNS queries. Comparing resolvers showed that the root TXT lookup problem was limited to the request through my configured resolver during this check. I would not conclude that a record is missing based only on a timeout, or treat a published SPF/DMARC policy as proof that a specific email is legitimate or malicious.

## Recommended next steps

If I needed to investigate a real email, I would preserve its full headers and evaluate the sending IP, SPF result, DKIM result, and DMARC alignment together. For a repeated DNS issue, I would repeat the same query through the configured resolver and a comparison resolver, record the time and result, and then collect DNS traffic or consult router/ISP DNS logs before changing settings. I would not change the device's DNS configuration based on this single query.

## Evidence

- [Command results transcribed from my screenshots](evidence/command-results.md)
- [Evidence notes](evidence/README.md)

The screenshot images themselves were not available as files in my working folder, so I preserved the observed command results as a text transcript rather than creating replacement screenshots.

