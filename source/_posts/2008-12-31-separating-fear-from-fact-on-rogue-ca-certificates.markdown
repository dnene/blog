---
date: '2008-12-31 18:31:45'
layout: post
slug: separating-fear-from-fact-on-rogue-ca-certificates
status: publish
title: Separating fear from fact - On rogue CA certificates
comments: true
wordpress_id: '324'
categories:
- security
- web
tags:
- ca
- certificates
- https
- md5
- pki
- rogue
- ssl
---

Earlier today a paper summary "[MD5 considered harmful today. Creating a rogue CA certificate](http://www.win.tue.nl/hashclash/rogue-ca/)" and an article "[MD5 collision creates rogue Certificate Authority](http://www.crunchgear.com/2008/12/30/md5-collision-creates-rogue-certificate-authority/)" appeared about a presentation to be made at the [Chaos Communication Congress](http://events.ccc.de/congress/2008/Fahrplan/day_2008-12-30.en.html) in Berlin.  John Viega commented on the issue by writing a lengthy post "[The sky is not falling (re: today's PKI attack)](http://broadcast.oreilly.com/2008/12/the-sky-is-not-falling-on-toda.html)". Tim Callan from Verisign soon after reverted with its response "[This morning's MD5 attack - resolved](https://blogs.verisign.com/ssl-blog/2008/12/on_md5_vulnerabilities_and_mit.php)". This post refers to implications of the same for websites and internet users the way I understand it.

**What does the attack do ?** The attack describes a mechanism by which a malicious user can create a rogue CA certificate and in turn create and sign tons of additional certificates which will also be rogue but which for all practical purposes shall be treated as valid certificates by a browser. It specifically refers to an attack mechanism which exploits the MD5 collision generation ease (which has been known for quite some time) and the fact that root CAs use (or now perhaps used to use) MD5 for generating the certificate digests and some predictable mechanisms for generating additional data (eg. serial numbers). While most CAs will soon plug the hole (if they haven't already), the primary concern point relates to the fact that someone already may have created rogue CA certificates and can use that to create additional certificates at will. 

**I run a website with https and have a certificate. Should I be concerned ?** You have a certificate that proves your credentials. This certificate continues to be valid and there is no change in its status. However there is now a possibility that someone who has setup a rogue CA can now issue additional certificates that claim to be you. Thus while this particular attack does not apparently increase your website vulnerability, it does make it potentially feasible for the rogue CA to now send your users to some other site which uses the new rogue certificate claiming to be you. However such new sites will typically use a different domain and are unlikely to use the same domain you do so long as you own and manage your website domain. Thus if your company is called ABC and hypothetically runs the website "abc.com", another certificate could get issued by a rogue CA asserting the claims to be "ABC" running on a hypothetically different site- "abc2.com". Users by default are unlikely to be entering abc2.com into their browser address bars to get to you and to that extent you are not so likely to be affected.  Note that you are still vulnerable to phishing attack (as you were before this attack was discovered) where a malicious site "abc2.com" could send spurious hyperlinks to your user through email or websites which point to "abc2.com" which used a certificate issued to abc2.com. What this attack makes possible but only in conjunction with yet another separate class of attacks called "redirection attacks" is that a rogue certificate can now be presented to your users as "abc.com" and they will be no wiser. So in summary, most existing risks especially those related to phishing continue, but now another possibility exists whereby another site can claim to be your site and present a _more authentic_ set of credentials that support its claim - but only in conjunction with exploiting an additional set of malicious net based attacks. 

However if your certificate has been signed using md5, you should consider reverting back to your CA to replace it with a new one. This is not absolutely necessary since the existing certificates even if signed using md5 have not been invalidated or rendered insecure per se. Verisign has already suspended its normal replacement fees for such certificates. **Update:**Given the fact that plugins like SSL blacklist are likely to offer warnings to the end users should they come to your site, I would think that it might make sense to replace the certificates not because your current site or certificates are any less secure, but that the user's interaction potentially could be (it is the user who runs a higher risk now).

**Should I be concerned as a user ?**Yes. Make sure you continue to follow all the good anti-phishing practices. Try to make sure your browsers are regularly updated with the security patches. Also make sure by either looking at the address bar (if the page is not in frames) or especially by looking at the domain name in the certificate that you are indeed communicating with a domain that you are aware of as the valid domain for the site. If the above attack has been used in conjunction with a redirection attack you are unlikely to be able to figure it out. The only way to protect yourself against such a possibility is to ensure that every certificate in the signing chain has been signed using a non-md5 algorithm (a rather onerous task to perform each time a new https connection is initiated). While I am not aware of any such existing capabilities, I would not be surprised if I start seeing either newer configuration parameters or browser add ons which disallow any https connections which use one or more md5 signed certificates in the certificate chain. 

**So is the sky falling ?** It isn't in the sense that existing websites or certificates haven't become any more vulnerable. As John Viega and the original researchers point out, it is exceedingly difficult to spoof a valid already issued website certificate - it requires a specially crafted certificate request. The primary vulnerability stems from creating a rogue CA certificate and using that in turn to generate addtional rogue certificates on demand. This does allow phishing sophistication to be taken a notch higher and more difficult to detect. Given that CAs have either already or are in the process of fixing the vulnerability, newer rogue CA certificates may no longer be feasible using the attack method specified. However if and whether any current rogue CA certificates have already been manufactured is anybody's guess (the researching attackers did create one even though it was back dated so as to be non-functional). Since this is a question that is tough to answer, probably a way to make people feel a little bit more secure is to allow a capability (configuration / addon) in the browsers to disallow / warn of any https communications with an md5 signed certificate anywhere in the chain. 

**Update:**The following firefox addons have since the writing of this post appeared which check for md5 digests in the certificate chain.



	
  * [SSL Blacklist 4.0](http://www.codefromthe70s.org/sslblacklist.aspx) : Allows root CA certs to be md5 signed since these are not likely to be rogue certificates.




_Disclaimer :I am a software architect and not a specialist security professional. These are my opinions expressed to the best of my understanding as of the time of writing this post. As in all matters related to health, law and security you should consult an appropriate professional when you need reliable advice and opinions. As with any developing piece of news, I do wonder if I might have not understood something entirely accurately. If you do find any errors or omissions, do point them out in the comments. _

