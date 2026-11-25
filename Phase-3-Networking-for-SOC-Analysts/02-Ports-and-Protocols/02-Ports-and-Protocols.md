# 02 — Ports and Protocols

## Objective

I used PowerShell to look at a small snapshot of established TCP connections on my main PC, then matched the owning process IDs to process names. I used this to practise reading local and remote ports, distinguishing a client-side port from a service port, and checking whether a connection fits the application using it.

## Investigation steps

1. I ran `Get-NetTCPConnection -State Established` and selected the first five rows with local address, local port, remote address, remote port, state, and owning process ID.
2. I mapped the process IDs shown in those rows with `Get-Process -Id 11652,17740,8984,21632 | Select-Object Id,ProcessName`.
3. I compared the remote port numbers with common service-port uses. For the Apple process using port 5223, I checked Apple's documentation for the expected APNs port and network range.
4. I treated the output as a point-in-time snapshot. I did not infer the content of encrypted traffic or declare a connection suspicious based only on its IP address or port.

## Evidence

- [Established TCP connections](evidence/established-connections.png): five connections with their local and remote endpoints and owning PIDs.
- [Process ID mapping](evidence/process-mapping.png): maps the PIDs from the connection output to `chrome`, `APSDaemon`, `ChatGPT`, and `OneDrive.Sync.Service`.

The private Wi-Fi address shown in the published connection screenshot is changed to `192.168.0.200` for privacy. This is an anonymized display value; the original command output showed my actual local address. The remote endpoints, ports, PIDs, and process names were retained from my screenshots.

## Findings

| Local port | Remote endpoint | Remote port | Process | Interpretation |
|---:|---|---:|---|---|
| 64319 | `104.18.36.252` | 443 | `chrome` (PID 11652) | Consistent with a browser TCP connection using the commonly used HTTPS port. |
| 63495 | `17.57.146.183` | 5223 | `APSDaemon` (PID 17740) | Consistent with Apple Push Notification Service (APNs). Apple documents TCP 5223 for APNs, and the remote address falls within Apple's listed `17.57.144.0/22` range. |
| 62848 | `104.26.7.190` | 443 | `chrome` (PID 11652) | Consistent with another browser TCP connection using port 443. |
| 60778 | `172.64.155.209` | 443 | `ChatGPT` (PID 8984) | Consistent with an application TCP connection using port 443. |
| 59508 | `20.52.64.202` | 443 | `OneDrive.Sync.Service` (PID 21632) | Consistent with a sync-service TCP connection using port 443. |

In these rows, the high-numbered local ports are client-side ports used for the outbound connections; the remote ports indicate the destination service ports. Port 443 is commonly used for HTTPS over TCP, while TCP 5223 is used by Apple devices to communicate with APNs. These conventions help with triage but do not, by themselves, prove the exact application protocol or the legitimacy of a particular process.

All five entries were in the `Established` state when I ran the command. That means a TCP connection was active at capture time. It does not show what data was sent, whether the applications' files are genuine, or whether the same connections persist. The process mapping and expected port use make these rows understandable, and this snapshot alone does not provide evidence of suspicious activity.

## Common port reference

These are common/default uses; actual services can use different ports, and a port number alone does not identify traffic conclusively.

| Port | Transport | Common use |
|---:|---|---|
| 20/21 | TCP | FTP data/control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet (unencrypted remote terminal) |
| 25 | TCP | SMTP mail transfer |
| 53 | UDP/TCP | DNS queries and responses; TCP is also used for larger responses and zone transfers |
| 67/68 | UDP | DHCP server/client |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 123 | UDP | NTP time synchronisation |
| 143 | TCP | IMAP |
| 443 | TCP, and UDP for HTTP/3 (QUIC) | HTTPS and secure web traffic |
| 445 | TCP | SMB file and printer sharing |
| 3389 | TCP/UDP | Remote Desktop Protocol (RDP) |
| 5223 | TCP | Apple Push Notification Service (APNs) device connection |

## Recommended resolution / next steps

No change was needed based on this snapshot. If I investigated an unfamiliar connection, I would first identify the process path and publisher, check whether the destination and port match the application's expected behaviour, and compare repeated observations with a known-good baseline. I would also correlate with firewall, DNS, or endpoint-security logs before deciding whether to block or escalate it. I would not block a connection or label it malicious solely because it uses an unfamiliar IP or a commonly used port.

## Sources

- Apple Support, [If your Apple devices aren't receiving Apple push notifications](https://support.apple.com/en-gb/102266) — documents TCP 5223 for APNs and lists the `17.57.144.0/22` Apple network range.
- Microsoft Learn, [Get-NetTCPConnection](https://learn.microsoft.com/en-us/powershell/module/nettcpip/get-nettcpconnection) — documents the Windows PowerShell command used to inspect TCP connections.

