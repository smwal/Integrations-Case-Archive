# SSO, OIDC & login

Covers these Salesforce buckets: Developer Support > Troubleshooting - SSO, Developer Support > Best Practices - SSO, and Developer Support > OIDC.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it. For team members signing in to the App Dashboard, see 11-app-dashboard-sign-in.md instead.

---

## SSO-01 · "Why is `email_verified` false in the Clever ID token, and can it be true?"
**Resolvable by CS:** Triage only. PE sets the flag.

**When to use this**
- An SSO developer sees `email_verified: false` in the OIDC ID token.
- Their users can't log in because the app requires a verified email.

**Answer / steps**
> By default Clever returns `email_verified: false`, because Clever doesn't verify the email addresses districts put on user records. We can set a flag for your application so it returns `true`. This is done app by app, so please send the client ID for each app you want changed (production, dev and demo are separate apps). Once it's set, you don't need to change anything else in Clever.

1. Collect the client ID for **every** app they want changed.
2. Before escalating, ask what value they're actually seeing for each app. Some apps already return `true`.

**Escalate if…**
- The flag needs setting on any app.
- The partner says an app that was already flagged still returns `false`.

**Notes / conflicts**
- In one case, some of a partner's apps already returned `true` and another didn't, with no explanation. Don't tell a partner an app is definitely `false`. Have PE check each app.

- PE can set a dev app to return `email_verified` true. It applies to every connected user in every sandbox district on that dev app (02969830). Clever doesn't verify emails, so PE recommends using the Clever ID (02993417).
**Links:** https://dev.clever.com/docs/oauth-oidc-overview
**Source cases:** 02969830, 02993417, 03001696

---

## SSO-02 · "Why does Clever say '<app> does not support Clever SSO for students / teachers / staff / app admins'?"
**Resolvable by CS:** Yes.

**When to use this**
- The partner, or one of their users, gets a Clever error page saying the app "does not support Clever SSO for <role>", even though the app should support that role.

**Answer / steps**
Four causes have come up. Check them in order:
1. **App setting:** in the app dashboard, under **Settings > Integration > SSO Supported Users**, is the role ticked? This is common on new dev apps, where nothing is ticked yet.
2. **"…for app admins":** the developer is testing in the same browser where they're signed in to the app dashboard, so Clever sees an app-admin session. Test in an incognito window or a different browser.
3. **District visibility:** the district has hidden the app from that role in its Portal settings. In one case a district had hidden it from staff until a far-future date. The partner can't see this setting; CS or PE can check it, and the district admin has to change it.
4. **Wrong client ID:** the authorize or "Log in with Clever" URL uses a different app's client ID (a dev app, another product, or an internal Clever app). The app named in the error tells you which one.

Related: if Portal launches work but the partner's "Log in with Clever" button sends users to the Clever login page, the district may not have turned on Clever login for students or teachers. The district admin sets that at https://schools.clever.com/instant-login/settings

> Thanks for reaching out! This message usually means one of a few things:
> - Your app's SSO Supported Users setting (Settings > Integration in your app dashboard) doesn't include this user type.
> - If it says "app admins", you're testing in a browser where you're signed in to your app dashboard. Please try again in an incognito window or a different browser.
> - The district may have hidden your app from this user type in their Clever Portal. We can check that for you.
> - The login link may be using a different app's client ID. Please check the client_id in your authorize URL.
>
> If none of these fit, could you send the Clever ID of an affected user and the full URL from the error page (with any secrets removed)?

**Escalate if…** All four checks pass and the error still happens, or you need someone to look at the district's settings. Transfer to PE.

**Notes / conflicts**
- A partner asked for clearer wording when a district setting causes this error. That was logged as product feedback; don't promise a change.
- Clever doesn't change a partner's production app settings without the partner's permission.
- Turning on student SSO for an existing SSO app: PE saw few risks beyond some end-user confusion. Test with a dev app and a sandbox first (03045789).

- "Does not support teachers" or "app admins" during a test login means no SSO Supported User Types are selected. Set them under Settings > Integration > SSO Supported User Types.
- A new app with no supported user types selected shows an error screen at the first login. Set the types under Settings > Integration.
**Links:** https://dev.clever.com/docs/testing-your-sso-integration · https://dev.clever.com/docs/oauth-implementation
**Source cases:** 02986191, 03079542, 02973516, 03076815, 02981844, 03070740, 02991769, 03077137, 03071100, 03045789, 03014590

