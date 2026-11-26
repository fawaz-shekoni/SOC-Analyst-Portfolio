# 06 — Windows Firewall

## Purpose

I used read-only PowerShell commands to learn how Windows Firewall profiles and rules are configured on my main PC. I did not turn off the firewall or change any rules.

- [PowerShell commands and evidence transcript](evidence/command-results.md)

## Firewall fundamentals

A firewall compares network traffic with rules and applies the matching action.

- **Inbound** means traffic trying to reach my computer.
- **Outbound** means traffic leaving my computer.
- **Allow** means permit traffic that matches the rule.
- **Block** means stop traffic that matches the rule.
- A rule can depend on its direction, network profile, application or service, protocol, port, and address.
- A rule listing shows configuration. It does not, by itself, prove that a connection was attempted or succeeded.

Windows Firewall has three profiles:

- **Domain** applies to a computer connected to a managed organization network.
- **Private** is for a network Windows treats as trusted.
- **Public** is for a network Windows treats as untrusted, such as public Wi-Fi.
- **Any** means the rule can apply across profiles, if its other conditions match.

## Practical 1 — Check firewall status

I ran `Get-NetFirewallProfile`. The output showed:

| Profile | Enabled | Default inbound action | Default outbound action |
| --- | --- | --- | --- |
| Domain | True | NotConfigured | NotConfigured |
| Private | True | NotConfigured | NotConfigured |
| Public | True | NotConfigured | NotConfigured |

All three profiles showed enabled at the time of the check. `NotConfigured` does not mean the firewall is off, and I did not infer the effective default actions from these fields alone. The output also showed `LogAllowed: False` and `LogBlocked: False` for each profile.

## Practical 2 — Inspect a sample of firewall rules

I displayed the first 20 rules. Seven of the 20 shown were enabled. Some rows had `Action: Allow` while `Enabled: False`; those rules were disabled. The enabled rows included both inbound and outbound rules.

This helped me distinguish the rule's configured action from whether the rule is enabled.

## Practical 3 — Find enabled inbound rules

I listed enabled inbound rules and their profiles. Examples included core networking rules with profile `Any` and Wi-Fi Direct printing/scanning rules with profile `Public`. `Any` does not mean every inbound connection is automatically allowed; the traffic still has to match the rule's other conditions.

## Practical 4 — Find enabled outbound rules

I listed enabled outbound rules. The displayed examples used several profiles, including Domain, Private, Public, and Any, and showed Allow actions. Each rule applies to traffic that matches its conditions; an Allow rule is not a record that an outbound connection happened.

## Practical 5 — Investigate a simulated firewall alert

**This is a training scenario I created for practice. It is not an alert or connection observed on my computer.**

The simulated alert described an inbound TCP connection reportedly permitted to local port `50000` on the Public profile, associated with `spoolsv.exe` / the `Spooler` service. The example source address, `198.51.100.25`, is reserved for documentation examples. The port and alert details are simulated.

I compared the scenario with a real Windows Firewall rule I had previously inspected: **Wi-Fi Direct Spooler Use (In)**. That enabled rule is inbound, Allow, and scoped to Public. Its application and service filters identify `spoolsv.exe` and `Spooler`; the protocol, port, and remote-address fields I inspected showed `Any`.

Those visible conditions make it a rule worth checking against the simulated alert. They do not prove that the rule allowed a real connection, and I did not inspect every possible rule condition. For a real alert, I would check the timestamp and result in available firewall or endpoint logs, verify whether Wi-Fi Direct printing is expected, and have an administrator review the complete rule conditions before making any change.

## Findings

The profile check showed Windows Firewall enabled for Domain, Private, and Public. The rule listings showed that enabled Allow rules exist for both inbound and outbound traffic, with different profile scopes. The rule details and simulated alert exercise helped me distinguish a configured permission from evidence of actual network activity.

I did not identify or investigate a real inbound alert during this project. I made no firewall changes.

## Evidence and limitations

The linked transcript includes the commands I ran and representative output transcribed from the screenshots I shared. The screenshots themselves are not attached to this repository version. Some command output was long, so the evidence transcript labels excerpts and representative rows as such. The simulated alert is kept separate from my real computer results.
