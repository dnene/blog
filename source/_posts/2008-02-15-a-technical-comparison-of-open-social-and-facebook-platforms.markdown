---
date: '2008-02-15 04:33:28'
layout: post
slug: a-technical-comparison-of-open-social-and-facebook-platforms
status: publish
title: A Developer's Comparison of Open Social and Facebook platforms
comments: true
wordpress_id: '19'
categories:
- software
- web
tags:
- facebook
- opensocial
---

### Acknowledgement

There are a couple of helpful posts that set me off on the path to blog this so here's the acknowledgement :

* "Facebook vs. OpenSocial":http://blog.widgetbox.com/2007/11/facebook-v-open.html
* "OpenSocial and Facebook Platform side by side comparison":http://www.iosart.com/blog/2007/11/03/opensocial-and-facebook-platform-side-by-side-comparison/


### Introduction

I have been spending some time on the Facebook and OpenSocial platforms, and while these both attempt to solve the same need, they end up doing so quite differently. For anyone interested in web based software design, the differences are both curious and interesting, and serve an interesting contrast of how different means can get used to attempt to reach similar ends.

### The Facebook platform 

The facebook platform primarily allows the hosted applications to access facebook data through a variety of APIs. The hosted application can primarily draw itself onto a "Canvas" which is the primary channel of interaction, and can also render its own part of the profile, and plug into the messaging infrastructure (news feeds, mails etc.). When a user is working with the canvas, the canvas occupies a large share of the screen and thus dominates the interaction with the user.

### The OpenSocial platform

The Open Social platform does not really require the applications to be hosted (they can be purely client side applications using HTML, XML and Javascript). The application presents itself as a gadget and can get information about the network graphs fed into it at runtime. When I refer to a client side application I shall be referring to the gadgets with type=html whereas when I refer to an application as a server side application, I am referring to those applications which are hosted with type=url.

Now for some of the important differences.

#### 1. Application Structure

OpenSocial applications can be both client side and server side. Most of the specification and almost all the examples are those of client side applications. These typically require a high competence in HTML, XML and especially in Javascript. The applications can also be server side but this does not seem to be a predominant mechanism of application serving as yet.

Facebook applications are all hosted on a web site. These typically require a reasonable amount of server side development using a variety of languages. PHP and Java client libraries are provided and supported by Facebook. However client libraries in other languages are also available.

#### 2. Application predominance

In facebook one application is predominant at a time since the canvas it is provided occupies a majority of the rendered screen space. It is not yet clear how most OpenSocial applications will get presented, though it is quite likely that many of these will coexist on one screen with different independent spaces assigned to them

#### 3. Control

In facebook, the facebook site itself acts as a reverse proxy between the browser and the application with some substantial mod_rewrite functionality. It thus intercepts the traffic and actually changes the data stream on the fly (eg. by changing all the javascript variable names). The facebook site is always firmly in control. 

In case of OpenSocial, the application is provided a space for its gadget to be hosted, and after that the application is firmly in control ie. the browser communicates directly with the application. 

#### 4. Development Skills

Facebook application development skill requirements are likely to be more similar to those required by the conventional server side application development. However the developers will need to learn the facebook platform apis and technologies (eg. rest apis, FBML, FQL etc.). 

With OpenSocial if the application is a server side application, the development skill sets are likely to be similar to those of typical server side application development with a lesser requirement to learn newer APIs. However if the application is a client side application and you don't have serious javascript capabilities, be ready to invest in that quite a bit if you want to create a slick app. 

In a scenario where one is using a client side Open Social app, it just seems like the entire controller is getting moved over to the client, and it takes some substantial skills and effort to work through all the interactions in Javascript. I like HTML+Javascript, but dont particularly look forward to writing any more than what is necessary. I generally found that it was relatively faster to work with creating Facebook apps especially in scenarios where a reasonable amount of server side processing was required (FBML is quite helpful).

#### 5. Application or applet

While one can potentially build both applications or applets with both the platforms, seems like the focus of Facebook is to build applications while that of Open Social is to build applets (small focused functionality) which primarily seems to stem from the fact that OpenSocial borrows heavily from the Google Gadget infrastructure which itself was not focused on heavy duty application development.

#### 6. Usage of IFRAMES

OpenSocial is likely to depend a lot more on the usage of IFRAMEs. While older facebook applications used IFRAMEs, many contemporary applications do not. Combined with the fact that due to Facebook acting as a reverse proxy, which ensures that the data stream is coming from a single domain, some of the difficulties associated with IFRAMEs especially for passing information across domains and usage of SSL with IE (where I've in the past faced some difficulties) can be avoided easily in Facebook.

#### 7. Security

I am going to go on a limb here and make a conjecture here - Given facebook's reverse proxy structure and given the fact that its platform had some reasonable time to develop (unlike OpenSocial which just seems like a lets make Google Gadgets into a platform API as fast as we can approach), its "probably" going to be tougher to build more secure applications under OpenSocial

#### 8. Platform data is being pulled or pushed ?

In case of Server side OpenSocial applications, the relationship graph and user preferences are pushed into the application as a part of the URL itself (reminds me a little bit about dependency injections - except that it is being done for data). Additionally the application can communicate with OpenSocial platform through REST based APIs but these specifications are currently still in progress. In case of client side applications, this data is pulled from the OpenSocial platform and mashed up with the application data on the client side. 

Facebook also supports the pull and push models but a little differently. While it has a well defined REST interface along with the corresponding language bindings into PHP and java, it allows the application to pull data from facebook as well as push it to facebook (eg. to write to feeds). However given the reverse proxy structure, in a way Facebook is really pulling the entire application content from the application before it mashes it up on its site (eg. processing FBML tags or changing javascript variable names) before sending it to the browser.

#### 9. Standards compliance

Clearly OpenSocial could be "considered" to be more standards compliant. However that is a rather mixed blessing - All the proprietary facebook additions eg. FBML tags actually help reduce a fair amount of development effort with no equivalent capability in OpenSocial yet. There still are many things that Facebook platform supports which are as yet unaddressed by OpenSocial, so I am not so sure if the issue is so important yet. But as OpenSocial starts offering more capabilities in future versions this could start becoming an issue.


### Summary

Clearly there exist quite a few dissimilarities between OpenSocial and Facebook platforms. These differences are sufficiently large enough to tempt me to call them apples and oranges (though their end goal is to feed me). It would be wise to avoid the question "which one is better ?" However I can indeed state that as a developer having spent a large time on building server side enterprise applications, facebook just seems to be a lot more enjoyable to work with along with it likely to be more stable and more secure. But remember YMMW (some people like apples more and others oranges).

