# App Dashboard sign-in (team member SSO, Google, 2FA)

Covers this Salesforce bucket: App Settings/Config > Dashboard SSO.
This file is about the app team's own staff signing in to the Clever App Dashboard (the app team-facing interface). It's not about building or troubleshooting SSO or SAML in a partner's own app, which is covered in 04-sso-and-oidc.md and 06-certification-and-onboarding.md.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it.

---

## DSH-01 · "We want our team to sign in to the Clever App Dashboard with our own SSO (Okta, Azure, Duo). What do you need from us?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A current app partner asks to put their dashboard team behind a SAML identity provider.
- First confirm they mean their own staff signing in to the App Dashboard, not SSO for their end users. In one case (03057539) the front line had to ask this. If it's about end-user SSO, use the SSO entries instead.

**Answer / steps**
1. Confirm it's for dashboard sign-in by their own staff.
2. Collect what PE needs: the identity provider's SAML metadata (file or URL), and whether assertions will be encrypted.
3. Transfer the case to PE. PE configures it on Clever's side. There's no partner-facing setup screen for this (PE said so in 03076856), and CS can't set it up.
4. Send the partner the Clever-side values to enter in their identity provider (below).
5. Don't ask the partner to email passwords or certificates. A metadata file or URL is what PE needs.

> Happy to help with this! To get started, please send over your identity provider's SAML metadata file or URL, and let us know whether your assertions will be encrypted.
>
> In your identity provider's console, please:
> - Add https://clever.com/oauth/saml/metadata.xml as the EntityID.
> - Add https://clever.com/oauth/saml/assert as the Assertion Consumer Service URL (ACS URL).
> - Add a SAML assertion attribute `clever.email` that is mapped to the email address of the app admin in Clever.
>
> Our Partner Engineering team will let you know once Clever's side is configured so you can test. Here's our help article on App Dashboard SSO: https://support.clever.com/hc/s/articles/000001564?language=en_US

**Escalate if…**
- Always for the actual configuration.
- The partner already has dashboard SSO and wants to change something (see DSH-04), or asks to bulk-add admins or pull a list of users and permissions.

**Notes / conflicts**
- These Clever-side values are for App Dashboard SSO only. Don't use them for a SAML SSO production app, which is a different setup.
- Okta, Azure and Duo all came up in the cases. PE confirmed Okta is supported. PE said Clever doesn't support SCIM or just-in-time provisioning (02994250).
- A partner with several email domains and app families wanted everyone on SSO. PE was still checking whether bulk-uploading admins is supported, so don't promise it (03073652).
- A front-line reply in 03076856 said the dashboard SSO help article was mainly for district tenants. PE later said it's the right guide for partner accounts. Don't repeat the earlier claim.
- Dashboard users need App Dashboard SSO (SAML), Sign in with Google, or 2FA. SSO is the most complete option but isn't required if one of the others is used.
- No turnaround time for configuration is stated in the cases.

**Links:** https://support.clever.com/hc/s/articles/000001564?language=en_US
**Source cases:** 02994250, 03057539, 03076856, 03104547, 03073652

---

## DSH-02 · "We use Sign in with Google. Can you confirm password logins are turned off? (security audit)"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes. Escalate only if they want managed SSO.

**When to use this**
- A partner asks whether passwords can be disabled for dashboard users, or whether email and password sign-in without 2FA is possible, usually for a security audit.

**Answer / steps**
Dashboard users have three sign-in options: App Dashboard SSO (SAML), Sign in with Google, and two-factor authentication. Every user needs one of them. Per PE (03070271):
- Sign in with Google uses the built-in OAuth Clever added for the App Dashboard. It isn't a managed SSO connection, so it doesn't turn off email and password login.
- A Google user who signs in with a password is still required to use 2FA.
- Managed App Dashboard SSO (SAML) is the only option that fully disables password logins and enforces SSO-only sign-in for the set of apps specified.

> Thanks for reaching out! Clever offers three ways to sign in to the App Dashboard: App Dashboard SSO, Sign in with Google, and two-factor authentication. Sign in with Google isn't a managed connection, so it doesn't turn off password logins. If someone signs in with a password anyway, they're still required to use 2FA, so basic email and password sign-in without 2FA isn't allowed for dashboard users. If you need password logins fully disabled, we can set up App Dashboard SSO with your identity provider. Let me know if you'd like to explore that.

**Escalate if…**
- They want password logins disabled. That's managed SSO through PE (see DSH-01).
- They need an audit-ready document. PE said this isn't in the Help Center and offered to pass on the feedback.

