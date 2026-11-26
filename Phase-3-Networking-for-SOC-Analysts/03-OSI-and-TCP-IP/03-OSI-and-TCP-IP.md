# 03 — OSI and TCP/IP Communication

## Objective

I traced a connection to `microsoft.com` from name resolution through TCP connectivity and the HTTPS response. I used PowerShell and `curl.exe` on my main PC, then mapped the evidence to the OSI and TCP/IP models.

## What I did

1. I looked up `microsoft.com` with `nslookup` and `Resolve-DnsName`.
2. I tested a TCP connection to remote port 443 with `Test-NetConnection`.
3. I requested the site over HTTP and observed its redirect to HTTPS.
4. I requested the HTTPS site and followed its redirects to the final response.

The command outputs transcribed from my screenshots are in [evidence/command-results.md](evidence/command-results.md). The local Wi-Fi address is shown there as `192.168.0.200` to protect my exact address; the original screenshot showed `192.168.0.121`. The DNS answer, gateway, target, ports, and observed HTTP status codes are unchanged.

## Results

### DNS lookup

Both lookup commands succeeded using the DNS server at `192.168.0.1`. `nslookup` displayed the server name as `Unknown`, but still returned an answer. `Resolve-DnsName` showed:

| Record | Result |
|---|---|
| A (IPv4) | `150.171.109.1` |
| AAAA (IPv6) | `2603:1061:14:1::1` |

The `Test-NetConnection` output selected the IPv4 address `150.171.109.1` for its TCP test.

### Routing and transport

`Test-NetConnection microsoft.com -Port 443 -InformationLevel Detailed` reported:

- `InterfaceAlias`: Wi-Fi
- `SourceAddress`: `192.168.0.200` (anonymized display value)
- `NetRoute (NextHop)`: `192.168.0.1`
- `RemoteAddress`: `150.171.109.1`
- `RemotePort`: `443`
- `TcpTestSucceeded`: `True`

This shows that Windows resolved the name, selected Wi-Fi and a route through the listed next hop, and completed a TCP connection to port 443. It does not inspect the HTTP contents or by itself test a TLS session.

### HTTP redirects and HTTPS response

- A request to `microsoft.com` returned `HTTP/1.1 307 Temporary Redirect` with a `Location` pointing to `https://microsoft.com/`.
- A request to `https://microsoft.com` returned `HTTP/1.1 301 Moved Permanently` with a `Location` pointing to `https://www.microsoft.com/`. The response included a `Strict-Transport-Security` header.
- After following the redirects, the final response was `HTTP/1.1 200 OK` from `AkamaiGHost`.

The 307 and 301 responses are redirects, not connection failures. The final 200 response shows that the HTTP request completed successfully after following the HTTPS redirects. Since `curl.exe` received the HTTPS response without reporting a TLS validation error, the HTTPS exchange completed for this request.

## Mapping the evidence to the models

| OSI layer | TCP/IP layer | Role in this connection | Evidence / limit |
|---|---|---|---|
| 7 — Application | Application | DNS lookup and the HTTP request/response for the website | The lookups returned A/AAAA records; `curl.exe` showed redirects and final `200 OK`. DNS is an application protocol, commonly using UDP or TCP port 53. |
| 6 — Presentation | Application | TLS protects the HTTPS exchange and handles presentation of encrypted data | The HTTPS request returned a response without a TLS error. These commands do not expose packet contents or TLS negotiation details. |
| 5 — Session | Application | Maintains the exchange while the client communicates with the server | The command output does not provide a separate session-layer trace. |
| 4 — Transport | Transport | TCP carries the connection to remote port 443 | `Test-NetConnection` showed `TcpTestSucceeded: True`. The local ephemeral port was not shown in this test output. |
| 3 — Network | Internet | IP addressing and routing move traffic toward the destination | The output showed remote IPv4 `150.171.109.1` and next hop `192.168.0.1`. |
| 2 — Data Link | Link | Wi-Fi carries frames between my PC and the local network access point/router | The interface was listed as Wi-Fi. I did not capture frames, so MAC addresses and frame exchange were not examined. |
| 1 — Physical | Link | Radio signal carries Wi-Fi bits over the air | This is implied by using Wi-Fi; I did not measure or capture the radio signal. |

The OSI and TCP/IP models group some functions differently. The table is a learning map, not a claim that each command directly observed every layer. In particular, successful name resolution, TCP connectivity, and an HTTPS response are separate observations.

## Findings

At the time of the checks, DNS returned IPv4 and IPv6 records, the PC had a successful TCP connection to the resolved IPv4 address on port 443, and the website returned redirects followed by a final `200 OK`. The evidence supports successful connectivity for this one request. It does not establish general network performance, prove that every website works, or show the contents of encrypted traffic.

## Recommended next steps

No repair was indicated by these results. If I were investigating a reported web-access issue, I would repeat the checks against the affected hostname and compare results across time or another device. I would identify which stage fails—name resolution, route/TCP connection, TLS, or HTTP response—before recommending a change. I would preserve the error/status output and timestamps, and avoid changing DNS or firewall settings without evidence pointing to them.

## Evidence files

- [Command results transcribed from my screenshots](evidence/command-results.md)
- [Evidence notes](evidence/README.md)

