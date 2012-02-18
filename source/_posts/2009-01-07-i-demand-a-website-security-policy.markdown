---
date: '2009-01-07 14:15:11'
layout: post
slug: i-demand-a-website-security-policy
status: publish
title: I demand a website security policy
wordpress_id: '396'
categories:
- architecture
- security
- web
tags:
- password
- twitter hack
---

Came across [Weak Password Brings 'Happiness' to Twitter Hacker](http://blog.wired.com/27bstroke6/2009/01/professed-twitt.html). While it primarily focused on the fact that a user with admin privileges and with account access over the internet had a weak password, which is rather surprising, Makes me very worried to now have accounts on so many websites which might not be treated with appropriate level of rigour.

I therefore demand that sites who want me to create an account with them enforce and publish a security policy comprising of at least the following basic password protection guidelines (I have attempted to keep out so many of the advanced ones to keep it simple)




	
  1. We shall never store your password in plain text in the databases, in the logfiles, on a piece of paper. In fact we will never store it anywhere in plain text - Period. We shall always store it as a hash digest (SHA1 or stronger) or only in case our functionality requires us to be able to access the password in plain text (eg. to pass on to another website), we shall store it in an encrypted fashion with an industry standard encryption scheme such as 3DES or equivalent in which case the encryption key will also be securely stored.

	
  2. The said encryption or hashing process will include both random salts and also the unique internal or external user identifier, so that two user's sharing the same password will still not have the same hash / encrypted string.

	
  3. For password reset requests not triggered by a logged in end user (eg. administrator initiated), we will strive to autogenerate new passwords on demand and mail them out to the user directly.  rather than let the administrators set it to a password of their choice. For situations that require an alternative workflow, we will ensure that password reset requests not coming from the user directly will get approved by at least another different admin person before getting effected (4 eyes principle).

	
  4. We will ensure that the login sequence always proceeds over SSL



Just like privacy policies I expect the websites to start publishing a minimum security policy without giving away any substantial implementation details. I think it is only reasonable for me to expect the websites to earn my trust before entrusting them with my password.

Update: Random passwords were being generated but how these passwords were shared with others is unclear - eg. "After resetting the password for the account, he gave the credentials to five people". vs. "doesn't know what the reset passwords were, because Twitter resets them randomly with a 12-character string of numbers and letters."

