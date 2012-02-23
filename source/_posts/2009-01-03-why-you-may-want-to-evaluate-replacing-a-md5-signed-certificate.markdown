---
date: '2009-01-03 15:59:40'
layout: post
slug: why-you-may-want-to-evaluate-replacing-a-md5-signed-certificate
status: publish
title: Why you may want to evaluate replacing a md5 signed certificate
comments: true
wordpress_id: '374'
categories:
- security
- software
- web
tags:
- ca
- certificates
- md5
- rogue
---

A few days ago I blogged about "[Separating fear from fact - On rogue CA certificates](http://blog.dhananjaynene.com/2008/12/separating-fear-from-fact-on-rogue-ca-certificates/)". It was an attempt at understanding and then expressing that understanding about the rather complex issue raised by the attack. My understanding in that post was subsequently reinforced by other incidents that this attack has less to do with the security of the web site itself and more to do with the probable security of the interactions of the end user with the web site. 

One easy way to find out whether your site has a md5 certificate in its chain is to use the [SSL Blacklist 4.0](http://www.codefromthe70s.org/sslblacklist.aspx) Firefox plugin. After installing the plugin open an https connection with your site. If there is a md5 certificate in the chain (excluding the root CA certificates), you are likely to see a pop up box as below :

![ssl-blacklist-output-2](http://blog.dhananjaynene.com/wp-content/uploads/2009/01/ssl-blacklist-output-2.png)

If you do see that, you have to deal with the fact that over a period of time more and more users are likely to see the same and feel concerned about interacting with your site. Is the fear well founded ? It is my rather speculative belief that the fears are likely to be far more than the actual risk the user runs, and the dialog box shown above makes things seem far more serious then what they perhaps could be in most cases. 

However as in this case, the attack has been published and plugins like these are in the wild. The appropriate response would be to recognise that there is indeed a likelihood of your user's interaction with such certificates now perceived to be statistically carrying a higher degree of risk than those with non MD5 certificates. 

So even if your site continues to be as secure as it was before the attack was published, your user's interactions no longer might be perceived to be so. Hence, you might want to evaluate the option of replacing the certificate with a non md5 signed certificate. Verisign has already waived its fees on such replacement certificates, so there might not be any additional explicit cash outflow in such a situation (though you would need to spend additional time and effort for implementing the change). 
