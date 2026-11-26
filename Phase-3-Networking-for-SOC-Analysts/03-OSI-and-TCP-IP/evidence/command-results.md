# Command results transcribed from screenshots

These results were captured on my main PC. I changed only my local Wi-Fi address to `192.168.0.200` in this public transcript; the screenshot showed `192.168.0.121`.

## DNS lookup

Commands shown: `nslookup microsoft.com` and `Resolve-DnsName microsoft.com`

```text
DNS server: 192.168.0.1 (server name displayed as Unknown)
microsoft.com A     150.171.109.1
microsoft.com AAAA  2603:1061:14:1::1
```

## TCP connectivity to port 443

Command shown: `Test-NetConnection microsoft.com -Port 443 -InformationLevel Detailed`

```text
ComputerName       : microsoft.com
RemoteAddress      : 150.171.109.1
RemotePort         : 443
NameResolutionResults : 150.171.109.1
NetworkIsolationContext : Internet
IsAdmin            : False
InterfaceAlias     : Wi-Fi
SourceAddress      : 192.168.0.200 (anonymized)
NetRoute (NextHop) : 192.168.0.1
TcpTestSucceeded  : True
```

## HTTP redirect

Command shown: `curl.exe -i microsoft.com`

```text
HTTP/1.1 307 Temporary Redirect
Location: https://microsoft.com/
```

## HTTPS canonical redirect

Command shown: `curl.exe -I https://microsoft.com`

```text
HTTP/1.1 301 Moved Permanently
Location: https://www.microsoft.com/
Strict-Transport-Security: max-age=31536000
```

## Final response after following redirects

Command run: `curl.exe -I -L https://microsoft.com`

```text
HTTP/1.1 200 OK
Server: AkamaiGHost
Content-Type: text/html
Content-Length: 419
```

The screenshots show the intermediate `307` and `301` responses and the final `200 OK`. Redirects are expected here; the final successful response demonstrates that the request reached the website after following them.

