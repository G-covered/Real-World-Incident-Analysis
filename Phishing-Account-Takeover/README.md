# Real-World Phishing & Account Takeover Incident Analysis

## Overview

On September 3, 2026, I experienced and investigated a real-world phishing attack that resulted in unauthorized access to my Google account.

What initially appeared to be a legitimate wedding invitation was actually part of a phishing attack originating from a friend's compromised account. The attack used social engineering and a fraudulent Google login page to capture my credentials and convince me to approve a legitimate Google authentication request.

Google's activity records later provided additional evidence of the attack. At approximately 11:57 AM, Google My Activity recorded a visit to a suspicious domain displaying a page titled **"Sign in with your Google Account."** At approximately the same time, Google's security activity reported a new sign-in from an unfamiliar **Mac OS device**.

The incident required a two-stage response. I initially changed my Google password after detecting the unauthorized sign-in. Approximately five hours later, I discovered that the password had been changed again by the unauthorized Mac OS activity, requiring me to recover the account and investigate the compromise further.

After recovering the account, I created a completely different password, enabled 2-Step Verification, reviewed devices and active sessions, and audited the account's security settings.

This report documents the attack, evidence collected during the investigation, incident response, and lessons learned.

---

# 1. Initial Attack

The attack began with an email that appeared to come from a friend.

The message looked like a legitimate wedding invitation and contained a link to a real wedding website. The site eventually prompted me to log into my Google account.

At the time, there was no obvious indication that the message was malicious.

### Why the Message Appeared Legitimate

- It came from a known contact.
- It directed me to a real wedding website.
- The message appeared to be a normal wedding invitation.
- The requested Google login seemed reasonable for accessing the invitation.
- The subsequent Google authentication prompt appeared legitimate because it was delivered through Google's normal authentication system.

After the incident, I contacted my friend and discovered that they had **not actually sent the message**.

This indicated that their account had likely been compromised and subsequently used to distribute the phishing message to their contacts.

---

# 2. Credential Phishing

After following the link, I was immediately presented with a Google login page that appeared legitimate.

I entered my Google email address and password into the page. The page then displayed a number and prompted me to continue the authentication process.

At the same time, my phone displayed a legitimate Google authentication prompt asking if I was trying to sign in. Because I was actively attempting to log in, the request appeared legitimate. My phone instructed me to confirm which of the three numbers displayed on the phone matched the number shown during the login process.

I confirmed the authentication request.

Immediately afterward, the website prompted me to log in again. This confused me because I had just completed the authentication process.

Shortly afterward, I received an email from Google reporting a new sign-in from a **Mac OS device**. I did not own or use a Mac, which indicated that the authentication request I had just approved may have been associated with an unauthorized device.

### Evidence from Google Activity

Google My Activity later provided additional evidence of the phishing interaction.

<img width="592" height="487" alt="Screenshot 2026-09-04 201758" src="https://github.com/user-attachments/assets/80c7f39d-c314-4183-8a09-952cb0e62755" />

At approximately **11:57 AM**, Google recorded a Chrome visit from an **Unknown Device** to the domain:

`yellowewte078.es`

The activity was titled **"Sign in with your Google Account."**

This was significant because the activity occurred at approximately the same time as the unauthorized Mac OS sign-in reported by Google Security Activity.

The domain was not a Google domain, despite the page presenting itself as a Google sign-in page.

### Important Observation

The attacker did not necessarily need to "crack" my password.

The password I entered was strong and difficult to guess.

Instead, the attack relied on **social engineering** to convince me to voluntarily enter my credentials into a fraudulent login page and approve what appeared to be a legitimate Google authentication request.

The authentication prompt was particularly convincing because I was actually attempting to sign in at the time. From my perspective, the request appeared to be confirming my own login.

The sequence of events is consistent with the possibility that the credentials entered into the phishing page were relayed to a legitimate Google authentication attempt initiated by the attacker.

This type of technique can be associated with **adversary-in-the-middle (AiTM)** attacks or other forms of authentication relaying. However, the available Google activity records do not provide enough information to definitively determine the exact technical method used.

This demonstrated an important security principle:

> Password strength cannot protect an account if credentials are voluntarily provided to an attacker.

---

# 3. Detection

Immediately after completing the login process, I received a Google security notification indicating that a new device had signed into my account.

The device was identified as:

- Operating System: **Mac OS**
- Browser: **Google Chrome**
- Device: **Unknown / not owned by me**

I do not own or use a Mac.

This immediately raised suspicion that the login I had just completed was not legitimate.

I began reviewing Google's security activity.

---

# 4. Security Timeline

