---
date: '2008-09-27 18:16:05'
layout: post
slug: how-can-a-isp-not-be-up-to-date-on-security
status: publish
title: How can a ISP not be up to date on security ?
wordpress_id: '138'
categories:
- hardware and networking
- security
tags:
- isp
- security
- wep
- wireless
---

A few days back I blogged about [wireless security becoming a prominent issue in India](http://blog.dhananjaynene.com/2008/09/should-wifi-routers-be-required-to-mandate-strong-authentication/). In that I had essentially defended the ISPs given the fact that they could not be expected to ensure security of all the domestic routers that connect into their network. 

I received a document in mail today from my ISP explaining the steps I need to take to secure the WiFi network. While it contained a number of useful suggestions, this one completely surprised me. Here's a snapshot from the document.

![](http://blog.dhananjaynene.com/wp-content/uploads/2008/09/router-configuration.png)

**WEP ?**

Just to feel a little bit confident (_or otherwise_) about the suggestion, lets review what some other data sources say about WEP.

Wikipedia : [Wired Equivalent Privacy](http://en.wikipedia.org/wiki/Wired_Equivalent_Privacy)



> Beginning in 2001, several serious weaknesses were identified by cryptanalysts with the result that today a WEP connection can be cracked with readily available software within minutes.



Microsoft : [Improve the security of your wireless home network with Windows XP](http://www.microsoft.com/windowsxp/using/networking/security/wireless.mspx)



> 64-bit WEP (Wired Equivalent Protection). The original wireless encryption standard, it is now outdated. The main problem with it is that it can be easily "cracked." Cracking a wireless network means defeating the encryption so that you can establish a connection without being invited.



The authors of one of the more important studies (circa 2006)  "[Intercepting Mobile Communications: The Insecurity of 802.11](http://www.isaac.cs.berkeley.edu/isaac/mobicom.pdf)" on a summary page "[Security of the WEP algorithm](http://www.isaac.cs.berkeley.edu/isaac/wep-faq.html)" state :



> We have discovered a number of flaws in the WEP algorithm, which seriously undermine the security claims of the system. In particular, we found the following types of attacks:

> 
> 
	
>   * Passive attacks to decrypt traffic based on statistical analysis.
> 
	
>   * Active attack to inject new traffic from unauthorized mobile stations, based on known plaintext.
> 
	
>   * Active attacks to decrypt traffic, based on tricking the access point.
> 
	
>   * Dictionary-building attack that, after analysis of about a day's worth of traffic, allows real-time automated decryption of all traffic.
> 
 

Our analysis suggests that all of these attacks are practical to mount using only inexpensive off-the-shelf equipment. We recommend that anyone using an 802.11 wireless network not rely on WEP for security, and employ other security measures to protect their wireless network.

Note that our attacks apply to both 40-bit and the so-called 128-bit versions of WEP equally well. They also apply to networks that use 802.11b standard (802.11b is an extension to 802.11 to support higher data rates; it leaves the WEP algorithm unchanged).



Moreover the document refers to only one particular router configuration. So if the consumer (who is unlikely to be so hardware configuration savvy) owns a different router model, he has to figure it out for himself of how to configure his router based on another router configuration.

So it has been known to be insecure starting 2001, and this is the advice going out to home owners from the ISPs. **_This is somewhat scary. When the agencies that are probably the most likely educators in the realm of security are themselves offering such solutions, implementing security is going to be indeed tough._**

**Suggestion :** Can all the ISPs in India collectively get together and create a security related site, and list out the various configuration steps for all the major models being sold in India ? (and please hire a consultant who makes sure suggestions such as WEP aren't made).

The original full document containing the suggestions can be found [here](http://www.tataindicombroadband.in/mailers/images/wirelessrouter_securitysnapshots.pdf).

