# UAC and Access Control

## Authentication and access control

**Authentication** checks who is signing in (for example, with a password, PIN, or biometric sign-in). **Access control** decides what that signed-in account can read or change, based on its permissions and group membership. Using a standard account for everyday activity limits the damage a program can do; administrator access should be used only when needed.

**User Account Control (UAC)** adds a consent prompt when an action needs administrator-level permission. A prompt is a chance to check which app is requesting a change and deny it if the request is unexpected. UAC does not replace antivirus or a firewall.

## VM findings

- Account protection showed that the VM was **not signed in to a Microsoft account**.
- **Windows Hello** was not set up.
- **Dynamic lock** was not set up; the page said it was available.
- The UAC slider was at the top **Always notify** position. The description said prompts appear when apps try to make changes and when I change Windows settings. I reviewed the slider and did not move it.
- The slider screenshot was cropped before publication to remove the VM name visible behind the settings window.

## How I would check it

Open **Account protection** to review available sign-in methods; do not sign in to an account just for this check. Search for **Change User Account Control settings**, note the slider position, and close the window with **Cancel** without moving the slider. Review a user’s groups and file/app permissions separately when investigating what that account is allowed to do.

Microsoft: [User Account Control overview](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/) · [Local accounts and access control](https://learn.microsoft.com/en-us/windows/security/identity-protection/access-control/local-accounts)
