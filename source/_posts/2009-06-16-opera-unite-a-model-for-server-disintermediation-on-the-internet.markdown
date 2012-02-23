---
date: '2009-06-16 14:52:14'
layout: post
slug: opera-unite-a-model-for-server-disintermediation-on-the-internet
status: publish
title: 'Opera Unite : A model for server disintermediation on the internet'
comments: true
wordpress_id: '737'
categories:
- architecture
- Internet and Social Media
- software
---

Since [Opera Unite](http://unite.opera.com/) got introduced a couple of hours ago. I had a chance to do a quick review of the functionality. As I begin to play with it, couldn't but help write a short post on what it seems to be doing.

Here's what my preliminary look into Opera Unite suggested :



	
  * It has a web server bundled into the browser.

	
  * It allows you to write (what would be in current parlance be called) serverside apps using javascript which are hosted by the web server.

	
  * Sample applications include photosharing apps to share photos on your desktop or fridge which allows internet users to post notes to you.

	
  * It allows these apps to be accessed across a router on a dynamic IP using a subdomain on a centralised operaunite service ..operaunite.com (not sure yet whether across a firewall as well but seems so)

	
  * It allows these applications to be shared / published using the config.xml file (similar to google widgets ?). There's also a central opera directory for the same, but I don't think sharing is restricted to that service alone (apple are you listening ?)

	
  * These applications can be further installed by other users of Opera Unite (which is the web server service running inside the Opera browser).



So why is this such a big deal ? Without going into any further elaboration let us just imagine user's used it for following (for desktop-to-desktop or peer-to-peer) communication :


	
  * Send mails to each other - disintermediates email services

	
  * Send short burst messages to each other - potentially disintermediates twitter

	
  * Share files with each other - disintermediates ftp servers and shares characteristics with gnutella, kazaa etc.

	
  * Share resume, product profiles etc. - disintermediates traditional static web hosts

	
  * Build networks of other interested users - disintermediates linkedin, facebook



The constraint it introduces is that there is no global list of user ids to search from - you need to know the URL upfront (like the facebook vanity URL). 

The possibilities are many. As are the interesting uses this model could be put to. And the characteristics of improved privacy, data ownership and control (including fine grained access control or selectivity). And there does remain a potential for malicious actors and virus writers to ride on popular apps to exploit vulnerabilities to tend to their nefarious needs.

As I put it differently in another tweet - this opens up the possibility for a google wave without the google in it.

**References :**



	
  * [Taking the web into our hands one computer at a time](http://labs.opera.com/news/2009/06/16/) : An introductory writeup

	
  * [An introduction to Opera Unite](http://dev.opera.com/articles/view/an-introduction-to-opera-unite/) : A get started guide

	
  * [Opera Unite developer's primer](http://dev.opera.com/articles/view/opera-unite-developer-primer/) : A primer for writing server side applications








