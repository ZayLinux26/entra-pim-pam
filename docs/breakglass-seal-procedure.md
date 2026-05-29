# Break-Glass Account — Seal Procedure

> Standard Operating Procedure for emergency access to the Meridian Health Partners Entra tenant.

## Account Details

- **UPN:** `breakglass@isaiahherard26gmail.onmicrosoft.com`
- **Display name:** Break-Glass Account (Emergency Access)
- **Role:** Global Administrator (Active, permanent, PIM-bypassed)
- **MFA:** Google Authenticator on a dedicated authenticator instance
- **Conditional Access:** Excluded from all CA policies
- **Credentials sealed in:** Bitwarden vault — `Meridian-PAM-Vault` folder, entry name `breakglass (Tier 0 - Emergency Access)`

## When to break the seal

Break-glass is reserved for situations where standard administrative access is unavailable:

- Compromise or lockout of all standing admin accounts (`meridian-admin`, `adm-*` accounts)
- PIM service outage preventing role activation
- Conditional Access policy misconfiguration locking out all admins
- MFA service outage affecting all primary admin authenticators
- Microsoft service incidents affecting tenant administration

Break-glass is **NOT** for:

- Routine administration (use `meridian-admin` or PIM-eligible admins)
- Convenience or speed — PIM activation should never be bypassed for time savings
- Bulk operations that PIM-active sessions can handle

## Who is authorized to break the seal

For this lab environment:
- Isaiah Herard (portfolio owner / sole operator)

In a production deployment, this list would be small and explicit — typically the CISO, senior IAM lead, and one designated on-call principal. The break-glass account must never be a free-for-all.

## How to break the seal

1. Retrieve credentials from Bitwarden vault (`Meridian-PAM-Vault → breakglass`)
2. Open a private/incognito browser window
3. Navigate to `entra.microsoft.com`
4. Sign in with the break-glass UPN and sealed password
5. Approve MFA via Google Authenticator
6. Perform **only** the minimum necessary action to restore standard administrative access

## During the break — required actions

- Log the break event: timestamp, reason, operator name, actions taken
- Document any tenant configuration changes made
- Do **not** perform routine work — break-glass is for restoration, never for operations

## Post-break — required actions (within 24 hours)

1. **Rotate password:** generate a new 40-character password in Bitwarden; force password reset on the break-glass account
2. **Re-register MFA:** unregister current Google Authenticator entry; register a fresh QR code
3. **Audit review:** export Entra sign-in logs for the break-glass account during the break window; review every action
4. **Notify stakeholders:** in production, inform CISO and IR team; document the incident
5. **RCA:** root cause analysis of the incident that necessitated break-glass use
6. **Re-seal:** update this procedure file with new credentials reference and any process changes