---

## SSO-03 · "Why am I getting invalid_grant when I exchange the code at /oauth/tokens?"
**Resolvable by CS:** Yes, for the explanation. PE if scopes need turning on or logs need checking.

**When to use this**
- The token exchange returns `invalid_grant` ("invalid code" or "unknown redirect_uri"), often only sometimes, or only on some devices.

**Answer / steps**
1. **The code was used twice.** Clever authorization codes work once. A second POST always fails: a retry after an error, a handler that fires twice, or on iOS both Safari and the native app handling the same callback. Ask the partner to log every token call along with the code. The fix is "first one wins": ignore a code that's already been used, or capture the redirect in only one place.
2. **"unknown redirect_uri":** the `redirect_uri` sent to `/oauth/tokens` must exactly match the one used at `/authorize` and registered in the dashboard, trailing slash included. Hardcode it rather than building it from the current page URL, which school networks or webviews can change.
3. **OIDC scopes missing on the app**, for example a dev app that gets no `id_token` back. PE can turn on the OIDC scopes.
4. Exchange the code right after the redirect.

> Thanks for the details! invalid_grant almost always means the authorization code was sent to /oauth/tokens more than once (each code works only once), or the redirect_uri in the token request doesn't exactly match the one registered in your dashboard (trailing slashes count). Could you check your logs for duplicate token calls with the same code, and make sure your redirect_uri is a fixed value rather than built from the incoming URL? If it still fails, send us your client ID, a timestamp, and the redirect_uri you're sending (no secrets, please) and we'll take a look.

**Escalate if…** There are no duplicate calls and the redirect_uri matches, or the `id_token` is missing. Transfer to PE.

**Notes / conflicts**
- The dev docs confirm that the iOS double callback shows up as `invalid_grant` (see SSO-05).
- Engineering said Clever puts no limits on the format or content of `state` (03039649).
- If the partner pastes a client secret or token while troubleshooting, immediately notify #partner-engineering to reset the token and redact it from the email thread.
- In one case a token call that was missing the `code` parameter returned `invalid_request`, not `invalid_grant` (02981327). Check the request has the code first.
- "Log in with Clever" needs nothing enabled on the app. Only OIDC scopes need PE to turn on, with the client ID (02998451).
- A token call that failed with "unknown grant type" was caused by a misspelled form content type in one case (02976745). Ask for the exact request (without secrets) and check the grant type, matching credentials, content type header, and that the code isn't reused.

- Intermittent "invalid code" is usually a reused authorization code. A code can be exchanged only once (03004935).
**Links:** https://dev.clever.com/docs/testing-your-sso-integration · https://dev.clever.com/docs/oauth-implementation · https://dev.clever.com/docs/oidc-implementation
**Source cases:** 03004935, 02969208, 02993919, 03058636, 03070137, 03074240, 03039649, 02978515, 03055999, 02981327, 02998451, 02976745

---

## SSO-04 · "Our own login button works, but Portal logins (or one district) fail since we added a state check or PKCE"
**Resolvable by CS:** Yes, for the explanation. PE if Clever-side logs are needed.

**When to use this**
- The partner enforces `state`, added PKCE, or restarts the authorization request in their own code.
- Logins from the Clever Portal, or from one district, fail, while their own "Log in with Clever" button works.

**Answer / steps**
1. Logins that start in Clever (the Portal icon or Instant Login) **don't include `state`**, because Clever never got one from the app. The callback must accept a missing `state`, or the app must restart the authorization request with its own `state`.
2. If the app restarts the request to add `state`, check it uses the **production client ID**, not the dev one. In one case the dev client ID was sent and a whole district failed.
3. **PKCE:** Clever accepts `code_challenge` and `code_verifier`. But when a valid `client_secret` is sent (a confidential client), the secret is what authenticates the request, so a wrong verifier isn't rejected.
4. To skip the school picker on the partner's own button, add `district_id` to the authorize URL.

> Logins that start from the Clever Portal don't include a state parameter, since Clever never received one from your app. Your callback needs to accept a missing state for those launches, or start a new authorization request with your own state. If you do that, please make sure the new request uses your production client ID. Also, because you send a client_secret, Clever treats your app as a confidential client: the secret authenticates the request and the PKCE verifier isn't enforced. Keeping PKCE in your code is fine.

**Escalate if…** You need Clever-side logs or the exact authorize URL a district user hit. Transfer to PE.

