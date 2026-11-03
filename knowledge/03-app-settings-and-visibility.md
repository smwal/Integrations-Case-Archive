# App settings, visibility & district connections

Covers these Salesforce buckets: App Settings/Config > App Icon, and app-visibility requests from Scopes and Tags. Team members signing in to the App Dashboard are covered in 11-app-dashboard-sign-in.md.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it.

---

## APP-01 · "How do I hide my app from districts or stop new district connections?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE makes the change.

**When to use this**
- A partner wants an app hidden from App Gallery search, for example hiding an SSO app but keeping the rostering app findable.
- A partner is shutting down an integration and wants no new connections.
- A partner wants districts to stop requesting a feature such as LMS Connect, or wants a dashboard alert removed.

**Answer / steps**
1. Confirm which app(s) and which of these they want:
   - **Hidden from search** (existing connections untouched): PE changes the app's visibility.
   - **"Invite only"**: districts can't see the app to request it, but the partner can still invite districts from their dashboard.
   - **Shutting down**: PE removes the app from the App Gallery so districts can't add it.
2. Set the timing:
   > Removal from the App Gallery may not show until after the overnight sync. If you still see the app tomorrow, let us know and we'll look into it.
3. Shutting down: existing districts have to be **disconnected one at a time**. There's no disconnect-all.
4. LMS Connect: "invite only" applies to district-app connection requests, **not** to LMS Connect. Removing a dashboard alert is an Engineering request.

**Escalate if…**
- Any visibility, invite-only or remove-from-gallery change is needed. PE made every one of these.
- The app still shows after the overnight sync. PE needs to force a resync.
- A dashboard notification needs removing. Send that to dashboard Engineering.

**Notes / conflicts**
- In one case, PE first offered "invite only" for an LMS Connect request, then corrected that it doesn't apply. Before offering anything, confirm whether the request is about district connections or LMS Connect.
- No help article on app visibility was found.

- A hidden app can still receive a request through its direct add-application URL (see APP-03). PE can also make an app non-searchable and non-requestable for all but one invited district. That needs PE and isn't offered proactively.
- A dev app listed in the public App Gallery by mistake is a PE follow-up. The production app is visible in the district dashboard and searchable (03082752).
**Source cases:** 03082752, 03054351, 02980147, 03085062, 03084129, 03073948, 03038841, 03051820, 03067801, 03032100

---

## APP-02 · "Are these districts already on Clever, and how do they add our app?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner wants to know whether districts they're talking to already use Clever.
- A partner asks how a district finds or adds their app.

**Answer / steps**
1. The partner can search for schools and districts in the App Dashboard. PE called this the fastest way to see whether a district is already on Clever.
2. If the district isn't on Clever, it can contact Clever Technical Support to get set up.
3. Once a district is on Clever, a district admin adds the app to the district's dashboard and uses sharing rules to choose who sees it. Users then open it from their Clever Portal.
4. If the partner is a prospect or new, run the paying-customer check: paying customers go to their ASM, everyone else to application-interest@clever.com.

> Thanks for reaching out! The quickest way to check is to search for the schools and districts in your App Dashboard. If they show up, they already have a Clever instance. If they don't, they can contact our Technical Support team to get set up. Once a district is on Clever, a district admin adds your app to their dashboard and uses sharing rules to choose who can see it.

**Escalate if…**
- The partner asks about a teacher-level app that skips district admin approval. PE's answer: if it's a Library app, yes, and districts have Library controls where district admins can block specific apps. Any Library question goes to #partner-engineering, so post there rather than answering in detail. For a non-Library app, ask PE.

**Notes / conflicts**
- If the partner shares the district list as a document link, PE couldn't open it once. Ask for the list as plain text in the email.
- The "district admin adds the app" answer applies to apps shared by sharing rules. Library apps work differently (see Escalate).

- There's no way to message a district requester from a pending invite. The requester's email is visible on the invite. Log it as feedback (02967820).
**Links:** none confirmed
**Source cases:** 02967820, 02976961, 03009813

---

## APP-03 · "Our app should be invite-only, but a district still sent us a request"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner has an invite-only (non-searchable) app and a district still sent a connection request.
- A partner thinks their dev app shows up in district search.

**Answer / steps**
1. Get the exact app, its client ID, and whether it's the dev or production app. In one case the partner's organization had several apps owned by different teams, and PE first changed the wrong one.
2. Ask PE to confirm the app's searchable setting. Any visibility change is made by PE.
3. A non-searchable app can still receive a request if a district opens the app's direct "add application" page URL.
4. The partner can decline any connection request from a district it didn't invite.
5. Dev apps aren't searchable by districts. After a sandbox is disconnected, allow some time for the change to show.

