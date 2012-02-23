---
date: '2008-09-03 02:26:05'
layout: post
slug: is-google-chrome-under-partiallyreporting-ram-usage
status: publish
title: Is google chrome under / partially reporting RAM usage ?
comments: true
wordpress_id: '47'
categories:
- software
- web
tags:
- chrome
- google
- memory
---

[caption id="attachment_48" align="alignleft" width="474" caption="Google Chrome memory utilisation"][![Google Chrome memory utilisation](http://blog.dhananjaynene.com/wp-content/uploads/2008/09/google-chrome-memory.png)](http://blog.dhananjaynene.com/wp-content/uploads/2008/09/google-chrome-memory.png)[/caption] 
  

You can see both the windows task manager and the google chrome task manager. Google chrome seems to be consistently under reporting the memory utilisation. Is there something more than meets the eye (at least mine) or is there some part of the memory which Windows Task Manager reports (eg. that used by the kernel on behalf of the user process) but google chrome doesn't or is it Windows over reporting memory usage ? Note : I captured the snapshot after pausing for some time to allow the memory reporting to catch up in both the tools.

Any thoughts or conjectures ?

PS: Sorry the rows are not in the same order in the two tables .. so you might have to mentally sort them to figure out the matching ones. 

**Update :**Just realised that since the overall browser memory utilisation is now captured and shown separately from that of a particular page, it just makes measuring memory utilisation of a particular web page so much more accurate and easier. I wonder how long before the following things happen :



	
  * Architects / Leads start laying down "thou's web page shalt not consume more than ## kBs in Google chrome" to the developers

	
  * My favourite web page is better than yours since it consumes lesser memory  (and I have the snapshot to prove it)