**Notes / conflicts**
- If the partner turns out to be a *new* SSO-only integration, don't offer it. New standalone SSO-only integrations aren't supported.
- Most users launch apps from the Clever Portal, so the docs strongly recommend supporting Portal login.

**Links:** https://dev.clever.com/docs/oauth-implementation · https://dev.clever.com/docs/best-practices-edge-cases
**Source cases:** 03072285, 03074243, 02991769, 03039649

---

## SSO-05 · "How do we make Clever open our native app instead of the website?"
**Resolvable by CS:** Yes, for guidance. PE and Engineering if it's a sudden regression.

**When to use this**
- The Portal icon or an LMS launch opens the partner's website in a browser instead of their iOS, Android or Chromebook app.
- The app opens, but the user isn't logged in, or login takes a long time.

**Answer / steps**
- There's **no app dashboard setting** for this. The device decides whether the redirect URI opens the app.
- **iOS:** Universal Links. Host the apple-app-site-association file over HTTPS, add Associated Domains in the app, and have the app read the code from the incoming URL and exchange it itself. Browser cookies aren't shared with the app.
- **Android / Chromebook:** host `assetlinks.json`, and consider a "Launch app" button with a custom URL scheme as a fallback. Managed devices may need the "Open supported links" policy turned on.
- Watch for the double callback, which causes `invalid_grant` (SSO-03).

> There isn't a dashboard setting for this. The device decides whether your redirect URI opens your app. For iOS, set up Universal Links (the apple-app-site-association file plus Associated Domains) and have your app exchange the authorization code itself. For Android and Chromebooks, host assetlinks.json and check that the device policy allows "Open supported links". Details: https://dev.clever.com/docs/il-native-ios and https://dev.clever.com/docs/ios

**Escalate if…** It used to work and broke for many users without any change on the partner's side. In one case this was caused by a Clever update that was then rolled back. Transfer to PE, who will involve Engineering.

**Notes / conflicts** —
- Mobile-first apps: Clever has an iOS app, Android uses the browser, and OAuth works as usual apart from the iOS handling above (02979706).
- Clever's iOS app doesn't accept brokered tokens from a third-party SSO extension and doesn't read Apple Managed App Configuration values. That came from a PE reply after an engineering review (03079510). Send those questions to PE.

**Links:** https://dev.clever.com/docs/il-native-ios · https://dev.clever.com/docs/ios · https://dev.clever.com/docs/testing-your-sso-integration
**Source cases:** 03000562, 03074240, 03058360, 03070371, 02979706, 03079510

---

## SSO-06 · "When users log out of our app they end up on the Clever login page. How does logout work?"
**Resolvable by CS:** Yes.

**When to use this**
- The partner asks about logout, or users are sent to a Clever page after logging out.

**Answer / steps**
> Clever doesn't support single logout. When a user logs out, end the session in your app and send them to a page on your own site rather than back to Clever. To test the full login flow, use your sandbox district's Portal URL (clever.com/in/<sandbox-shortname>) in an incognito window instead of "Log in as…", which skips the password step.

**Escalate if…** The partner needs something beyond this (for example a session problem on shared devices). Transfer to PE.

**Notes / conflicts**
- Certification reviews have flagged apps that don't end the earlier session when a new Clever login arrives on a shared device (CRT-04).

**Links:** https://dev.clever.com/docs/best-practices-edge-cases · https://dev.clever.com/docs/testing-your-sso-integration
**Source cases:** 02990697, 02998782, 02978791

---

## SSO-07 · "Can we have a different redirect URL for each district, or one URL for the Portal and another for our own button?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the explanation. Triage only for duplicate redirect URI errors.

**When to use this**
- The partner wants a district-specific or role-specific redirect URI.
- Portal-initiated logins land on the wrong URL because Clever "always picks the first" URI.
- The dashboard says "Duplicate redirect URI detected" when adding a URL used by another of the partner's apps.
- The partner wants to add an initiate-login URL.

**Answer / steps**
1. There's no per-district setting for redirect URIs. Logins that start in the Clever Portal use the first redirect URI in the app's list, so put the URL that should handle Portal launches at the top.
2. If the partner needs different destinations, make that first URL a routing endpoint on their side. It reads the user and district data it receives and sends the user on to the right site or domain.
3. A single app tile can't send teachers and students to different URLs. Have one endpoint receive everyone, call Clever's `/me` endpoint to find the user, read which roles are listed in the user's `roles` object, then redirect.
4. Every production app needs its own unique redirect URI. This doesn't apply to dev apps.
5. If the dashboard shows "Duplicate redirect URI detected", or the partner needs help with a redirect URI that isn't unique, ask in #partner-engineering. Don't suggest a workaround.

