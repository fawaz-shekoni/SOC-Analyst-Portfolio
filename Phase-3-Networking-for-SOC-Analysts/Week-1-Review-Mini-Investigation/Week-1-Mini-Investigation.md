# Week 1 — Mini-investigation: network alert and Facebook connectivity

## Scenario

This is a **simulated SOC alert**, not a confirmed security incident on my computer:

> A workstation is communicating with an unfamiliar external IP address. The user also reports that Facebook is not opening properly.

I used real, point-in-time command results from my main Windows PC to practise the investigation. The alert did not include the external IP, timestamp, port, or process.

- [Command evidence transcript](evidence/command-results.md)

## What I checked

| Check | What I used it for |
| --- | --- |
| `ipconfig /all` | Review the active Wi-Fi address, gateway, DHCP, and DNS configuration |
| `nslookup facebook.com` | Check whether my configured resolver returned addresses for Facebook |
| `ping 1.1.1.1` | Check whether an external IP replied to basic connectivity tests |
| `arp -a` | Review learned IP-to-device mappings on the local network |
| `Get-NetTCPConnection -State Established` | See established TCP connections and their remote addresses, ports, and process IDs |
| `Test-NetConnection facebook.com -Port 443` | Check TCP reachability to Facebook on the HTTPS port |

## Evidence and findings

- My active Wi-Fi adapter had DHCP enabled, a subnet mask of `255.255.255.0`, and `192.168.0.1` as its gateway, DHCP server, and DNS server. The local IPv4 address is shown as `192.168.0.200` in the published evidence as an anonymized display value.
- `nslookup facebook.com` returned IPv4 and IPv6 addresses through the configured resolver. The resolver was labelled `UnKnown`, but the lookup succeeded.
- `ping 1.1.1.1` received four replies with no packet loss; the average round-trip time was 145 ms.
- The Wi-Fi ARP table had dynamic entries for the gateway (`192.168.0.1`) and another local device (`192.168.0.107`). The VMware adapter entries were separate virtual interfaces.
- The TCP snapshot showed multiple established connections, many to remote port 443. It also included connections on other ports and to loopback/local addresses. The process IDs were not mapped to process names in this exercise.
- `Test-NetConnection facebook.com -Port 443` succeeded over Wi-Fi and resolved to the same IPv4 address returned by the DNS lookup.

At the time of these checks, DNS resolution for Facebook, replies from the external ping target, and a TCP connection to Facebook on port 443 all worked. This provides evidence of basic connectivity at that moment.

## What I could not determine

- I could not identify which connection the simulated alert referred to because it supplied no exact external IP, timestamp, or port.
- The TCP output alone does not show which application owned each process ID or whether a destination is expected for that application.
- I did not establish whether any connection was malicious.
- A successful DNS lookup and TCP connection do not prove that the Facebook page rendered correctly in a browser. They also do not rule out an intermittent or browser-specific problem.
- No real alert or Facebook outage was confirmed during this exercise.

## What I would investigate next

For a real SOC alert, I would obtain the exact remote IP, timestamp, port, protocol, and alert outcome, then correlate those details with the connection and endpoint/firewall logs. I would map the owning process ID to its process name and executable path, verify its publisher, and check whether the destination is expected for that application. I would compare the event with the user's normal activity and network baseline before deciding whether it is suspicious.

If Facebook still failed in a real report, I would record the browser error and time, test the HTTPS response, and compare with another site or device. I would avoid changing network settings until the failure is reproducible and the evidence points to a cause.

## Privacy and evidence note

The local Wi-Fi IPv4 address `192.168.0.121` is changed to `192.168.0.200` in the published transcript. This is explicitly identified as an anonymized display value. The evidence transcript contains representative output transcribed from the screenshots; the screenshot image files are not attached in this submission.