The Google security activity provided a useful timeline of the incident.

<img width="892" height="580" alt="image" src="https://github.com/user-attachments/assets/4a7af683-6d20-4afa-946e-d659a1d833ed" />

The most significant event was the password change attributed to the Mac OS session.

I had changed my password right after the new sign-in from Mac OS, but several hours later Google reported another password change from the unfamiliar Mac OS session.

---

# 5. How the Attack Likely Worked

Based on the sequence of events, the attack appears to have involved phishing, credential theft, social engineering, and potentially authentication relaying.

The exact technical mechanism cannot be confirmed from the available Google security logs. However, the sequence of events is consistent with an **Adversary-in-the-Middle (AiTM)** attack.

The likely attack flow was:


```text
 Compromised Friend's Account
             |
             v
     Phishing Email Sent
             |
             v
Legitimate-looking wedding invitation
             |
             v
    

            YOU                         PHISHING SITE                 ATTACKER
             │                                │                           │
             │ Enter Gmail + password         │                           │
             ├──────────────────────────────► │                           │
             │                                │──── credentials ─────────►│
             │                                │                           │
             │                                │◄── Login attempt ─────────│
             │                                │                           │
             │◄──── Google verification ──────│◄──── Google prompt ───────│
             │                                │                           │
             │ "Are you signing in?"          │                           │
             │ YES                            │                           │
             │                                │                           │
             │──── Approve ──────────────────►│───── Authentication ─────►│
             │                                │                           │
             │                                │                           │
             │                         ATTACKER LOGGED IN                 │
             │                         Mac OS session                     │
                                              |
                                              v
                                    Google account takeover
```


Step-by-Step Explanation
### 1. Phishing Link

The email appeared to come from a trusted contact and presented a believable reason to follow the link.

When I opened the link, I was immediately presented with what appeared to be a Google login page.

### 2. Credential Entry

I entered my Gmail address and password into the page.

The credentials may have then been relayed to the attacker or their authentication infrastructure.

### 3. Attacker Initiates a Google Login

The attacker appears to have used the credentials to initiate a legitimate Google login from their own device.

This is consistent with the Mac OS activity that appeared in Google's security history.

### 4. Legitimate Google Authentication Prompt

Google sent an authentication request to my phone.

My phone asked whether I was attempting to sign in.

Because I was actively trying to log in at that exact moment, the authentication request appeared legitimate.

The prompt also displayed a number that matched the number shown during the login process.

### 5. Authentication Was Approved

I confirmed the authentication request on my phone.

From my perspective, I was confirming the login that I had just initiated.

However, the authentication may have actually been approving the attacker's Google login session.

### 6. Mac OS Session Appeared

Immediately after approving the authentication request, I received an email reporting a new sign-in from Mac OS.

This was unexpected because I had been using my phone and did not own or use a Mac.

This provided an important clue that the authentication I had just approved may have been associated with another device.

### 7. Phishing Page Requested Another Login

After the authentication was approved, the website prompted me to log in again.

This was confusing because I had just completed the login process.

The second login request is consistent with the possibility that the phishing page was not actually completing a normal login for me, but was instead being used to collect or relay authentication information.

### Why the Attack Happened So Quickly?

The attacker would not necessarily have needed to manually type each character into their own computer.

If the phishing page was acting as an intermediary, information entered by the victim could potentially be transmitted electronically to the attacker's authentication process almost immediately.

The sequence could therefore occur within seconds:

```text
Credentials Entered
        ↓
Credentials Relayed
        ↓
Attacker Initiates Login
        ↓
Google Sends Authentication Prompt
        ↓
Victim Approves Prompt
        ↓
Attacker's Session Authenticated
        ↓
Mac OS Sign-In Appears
```

This explains why the attack felt as though I had entered the credentials directly into the attacker's device.

Technically, I was not necessarily controlling or typing into the attacker's computer. Instead, the information entered into the phishing page may have been transmitted to the attacker's authentication process.

### Important Limitation

The exact technical mechanism cannot be confirmed solely from Google's security logs.

The evidence strongly supports phishing, credential theft, social engineering, and account takeover.

The sequence is also consistent with an Adversary-in-the-Middle (AiTM) or authentication-relaying attack, but this should be treated as a likely possibility rather than a confirmed technique.

---

# 6. Social Engineering Techniques

Several social engineering techniques were combined to make the attack convincing.

## Trusted Sender

The phishing email originated from a friend's compromised account.

Because the sender was already known and trusted, the message appeared more credible.

## Familiar Context

The email was presented as a wedding invitation.

This provided a believable reason for the recipient to click the link and authenticate.

## Credential Phishing