> Thanks for reaching out! Clever uses the first redirect URI in your app's list when a login starts from the Clever Portal, and we can't set a different URI for each district. The usual approach is to put one routing URL at the top of the list that reads the user and district data in the request and sends people on to the right site. If you want different destinations for teachers and students from one app tile, you can send everyone to that same endpoint, call the Clever /me endpoint to find the user, check which roles are listed on the user, and redirect from there. If you're seeing a "Duplicate redirect URI detected" message, let me know and I'll bring in our Partner Engineering team.

**Escalate if…**
- The dashboard shows "Duplicate redirect URI detected", or the partner needs help with a non-unique redirect URI. Ask in #partner-engineering.
- The partner asks for a second app for another organization or product. Check that they're a paying customer, then ask in #partner-engineering. Don't say how many apps they can have.
- The partner wants to use the Clever initiate-login URL. In one case PE confirmed putting it first was fine, but the dashboard rejected it for a while before accepting it.

**Notes / conflicts**
- PE has a workaround for duplicate redirect URIs that isn't shared with partners. Don't suggest one.
- The unique redirect URI requirement applies to production apps, not dev apps.
- Older cases created extra dev apps freely. That's no longer the process: check that the partner is a paying customer, then ask in #partner-engineering.

- "redirect_uri is not valid" usually means the URI isn't listed on the app. Add it under Settings > Integration. The first URI in the list is the one used for Portal logins, so change the order if a dev app's Portal login goes to the wrong place. For two apps with different landing pages, route users from one URI or build two separate app integrations (PE-level, not confirmed).
- For a multi-tenant app with a subdomain per school, PE recommended one general redirect URI and routing users after login (02978060). Redirect URIs can be edited under the app's Settings > Integration (03081955).
**Links:** https://dev.clever.com/docs/oauth-implementation
**Source cases:** 02978060, 03081955, 02970975, 02988644, 02969222, 03004185, 03045189, 02976643, 03000191, 03076701, 03086261, 03088204, 02983219

---

## SSO-08 · "How do we match Clever users to our existing accounts, and how do we tell students from teachers?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- The partner asks how to handle a user who already has an account in their app.
- The partner can't tell whether a new login is a student or staff.
- The partner wants to send a student straight to the right teacher or class.
- The partner asks which ID to use as the primary key.

**Answer / steps**
1. Key users on the Clever ID. The `id` returned at login is a globally unique identifier for every user. An app that keys on email or another field won't be certified until it switches.
2. For existing accounts, most partners match Clever users to existing records and offer to merge. Offering a merge when the email matches is acceptable. If accounts must stay separate, store a flag for which identity provider created each account.
3. To tell students from teachers, look up the user after the OAuth flow (`/users/{id}`, or follow `/me` to the user) and read the `roles` object. There's no single "role" field. The roles listed inside the object (for example `student`, `teacher`, `staff`, `district_admin`) tell you who the user is, and a user can have more than one.
4. For a student in several teachers' classes, don't pass teacher data through `state`. Call `/v3.0/users/{id}/sections` with the student's token. It returns the student's sections with the teacher Clever IDs. Match those against your own records, or show a class picker.
5. In the sandbox, use Support Tools > "Access as Portal" to test as a particular user.

Example of what the `roles` object looks like on a user (values shortened). Here the only role listed is `student`:
```json
{
  "data": {
    "id": "<clever user id>",
    "district": "<clever district id>",
    "name": { "first": "<first>", "last": "<last>" },
    "roles": {
      "student": { "enrollments": null, "schools": [] }
    }
  },
  "links": [ { "rel": "self", "uri": "/v3.0/users/<clever user id>" } ]
}
```

> Great question! Please use the Clever ID (the `id` field) as the primary key for users in your app. It's a globally unique identifier for every Clever user. When someone logs in who already has an account on your side, most partners match on the Clever ID and email and offer the user a chance to merge. If you need to keep them separate, store a flag for which sign-in method created the account. To tell students from teachers, look at which roles are listed on the user after login. If a student belongs to several teachers' classes, you can call the sections endpoint for that student and match on the teacher Clever IDs.

