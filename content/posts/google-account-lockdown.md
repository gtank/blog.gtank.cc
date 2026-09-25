---
title: "How to lock down your Google account"
date: 2019-08-17T17:13:09Z
lastmod: 2019-08-17T17:15:06Z
slug: "google-account-lockdown"
author: "George Tankersley"
---

If someone has access to your Google account, they have immense power to track  or impersonate you. Whether they broke in, stole a computer, or you gave them access and now want them gone, here's what you can do to kick them out.

### First, get a computer you can trust

Because we're going to generate new passwords and security settings, you'll need to use a computer you trust.  This is very situational; someone who is intent on stalking you might have installed spyware on your computer that will tell them what your new passwords are, so you shouldn't use your normal machine when you do these resets. If it's less of a personally malicious situation, you probably don't need to worry about this!

If you don't have a machine you trust, here are some ways to get one:

- Borrow a friend's computer.
- Buy a Chromebook. They are widely available, cheap, and very secure. It does not have to be a nice one, and you can create a new Google account just for using the Chromebook.
- If you live near an Apple Store and can handle the (temporary) monetary commitment, their [generous return policy](https://www.apple.com/shop/help/returns_refund) basically allows you to try out a Macbook free for two weeks.

If you're worried but want to continue using your current computer, you can reinstall its operating system. This will get rid of  most spyware. Keeping backups of your personal files is OK! It's the system part you want to replace.

Now, on to Google.

### Change your passwords

1. [Change your password](https://support.google.com/accounts/answer/41078) for Google. Access to your email can be used to gain access to any other services you use by requesting a password reset, so do this as soon as possible.
2. Enable 2-step verification for your Google account. Google offers [instructions](https://www.google.com/landing/2step). You should prefer to enable 2-step [with a security key](https://yubikey.me/google-account/)\*. The second-best option is to use the Google Authenticator app on your phone. You should enroll more than one 2-step option so you don't get locked out from your account. This can be two different security keys, a security key and Authenticator, or Authenticator and some backup codes written on a piece of paper that you keep safe.  
     
   Do not use SMS 2-step, since associating a phone number with your account (especially one known by the person you're kicking out) can open you up to further mischief.
3. Force logout all other sessions. This will make sure that anyone else who was signed in as you gets kicked out. Instructions are [here](https://support.google.com/mail/answer/8154), under the heading "Sign out from another computer."
4. Change the passwords for all your other online accounts. Use a password manager (I recommend [1Password](https://1password.com/)) to generate secure, unique passwords for each account. This is really annoying to do, but if someone had access to your email they could have used it to access all of your other accounts. If you're short on time or mental energy, prioritize things like Facebook, Twitter, and your bank.

\* The most trusted manufacturer of security keys is a company called Yubico, and this is their [least expensive model](https://www.yubico.com/product/security-key-by-yubico). It will do everything you need, unless your computer does not have that type of USB port. If your computer only has USB-C, like most recent Macbooks, buy this one: <https://www.yubico.com/product/yubikey-5c-nano>.

### Review your Google security settings

While logged in to to your account, go to the [Google security settings page](https://myaccount.google.com/security?pli=1&pageId=none).

1. Is your account recovery information valid? Remove any recovery phone numbers or recovery emails that aren't yours. I recommend removing *all* recovery phone numbers (see above about enabling further mischief). Make sure you've already changed the passwords on any recovery email accounts.
2. Check your "app passwords" (which can still log in without entering your new password) and remove any that you didn't add yourself.

### Review your other Google settings

The intent here is to stop any ongoing monitoring and assess what information the person might have seen.

1. Are your emails set up to forward somewhere? Here's how to [turn off automatic forwarding](https://support.google.com/mail/answer/10957?hl=en).
2. Are there delegated accounts that aren't you? They have the ability to send email as if they were you. [Here is how to remove them](https://support.google.com/mail/answer/138350?hl=en).
3. Are there email aliases that aren't you? You can check and remove them in the **Accounts** tab of your email settings.
4. Is location sharing on? Here's how to [stop sharing](https://support.google.com/maps/answer/7326816).
5. Was your location history enabled? [This page](https://support.google.com/accounts/answer/3118687?hl=en) explains how to turn it off and how to see what information about where you go might have been available.
6. Is calendar sharing or delegation on? [Here](https://support.google.com/calendar/answer/37082?hl=en) is how to stop those.
7. Is photo sharing on? If you have an Android phone, any picture you took (see next point) might have been shared. Here's how to [stop sharing](https://support.google.com/photos/answer/7378858).
8. Check your sync settings. If you're using an Android phone, it might have automatically synced and made information from the phone accessible from the web, such as photos, contacts, or notes.
9. **(optional/may not apply)** Review the share settings for all of your Google docs, spreadsheets, Drive folders, etc.
10. Check your installed apps. Is anything installed on your computers or phones that you didn't install yourself? Of particular note: if you're on Android, it is possible to use the web interface to the Play Store to remotely install things on phones registered to that account, so do check. The good news here is that phone spyware  often requires configuration to work, so *only* a remote install through Play won't activate it unless they were later able to use the phone.
11. Check for unauthorized access: click "details" at the bottom right corner of the Gmail page to view the IP addresses and more details of last 10 logins to the account. Take a screenshot. If necessary, the abuser's info on this list can confirm the unauthorized access and be used for restraining orders, police reports, etc.

### Acknowledgements

This post incorporates advice from several people who may not wish to be credited. Thanks anyway, and let me know if you do.

YOUR NAME HERE. Did I miss something? There's so much surface area to a Google account! Feel free to [point out](mailto:blog@gtank.cc) anything that isn't here.