The fake login page was designed to resemble a legitimate Google authentication page.

This encouraged the victim to enter their credentials.

## Authentication Prompt Manipulation

The attacker also used a verification step involving three-number confirmation.

The legitimate Google prompt appeared to confirm the login the victim was already attempting.

This demonstrates that multi-factor authentication can still be targeted through social engineering.

---

# 7. Why the Attack Was Effective

The attack was effective because it combined multiple layers of deception:

```text
Trusted Contact
      +
Believable Situation
      +
Convincing Login Page
      +
Legitimate Authentication Flow
      +
Social Engineering
      =
Account Compromise
```

The attack did not depend on guessing a weak password.

Instead, it exploited human trust and familiarity.

This was an important lesson because it demonstrated that even a strong password can be compromised through phishing.

---

# 8. Incident Response

The incident response occurred in two separate stages.

The first response happened immediately after the initial unauthorized Mac OS sign-in was detected. The second response occurred several hours later after I discovered that the attacker had changed the password.

## Stage 1 — Initial Response

### Step 1 — Detected the Unauthorized Sign-In

Google reported a new sign-in from a **Mac OS device** that I did not recognize.

I do not own or use a Mac, so this immediately indicated that someone else may have accessed the account.

### Step 2 — Changed the Password

Immediately after discovering the unauthorized Mac OS sign-in, I changed my Google account password from my trusted device.

The key here is that I reuse part of the password and combined with another harder password. The other password was practically unguessable, unless the attacker was able to view my password manager see what I used for accounts.

At this point, I believed changing the password would secure the account and prevent the unauthorized session from continuing.

---

## Stage 2 — Discovered the Account Was Still Compromised

Approximately five hours later, I discovered that the attacker had changed the password.

### Step 3 — Unexpected Sign-In Prompt

While I was already using my Windows computer and had recently signed into my Google account, I received another prompt to sign in.

This was unusual because I had already been signed in moments earlier.

I attempted to sign in using the new password I had created approximately five hours earlier.

The password was no longer accepted.

### Step 4 — Investigated Recent Security Activity

Because I could no longer sign in with the password I had created earlier, I checked Google's recent security activity from my phone.

The security activity showed that the **same unfamiliar Mac OS device had changed the Google account password** approximately five hours after my initial password change.

The attacker had somehow retained or regained sufficient access to change the password.

### Step 5 — Enabled 2-Step Verification

After discovering that the password had been changed by the unauthorized Mac OS session, I enabled Google 2-Step Verification from my trusted device.

This added an additional authentication requirement to future sign-ins.

### Step 6 — Account Recovery

I used Google's account recovery process to regain control of the account.

### Step 7 — Created a Completely Different Password

After recovering the account, I created a completely different password.

The new password was not based on the previous password and was not reused from another account.

### Step 8 — Reviewed Devices and Sessions

I reviewed the devices and sessions associated with the Google account and identified the unfamiliar Mac OS activity.

### Step 9 — Audited Security Settings

I reviewed:

- Recovery email
- Recovery phone
- 2-Step Verification methods
- Passkeys
- Connected applications
- Active sessions
- Other account security settings

No unauthorized recovery methods or other suspicious security settings were identified after securing the account.

---

## Incident Response Timeline

```text
Initial Mac OS Sign-In
        |
        v
Detected Unauthorized Activity
        |
        v
Changed Password
        |
        |  ~5 hours later
        v
Unexpected Sign-In Prompt
        |
        v
New Password No Longer Worked
        |
        v
Checked Recent Security Activity
        |
        v
Discovered Mac OS Changed Password
        |
        v
Enabled 2-Step Verification
        |
        v
Account Recovery
        |
        v
Created Completely Different Password
        |
        v
Reviewed Devices & Sessions
        |
        v
Audited Security Settings
        |
        v
Account Secured
```

This two-stage response was an important part of the incident because the initial password change did not completely resolve the compromise. 

The later security investigation revealed that the attacker had somehow retained or regained sufficient access to change the password. The available logs do not establish whether the original session remained active, another session existed, or another authentication mechanism was used.

The incident demonstrated the importance of not only changing a compromised password, but also reviewing recent security activity, active sessions, authentication methods, and account recovery settings after an account takeover.

---

# 9. Password Manager Considerations

Another concern was that passwords were stored in Google Password Manager.

Because the Google account had been compromised, I considered whether stored credentials could potentially have been exposed.

This highlighted the importance of:

- Using unique passwords
- Avoiding password reuse
- Protecting password managers
- Using multi-factor authentication
- Securing account recovery methods

Important accounts were prioritized for additional protection.