**Escalate if…**
- The partner's app keys on something other than the Clever ID and is heading for certification. That's a blocker. Post in #partner-engineering.
- The partner is stuck at a Portal login that isn't passing the user.

**Notes / conflicts**
- Never describe SSO as free or as its own tier. One case's partner called it "free SSO".
- Older case replies called this a "role field". It's actually a `roles` object with the roles listed inside, so don't say "role field" to a partner.
- PE suggested the sections endpoint instead of passing teacher data in `state`. The partner said they'd do it later, so it's unknown whether it fully solved their case.

**Links:** https://dev.clever.com/docs/users · https://dev.clever.com/docs/oauth-implementation
**Source cases:** 02984087, 03008127, 03051366, 03065406, 02972501

---

## SSO-09 · "What happens when we rotate our client secret or district tokens?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the client secret. Triage only for district tokens, which PE resets.

**When to use this**
- The partner's security team asks about secret rotation.
- The partner asks whether users are affected.
- The partner suspects a token or secret leaked.

**Answer / steps**
1. **Client secret:** app admins can roll their own client secret in the API Credentials section of the Settings page in the app dashboard. It's a section on the Settings page, not its own page. This works for dev and production apps. PE can also roll a secret if the partner can't.
2. **District-app tokens** (the OAuth token in the dashboard) are reset by PE only, for one district or all at once. Collect the client ID (dev and/or production) and the district IDs, post in #partner-engineering, and transfer the case to PE.
3. PE said end users shouldn't see a change as long as the partner's code has been updated with the new values, but that wasn't tested. Don't promise it to the partner.
4. Never ask the partner to send secrets, tokens or authorization headers by email.

> Thanks for asking. Your app admins can roll your client secret any time in the API Credentials section of the Settings page in your app dashboard. If individual district tokens need to be reset, our Partner Engineering team can do that for specific districts or all at once. Please send the client ID and the district IDs. Please don't send the secret or token by email.

**Escalate if…**
- The partner needs a district token reset, or can't roll the secret themselves (for example they aren't an app admin): transfer to PE. An existing admin can change a teammate's permissions on the app dashboard's Team page.
- A secret or token appears in an email: tell #partner-engineering immediately so it can be rotated. Don't wait for the case to be transferred.

**Notes / conflicts**
- "Users see no change" came from PE in one case and wasn't tested, so it's left out of the partner reply.
- Don't send the partner to a separate "API Credentials page". It's a section within Settings.

**Links:** https://dev.clever.com/docs/oauth-implementation
**Source cases:** 03031330

---

## SSO-10 · "Why do I get 'Resource is not available for token' when I call the Data API from my SSO app?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the explanation. Triage only if the app is a Secure Sync app.

**When to use this**
- A District SSO partner calls Data API endpoints such as sections or schools and gets "Resource is not available for token".
- They can call the districts endpoint but nothing else.

**Answer / steps**
1. Confirm the app type. If it's an SSO-only app, this is expected, not a bug.
2. With the app's district token, an SSO app can reach the districts endpoint. To look up a user after login, complete the sign-in flow and use the user's access token.
3. Data like schools, sections and enrollments comes from Clever's rostering integrations, which need Secure Sync scopes and a Clever Complete agreement.

> Thanks for reaching out! This is expected for an SSO-only app. With the app's district token you can call the districts endpoint. To read user details after a login, complete the sign-in flow, get the user's access token, and look the user up with it. Data like schools, sections and enrollments is part of Clever's rostering integrations, which come with a Clever Complete agreement. Here's an overview of what each integration type includes: https://dev.clever.com/docs/integration-types. If your app needs that rostering data, let me know and I'll bring in the right team.

4. If the partner wants rostering data, check whether they're a paying customer. Paying customers go to their ASM. Everyone else goes to application-interest@clever.com.
5. If the partner wants another demo district attached to their dev app, post in #partner-engineering. PE connected it in 02999664.

**Escalate if…**
- The partner says their app is a Secure Sync app and still gets the error. It may be a scope problem for PE.

**Notes / conflicts**
- PE's old wording was "Secure Sync scopes, which require an agreement with Clever". Today new integrations need a Clever Complete agreement, and routing starts with the paying-customer check.

**Links:** https://dev.clever.com/docs/integration-types
**Source cases:** 02999664, 03067943, 03045885, 03010510

---

