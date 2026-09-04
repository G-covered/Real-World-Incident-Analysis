# Real-World Phishing & Account Takeover Incident Analysis

## Overview

On September 3, 2026, I experienced and investigated a real-world phishing attack that resulted in unauthorized access to my Google account.

What initially appeared to be a legitimate wedding invitation was actually a phishing message sent from a friend's compromised account. The attack used social engineering, a fraudulent Google login page, and a legitimate Google authentication process to convince me to provide credentials and approve an authentication request.

The incident was detected shortly after the initial compromise, allowing me to investigate the attack, recover the account, change the compromised credentials, enable 2-Step Verification, and audit the account's security settings.

This report documents the attack, detection process, response, and lessons learned.

---

# 1. Initial Attack

The attack began with an email that appeared to come from a friend.

The message looked like a legitimate wedding invitation and contained a link that required me to log into my Google account.

At the time, there was no obvious indication that the message was malicious.

### Why the message appeared legitimate

- It came from a known contact.
- Real website.
- The message appeared to be a normal invitation.
- The requested Google login seemed reasonable for accessing the invitation.
- It had Google's two step verification.
- The sender's account had apparently already been compromised.

After the incident, I contacted my friend and discovered that they had **not actually sent the message**.

This indicated that their account had likely been compromised and subsequently used to distribute the phishing message to their contacts.

---

# 2. Credential Phishing

After following the link, I was presented with a Google login page that appeared legitimate.

I entered my Google credentials.

The phishing site then requested additional verification and instructed me to confirm which three numbers were correct.

At the time, this appeared to be a legitimate Google 2-Step Verification process.

I approved the authentication request.

### Important Observation

The attacker did not necessarily need to "crack" my password.

The password I entered was strong and difficult to guess.

Instead, the attack relied on **social engineering** to convince me to voluntarily enter the credentials into a fraudulent login page and approve an authentication request.

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

| Time | Event | Device | Interpretation |
|---|---|---|---|
| 11:57 AM | New sign-in | Mac OS | Unrecognized login |
| 12:07 PM | Password changed | Galaxy S26 Ultra | Password changed by me |
| 5:20 PM | Password changed | Mac OS | Unrecognized password change |
| 5:35 PM | 2-Step Verification enabled | Galaxy S26 Ultra | Security measure enabled by me |
| 5:49 PM | New sign-in | Windows | Recognized device |

The most significant event was the password change attributed to the Mac OS session.

I had changed my password earlier in the day, but several hours later Google reported another password change from the unfamiliar Mac OS session.

This explained why the password I had originally created was no longer accepted.

---

# 5. How the Attack Likely Worked

The exact implementation used by the attacker cannot be confirmed from Google's security logs alone.

However, the behavior was consistent with a phishing attack that leveraged a legitimate authentication process.

A simplified attack flow is:

```text
Compromised Friend's Account
            |
            v
    Phishing Email Sent
            |
            v
   Fake Wedding Invitation
            |
            v
     Fake Google Login
            |
            v
      Credentials Entered
            |
            v
 Attacker Initiates Google Login
            |
            v
 Legitimate Google Verification
            |
            v
 Victim Receives Authentication Prompt
            |
            v
 Victim Approves Verification
            |
            v
   Attacker Gains Account Access


The exact technical mechanism used by the attacker cannot be confirmed solely from the Google security logs.

However, the behavior was consistent with a phishing attack that leveraged a legitimate authentication process and social engineering.

One possible technique is an **Adversary-in-the-Middle (AiTM)** attack, in which an attacker relays authentication information between the victim and the legitimate authentication service.

Regardless of the exact implementation, the attack demonstrated that the attacker did not necessarily need to break Google's authentication system. Instead, the attacker manipulated the user into participating in the authentication process.

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

The authentication request appeared legitimate because it resembled Google's normal security process.

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