> Thanks for flagging this! A non-searchable app is hidden from search in the district dashboard, but a district that opens your app's direct add-application link can still send you a connection request. You can decline any request from a district you haven't invited. I've asked our Partner Engineering team to confirm your app's settings. Could you confirm which app this is and whether it's your dev or production version?

**Escalate if…**
- Any visibility change is needed, or it's unclear which of several apps needs it.

**Notes / conflicts**
- A district can't see an invitation that was previously rejected. The invite has to be reset by CS or Application Success (PE confirmed). Internal reference for the agent only, not for the partner: Guru card "App Invites", https://app.getguru.com/card/aczqLRzc/App-Invites
- See APP-01 for hiding an app. This entry covers why a request still arrived.
- In 02978346, PE checked that an app was already findable by searching from a production account before changing anything.

**Links:** none confirmed
**Source cases:** 03054351, 02978346

---

## APP-04 · "What does 'Licenses Exceeded' mean on a district connection?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner sees "Licenses Exceeded" next to a connected district in the App Dashboard.

**Answer / steps**
1. It's a notification, not an error. The district is sharing more schools than the license limit the partner set for that district. A warning shows in both the app and district dashboards, but no data is blocked and sharing keeps working.
2. The partner can change the limit for a district from the dashboard, using the help article.
3. Suggest they check which schools are being shared. In the case, the partner found the extras were an auto-generated school and one holding graduated students (the partner's own finding).

> "Licenses Exceeded" is just a notification. It appears when a district shares more schools with you than the license limit you set for that district. For example, a district sharing 4 schools with a limit of 2. You can change the limit for each district from your dashboard: https://support.clever.com/hc/s/articles/115003921667

**Escalate if…**
- The partner asks what limits cost or how they affect billing. Route to their ASM and never quote fees.

**Notes / conflicts**
- A warning shows in both the app and district dashboards when licenses are exceeded. No data is blocked, and data sharing keeps working (confirmed by PE).

**Links:** https://support.clever.com/hc/s/articles/115003921667
**Source cases:** 03074017

---

## APP-05 · "How do I change my app's icon or logo?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes (CS has the permissions to update it)

**When to use this**
- A partner asks to update, swap or fix an app's icon or logo, including rebrands and requests from a reseller on behalf of a customer's app.
- A partner says the icon looks small, off-center, squished or blurry in the Portal, or can't find where to submit a request.

**Answer / steps**
1. Get the app name or client ID, which app it is (dev or production), and whether it's the icon or the logo.
2. Check the image against the requirements. Clever only accepts PNG files for the app icon and logo. Icon 200x200 pixels, logo smaller than 500x100 pixels. Icons can sit on a white background, should fill the whole square, and shouldn't have rounded corners.
3. If the file doesn't fit (for example a tall or wide image sent as an icon), ask for a square 200x200 PNG. PE said a non-square image looks squished.
4. CS makes the change in Hall Monitor: open the app, select the Advanced tab, and scroll down to Icon. The logo can be updated right under the icon. PE updates the icon and logo when an app is certified and occasionally handles a one-off, but a request that reaches CS should be done by CS, not transferred. In the older cases PE made the change, usually within minutes to hours.

> Thanks for reaching out! We can update your app icon and logo. Please reply with the app name (or client ID) and the image files. Icons need to be a 200x200 pixel PNG and logos need to be a PNG smaller than 500x100 pixels. Icons can be on a white background, should fill the whole square, and shouldn't have rounded corners. Once we have the files, we'll update the app and let you know when it's done.

**Escalate if…**
- The updated icon still looks wrong after a refresh, or the PNG may be corrupted.
- The partner asks to use the icon in marketing materials. The case never answered that.

**Notes / conflicts**
- Only PNG is accepted for the icon and logo (PE confirmed). Older certification guidance that said "SVG or PNG" is outdated.
- A "logo" (under 500x100, wide) and an "icon" (200x200, square) are different files. Partners sometimes confuse them.
- The dev app keeps its "(DEV)" name and placeholder logo on purpose so it isn't confused with the production app. The real name and logo are applied to the production app created after certification. Don't promise to change the dev app (02998879).
- A Library marketing-collateral page is a Library item. Route it to #partner-engineering (02984268).

**Links:** none confirmed
**Source cases:** 02966962, 03058194, 03067876, 03079532, 03016268, 03016962, 03100150, 03028785, 03110315, 03105652, 02984268, 02998879

---

## APP-06 · "The icon is right in our account but wrong, old or missing in the app gallery"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for icon updates, ask PE if the gallery listing itself is wrong

**When to use this**
- A partner updated the icon but the public gallery still shows the old one, or a newly certified app isn't in the gallery yet or shows the wrong logo.

**Answer / steps**
1. Confirm the app name and which listing they mean (the public gallery or what districts see). The gallery matches what district admins see.
2. Update the icon in Hall Monitor as in APP-05 (app, Advanced tab, Icon) if it's an icon change. In the case seen, PE updated the gallery icon and the partner confirmed the same day. If it's still wrong after the update and a refresh, ask in #partner-engineering.
3. For a newly certified app that isn't showing, a gallery refresh lag was the explanation in one case. Ask them to reload and search again. Don't give a timeframe.

> We're updating the icon in the app gallery and will confirm here once it's live. If the app was just added, reloading the gallery page and searching again can help.

**Escalate if…**
- The partner has a launch or district deadline (flag urgency).
- Nothing changes after the update and a refresh.
- The partner asks about certification status. Post in #partner-engineering with no detail answers.

**Notes / conflicts**
- In 03091863 an ASM first said the app wasn't certified until a form was submitted, then said it was certified and it was a refresh issue. Older cases sent forms like that, but that's no longer the process. Don't send or mention a form.
- In 03087354 the partner was told by the phone line to "wait for the ticket". Don't leave them waiting.

**Links:** none confirmed
**Source cases:** 03087354, 03091863

---

## APP-07 · "I'm trying to invite a district but it says 'Unable to send Invite', and the district isn't on our District Overview"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes (CS resets it; don't transfer to PE)

**When to use this**
- A partner can find a district in search and see its data in the Data Browser, but the District Overview is empty and the invite fails with "Unable to send Invite. Please try again", or says the district is "already using" the app.

**Answer / steps**
1. The usual cause is that the district previously rejected the invite. CS is the only team with permissions to reset it, so don't transfer the case to PE. Application Success can also reset it.
2. Reset the invite. The Guru card "App Invites" has the steps: https://app.getguru.com/card/aczqLRzc/App-Invites (internal, for the agent only, not for the partner).
3. Ask for the app's client ID, the district ID and a screenshot of the error. Check the connection timelines in Hall Monitor too, in case the district is attached to a different app.

> I've reset the invite for this district, so you should be able to send it again now. Please try again and let us know if you still see the error.

**Escalate if…**
- The invite wasn't rejected and the district is attached to a different app. In one case the district had been switched to another app and engineering moved it back.

**Notes / conflicts**
- See APP-03 for a partner who wonders whether a district saw or rejected an invite.

**Links:** none confirmed
**Source cases:** 03080856

---

## APP-08 · "Can you delete or remove an old or duplicate app so districts stop installing it?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner has an old or duplicate app and wants it deleted or hidden, or users are installing the wrong app.

**Answer / steps**
1. Get the app's client ID and confirm with the partner that no connected district still uses it.
2. If the partner says an app exists that PE can't find, ask for a screenshot of where users see it. In one case PE found only a Secure Sync app and no SSO app.
3. Transfer to PE with the app ID and the confirmation that no districts use it.

> I can send this to our Partner Engineering team. Could you confirm the app's client ID, and that none of your connected districts are still using it? If you can, please share a screenshot of where users are finding it.

**Escalate if…**
- Districts are still connected, or the partner only wants it hidden (see APP-01).

**Notes / conflicts**
- In one case a CSM confirmed no districts used the app and PE deleted it. The other case's outcome wasn't shown.

**Links:** none confirmed
**Source cases:** 02988131, 03020072

---

## APP-10 · "We're renaming or moving our app, or the person who set up our Clever account left. How do we update it?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- An existing partner asks to rename the app, change the login or redirect domain, remove a listing, or update the primary contact or links after a company change, and wants to keep districts connected.

**Answer / steps**
1. Collect the app names or client IDs, the new name or URL, which listings to remove, and the new primary contact.
2. If a Library app is involved, post in #partner-engineering and promise nothing.
3. Otherwise transfer to PE, and loop in the ASM for paying partners.
4. Acknowledge quickly. In both cases the partner chased because the only reply was "I'll look into it".

> I'm sending this to our Partner Engineering team. Could you confirm the app names or client IDs, the new name and login URL, and the new primary contact?

**Escalate if…**
- A Library app is involved, or the previous contact can't be reached to verify ownership.

**Notes / conflicts**
- A rename or login-domain change doesn't need re-certification (PE confirmed). Neither case showed what happened to connected districts, so don't guess. For an icon or logo only, see APP-05.

**Links:** none confirmed
**Source cases:** 03012577, 03114112