The incident did not establish that the attacker accessed or copied the entire password manager. However, because unauthorized account access had occurred, stored credentials were treated as a potential risk and reviewed accordingly.

---

# 10. Why a Strong Password Was Not Enough

One of the most important lessons from this incident was that the attacker did not necessarily need to guess my password.

The original password was strong and difficult to guess.

However:

Strong Password
      +
Credential Phishing
      =
Potential Account Compromise

This demonstrates the difference between password security and authentication security.

A password can be extremely difficult to brute-force while still being vulnerable to phishing.

The objective is therefore not only to create a strong password, but also to prevent the password from being disclosed to an attacker.

---

# 11. Lessons Learned
### Lesson 1 — Verify the Login Destination

A login page that looks legitimate is not necessarily legitimate.

Before entering credentials, verify the website's domain and consider navigating directly to the legitimate service instead of following an unexpected authentication link.

### Lesson 2 — Trusted Contacts Can Become Attack Vectors

A message from a known person should not automatically be considered safe.

If a friend's account is compromised, their account can be used to distribute malicious messages to people who trust them.

### Lesson 3 — MFA Does Not Eliminate Social Engineering

Multi-factor authentication provides significant protection, but users can still be manipulated into approving authentication requests.

MFA should therefore be combined with security awareness.

### Lesson 4 — Password Reuse Creates Additional Risk

If the same or similar password is used across multiple services, compromise of one account can increase the risk to other accounts.

Unique passwords should be used for important accounts.

### Lesson 5 — Account Recovery Is Part of Security

Recovery email addresses, recovery phone numbers, passkeys, and other recovery mechanisms should be treated as security-critical components of an account.

An attacker who compromises a recovery method may be able to regain access even after a password change.

---

# 12. What I Would Do Differently

If I encountered a similar email again, I would:

- Contact the supposed sender through another communication method if the message seems unusual or unexpected.
- Not assume that a legitimate website makes the entire message or login process legitimate.
- Verify that the invitation actually exists before following links or entering credentials.
- Check the domain carefully before entering any credentials.
- Be more cautious with unexpected authentication prompts, even when I am actively attempting to sign in.
- Remember that attackers can use legitimate authentication processes as part of a social engineering attack.

---

# 13. Cybersecurity Concepts Demonstrated

This incident provided practical exposure to several cybersecurity concepts across the attack lifecycle, from initial social engineering through account recovery and incident response.

## Attack & Social Engineering

- **Phishing** — A fraudulent message and login page were used to obtain account credentials.
- **Social Engineering** — The attack relied on trust in a known contact and the believable context of a wedding invitation.
- **Credential Theft** — My Google credentials were captured after being entered into the fraudulent login page.
- **MFA Social Engineering** — A legitimate Google authentication prompt was presented during the phishing process, and I was convinced to approve it.
- **Authentication Relaying** — The sequence of events was consistent with the possibility that credentials entered into the phishing page were relayed to a legitimate authentication attempt initiated by the attacker.

## Account & Authentication Security

- **Account Takeover** — The attacker gained unauthorized access to my Google account.
- **Multi-Factor Authentication (MFA)** — Google used an additional authentication factor during the login process.
- **Session Security** — The incident demonstrated the importance of reviewing active sessions and unfamiliar devices after an account compromise.
- **Account Recovery** — Account recovery mechanisms were necessary after the attacker changed the password.
- **Defense in Depth** — Passwords, MFA, device monitoring, account recovery, and security auditing all contribute different layers of protection.

## Credential & Password Security

- **Password Security** — A strong password can still be compromised if it is voluntarily entered into a phishing site.
- **Password Managers** — The incident demonstrated why credentials stored within a compromised account should be considered during incident response.
- **Credential Reuse** — A compromised password can create additional risk if the same password is used on other accounts.

## Detection & Incident Response

- **Incident Detection** — An unfamiliar Mac OS sign-in provided the first indication that the account had been compromised.
- **Security Monitoring** — Google Security Activity and My Activity provided evidence that helped reconstruct the timeline.
- **Security Auditing** — Devices, sessions, recovery methods, authentication methods, and connected applications were reviewed after the compromise.
- **Incident Response** — The account required multiple response actions, including password changes, account recovery, enabling 2-Step Verification, and reviewing account security.

## Key Takeaway

This incident demonstrated that cybersecurity is not dependent on a single security control.

A strong password did not prevent the initial credential theft, and the presence of MFA did not prevent the attacker from obtaining unauthorized access after the authentication request was socially engineered.

Effective account security requires **multiple layers of defense**, combined with the ability to detect suspicious activity and respond quickly when those defenses are compromised.

---