**Notes / conflicts**
- In one case (03085255) PE removed MFA enforcement for one sandbox QA account so automated tests could run without codes. That was an exception and doesn't fit the current rule. Don't offer it. Send any such request to PE, and say every dashboard user needs SSO or 2FA.

**Links:** https://support.clever.com/hc/s/articles/000001564?language=en_US
**Source cases:** 03070271, 03085255

---

## DSH-03 · "A team member can't sign in to the dashboard with SSO. It says there's no Clever account for my email."
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner admin covered by dashboard SSO gets an error such as "we can't find a Clever account for the email", "Login information not found", or "check your account setup". Often password login still works.

**Answer / steps**
1. Ask them to compare the exact email on their Clever team-member record with the email their identity provider sends. Typos matter. In one case (03058436) the stored email had one wrong letter, and PE had to get the record corrected.
2. If their email domain recently changed (a merger or rebrand), the new domain may not be on the partner's SSO setup. In 03083062 and 03085255 it wasn't, and PE had to add it. CS can't add domains. CS can check whether a colleague can edit the team member's email.
3. Ask for a screen recording of the error from a private browser window, the URL on the error page, and whether signing in from the Clever login page works as well as from the identity provider tile. PE used these steps in 03058436.
4. Transfer to PE with the above.

> Thanks for the details! To help us narrow this down, could you try a private browser window, go to the Clever login page, choose the SSO sign-in option, and record your screen as you go through the login? Please also tell us the exact email address on the team member's Clever account and the one your identity provider sends. Our Partner Engineering team will take it from here.

**Escalate if…**
- Always, when the cause is a backend email typo, a missing domain, or a record that needs correcting.

**Notes / conflicts**
- In 03058436 the account had been deleted and recreated, and the typo carried over. That was the cause PE found there, and it isn't confirmed as a general pattern.
- The replies don't name the dashboard menu for editing team members.

- If a partner needs access to an app dashboard account they don't own, find the existing admins on the Team page. They can add team members. PE can grant access if nobody is reachable.
- After a company email-domain change, ask whether everyone will use the new domain and whether the sign-in provider supports the old one, because matching is on email (02982038). To add one person to every app, a team member adds them on each app's team members tab (02982933). If "Go to Portal" does nothing for one person, try an incognito window or reset the browser session (03024219).
**Links:** https://support.clever.com/hc/s/articles/000001564?language=en_US
**Source cases:** 02982038, 02982933, 03024219, 03119634, 03058436, 03083062, 03085255

---

## DSH-04 · "We need to change our dashboard SSO setup: new certificate, new IdP, new domain, or more apps."
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- The partner already has App Dashboard SSO and needs a change. Examples from the cases: an expired SAML signing certificate, switching identity provider, a new email domain, a domain linked to another company's SSO setup, admins who can't be added to some apps, or apps not yet covered by SSO.

**Answer / steps**
All of these are PE tasks. Gather the details, then transfer the case.
- **Expired certificate (03072708):** partners can't manage this themselves. Ask for the new metadata file. PE uploaded it to the partner's SSO setup and it worked the same day.

> Thanks for letting us know! Please send the new SAML metadata file for your identity provider and we'll update the configuration on our side and let you know when it's ready to test.

- **Changing identity provider (03076856):** PE updates the existing setup. Collect the new provider's metadata and encryption details (see DSH-01).
- **New or changed email domain (03083062, 03085255):** PE adds the domain to the SSO setup.
- **Admins can't be added to some apps (03065284):** PE explained that with dashboard SSO on, people with an SSO-covered email domain can't be added to apps outside the partner's SSO group. Options PE gave: add those apps to the SSO group (recommended, and existing admins on other domains would then have to use the identity provider), or use a different email address that isn't covered by SSO.
- **A domain tied to another company's SSO setup (03013738):** PE asked whether the partner wants a brand-new SSO setup unrelated to the other company. The outcome isn't visible.
- **Reviewing team members or audit logs across apps (03117623, 03073652):** PE can pull lists or logs manually. There's no self-service report today. The partner's request was logged as feedback. Don't promise delivery dates.

**Escalate if…**
- Always. PE does the change.

**Notes / conflicts**
- Don't tell partners where to find their SSO setup in the dashboard. PE said there's no UI for it (03076856).
- Don't offer one-off exceptions to the sign-in rule (see DSH-02).
- Unconfirmed: whether admin lists can be bulk-uploaded (03073652), and whether PE has a set turnaround.

**Links:** https://support.clever.com/hc/s/articles/000001564?language=en_US
**Source cases:** 03086631, 03111510, 03015166, 03072708, 03076856, 03083062, 03085255, 03065284, 03013738, 03117623, 03073652, 03067111
