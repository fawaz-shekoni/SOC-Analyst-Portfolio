# Groups and Privileges

## Group membership observed

The command used while signed in as ANJI was:

```powershell
whoami /groups
```

It reports the groups and security identifiers in the current sign-in token. The output included:

| Group or label | What it indicates |
|---|---|
| `BUILTIN\Users` | ANJI is in the standard local Users group. |
| `Everyone` | A broad Windows security principal present in access checks. |
| `NT AUTHORITY\Authenticated Users` | The session is authenticated. |
| `NT AUTHORITY\INTERACTIVE` | The account is signed in locally/interactively. |
| `CONSOLE LOGON` | The session is a console sign-in. |
| `NT AUTHORITY\Local account` | The sign-in is using a local account. |
| `NT AUTHORITY\NTLM Authentication` | A Windows authentication-related security group in the token. |
| `Mandatory Label\Medium Mandatory Level` | The process runs at the normal medium integrity level. |

The captured output did not list `BUILTIN\Administrators`, consistent with ANJI being a standard user. `whoami /groups` can show Windows security principals and integrity labels as well as local groups.

## Least privilege

The **principle of least privilege** means giving an account only the access it needs to perform its task. A standard account is usually sufficient for everyday use. Limiting administrator access reduces the potential impact of a mistaken system change or malicious software running under that account.

In this lab, ANJI could read/list the permission-test folder but was denied Write on that folder. The administrator account retained the ability to create a file there.

## Investigating a folder access problem

A useful first-pass investigation is:

1. Confirm the signed-in identity with `whoami`.
2. Review group membership with `whoami /groups`.
3. Confirm the exact file or folder path.
4. Inspect **Properties → Security** for the relevant user and groups.
5. Use **Advanced → Effective Access** to evaluate the intended user.
6. Test the operation using the affected account and record the exact result.

Avoid using a prompt to permanently take ownership or grant access as a shortcut; that changes the configuration being investigated.