# 14. Same-Day Incident Response (9/3/2026)

One of the most valuable aspects of this experience was being able to detect, investigate, contain, and recover from a real account compromise within the same day.

Rather than treating the incident as a single event, I approached it as an incident-response process.

## Phase 1 — Detection [11:57 AM]

The first indication of compromise was an unexpected Google sign-in from a **Mac OS device**.

Because I did not own or use a Mac, I immediately treated the activity as suspicious.

- Identified the unfamiliar Mac OS sign-in.
- Reviewed Google's security activity.
- Compared the sign-in time with the events that had just occurred.
- Recognized that the activity occurred immediately after the phishing interaction.

## Phase 2 — Initial Containment [12:07 PM]

My first objective was to prevent further unauthorized access.

- Changed my Google account password from my trusted device.
- Used a new password rather than continuing to use the compromised credentials.
- Monitored the account for additional suspicious activity.

At this point, I believed the account had been secured. However, the later events demonstrated that the initial response had not completely resolved the compromise.

## Phase 3 — Reassessment [5:20 PM]

Approximately five hours later, I received another unexpected sign-in prompt and discovered that the password I had previously created was no longer accepted.

I treated this as a new indication of compromise rather than assuming the password had simply been entered incorrectly.

- Checked Google's security activity from my phone.
- Discovered another password change from the unfamiliar Mac OS activity.
- Noticed that the Windows session had also been signed out.
- Re-evaluated the assumption that the initial password change had completely secured the account.

This second discovery was an important part of the investigation because it demonstrated that an account takeover may require more than a single password change to fully resolve.

## Phase 4 — Account Recovery & Containment [5:35 PM]

After determining that the account was still compromised, I took additional steps to regain control.

- Completed Google account recovery.            
- Created a completely different password.      
- Enabled 2-Step Verification.               
- Reviewed devices and active sessions.      
- Removed or investigated anything unfamiliar.

## Phase 5 — Security Audit [6-11 PM]

After regaining control, I performed a broader security review rather than stopping after changing the password.

I reviewed:

- Recovery email and phone number
- 2-Step Verification methods
- Passkeys
- Connected applications
- Active sessions
- Recognized devices
- Other account security settings

I also considered the potential impact on other accounts and credentials associated with the compromised Google account.

## Phase 6 — Investigation & Analysis [Sep 3-4, 2026]

After the account was secured, I reconstructed the sequence of events using the available evidence.

This included:

- Comparing Google Security Activity with Google My Activity.
- Identifying the suspicious `yellowewte078.es` domain.
- Correlating the **11:57 AM** phishing-site activity with the **11:57 AM** unauthorized Mac OS sign-in.
- Comparing the initial password change with the later unauthorized password change.
- Contacting the original email sender and confirming that they had not sent the message.
- Reconstructing the likely phishing and social-engineering attack chain.
- Evaluating the role of MFA and authentication prompts.
- Identifying which parts of the attack could be confirmed and which could only be considered possible.

## Key Lesson

The most important lesson from the incident was that incident response is an ongoing process, not a single action.

Changing my password was an appropriate first response, but the later unauthorized password change demonstrated why compromised accounts should continue to be monitored and investigated after the initial containment step.

The incident also showed the value of correlating multiple sources of evidence. Google's Security Activity showed the authentication and password-change events, while Google My Activity provided additional evidence of the suspicious website visit. Together, these records helped reconstruct the attack more accurately.

---

# 15. Final Reflection

This incident was an unexpected but valuable real-world cybersecurity learning experience.

Rather than only studying phishing and authentication attacks theoretically, I was able to observe an attack from the perspective of the victim and then apply incident-response concepts to investigate, contain, recover, and secure the account.

The most important realization was that the attacker did not necessarily need to break or guess a strong password.

Instead, the attack relied on:

- Trust
- Social engineering
- Credential phishing
- Authentication prompt manipulation
- A compromised trusted contact

The incident reinforced that cybersecurity is not only about technical controls.

Human behavior, authentication design, account recovery, security awareness, monitoring, and rapid incident response are all important components of account security.

Within a single day, I was able to identify the suspicious activity, reconstruct the attack timeline, recover the account, secure the account with a new unique password and 2-Step Verification, audit the account's security settings, and document the incident.

This experience changed the way I approach authentication and phishing. It also provided a practical example of how multiple cybersecurity concepts interact during a real-world account compromise.

Most importantly, the incident demonstrated the importance of treating unexpected authentication requests with caution, even when they appear to be part of a legitimate login attempt.

# Key Takeaway

> **The strongest password in the world cannot protect an account if an attacker convinces the user to give it to them.**