## SSO-11 · "How do I get OIDC (an id_token) turned on for my app?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A developer says the dev docs told them to contact Clever to have OIDC configured, or their token response has an `access_token` but no `id_token`.
- They often name an auth provider (Firebase, Cognito, Clerk, Supabase, Azure B2C).

**Answer / steps**
1. Get the client ID of the dev app they're testing with (and the app name if there's more than one).
2. If this is a new partner, run the paying-customer check: paying customers go to their ASM, everyone else to application-interest@clever.com. Don't promise anything about production access.
3. Transfer to PE. PE enables the OIDC scopes on the dev app. In the cases this usually happened the same day.
4. After PE enables it, ask the partner to retest and tell you what the token response contains.

> To receive an id_token from the token endpoint, our Partner Engineering team needs to turn on the OIDC scopes for your app. Could you send the client ID of your dev app (and the app name, if you have more than one)? I'll pass it to them right away.

**Escalate if…**
- OIDC was enabled but there's still no `id_token`. In one case the first "enabled" didn't work and the scopes were reset afterward. The final outcome wasn't recorded (02969208, 02993919).
- The partner says SSO is all they want. Don't describe a standalone option. Run the paying-customer check and route.
- They ask about certification: post in #partner-engineering and don't give detail answers.

**Notes / conflicts**
- Older cases enabled OIDC with no commercial check, and one older email said an SSO integration had no fee. That's no longer the process: new SSO comes with Clever Complete, and PE now asks about it.
- Application Partnerships told several partners that district SSO needs a signed agreement to go live. That's fine to relay as the direction without adding detail.

- OIDC today is only for logging in to apps with Clever. Using a partner's SIS as the identity provider into Clever isn't supported. Say "not supported today, thanks for the feedback" and log the feedback in Salesforce (03004197).
**Links:** https://dev.clever.com/docs/oidc-implementation · https://dev.clever.com/docs/example-oauth-walkthrough
**Source cases:** 02969041, 02981411, 03012621, 02991207, 03004197, 03061491, 02967380, 02969208, 03048334, 03058636, 02991266, 03066996, 02993417, 03070137, 02993919, 03068702, 03073263, 03003713, 03004263, 03004910, 03006758, 03007053

---

## SSO-12 · "We use Firebase / Cognito / Clerk / Supabase / Azure B2C. Does Clever work with it?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the general answer, triage only for provider-specific setup

**When to use this**
- A developer asks whether Clever works with a hosted authentication service.

**Answer / steps**
1. PE said partners have used Firebase for their SSO integration and that Firebase typically requires OIDC. Other providers in the cases were handled by enabling OIDC scopes on the dev app (SSO-11). PE didn't comment on provider-specific setups.
2. Collect the client ID and follow SSO-11.
3. For a custom login design (for example exchanging the code, calling `/v3.0/me`, then matching the user to an existing account), don't approve or reject it. Send it to PE.

> Partners have used Firebase to build their SSO integration with Clever, and Firebase typically requires OIDC. To get started we need to turn on OIDC scopes for your dev app. Could you send the client ID? Our Partner Engineering team can look at anything specific to your setup.

**Escalate if…**
- The partner wants sign-off on a custom login design, or asks about chaining another identity provider (answer not seen).

**Notes / conflicts**
- Library apps are different. See LIB-02.
- Don't call SSO free, and check paying-customer status for new partners.

**Links:** https://dev.clever.com/docs/oidc-implementation
**Source cases:** 03004910, 03004263, 03061491, 03070137, 03068702, 03073263, 03003713, 02967380, 02969208

---

## SSO-13 · "What's the difference between `user_id` and `multi_role_user_id` in /userinfo?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner sees both fields in the `/userinfo` response and asks which to store.

**Answer / steps**
1. `multi_role_user_id` exists only for users who have more than one role in Clever, so it isn't on every user.
2. PE recommends keying on the Clever user ID, which every user has and which is globally unique. Using the Clever ID as the primary key is also required for certification.

> Not every user has a `multi_role_user_id`. It's only present for users who have more than one role in Clever. We strongly recommend using the Clever user ID as your identifier, since every user has it and it's globally unique. If you'd like us to look at a specific example, send the district and user so our Partner Engineering team can check.

**Escalate if…**
- The partner needs to map a role-level legacy ID to the main ID for a specific user, or something looks inconsistent.

**Notes / conflicts**
- PE's answer didn't spell out which ID each field maps to. Don't say more than this.

**Links:** none confirmed
**Source cases:** 02997959
