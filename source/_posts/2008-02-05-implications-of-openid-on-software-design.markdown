---
date: '2008-02-05 15:40:41'
layout: post
slug: implications-of-openid-on-software-design
status: publish
title: Implications of OpenID on software design
comments: true
wordpress_id: '18'
categories:
- software
- web
tags:
- openid
- software design
- web
---

In my earlier post "OpenID or OpenAvataar ? UserID or AvataarID ?":http://blog.dhananjaynene.com/archives/14 , I referred to the fact that a user may have multiple OpenIDs (my hypothesis being that these will reflect his various avataars). The question I attempt to answer here is how will usage of OpenID impact the design of the software itself.

### Case of a site with OpenID integration

Here's where things get interesting. One of the sites I went to "dzone":http://www.dzone.com which is supposed to be for developers. It supports OpenIDs, but when I try to go to the registration page, here's what I get.

![dzone-register.png](http://blog.dhananjaynene.com/wp-content/uploads/2008/02/dzone-register.png)

Notice that the site seems to be supporting an OpenID login (bottom right) but yet goes on to add the following : *"You will need to set your password using the one-time link you received in your email before you can login."*. I could not find any way of using my openID without the site creating a new user id and a password for me that it would send me to my email address. (And this is a rather geeky set of sites which seems to implement a single user id across JavaLobby, EclipseZone, dzone and JavaBlackBelt.). Clearly this was not how I believe OpenIDs were intended to be used (remember they are supposed to reduce userids and passwords) since the site expects me to create one more userid and one more password.

I shall assume that these sites are in the middle of a incremental transformation towards an OpenID oriented site and hopefully at a future date, I can simply register myself using my OpenID without creating any new userid or password.

### Design considerations when using OpenID


I submit the following as requirements for any OpenID based account registration

* My OpenID should be my userID / accountID (i.e. I am my OpenID) 
* I should not be required to provide any new userid / password
* I may be required to provide additional information required by the site which is not published within my OpenID

But wait a minute we just went past the hypothesis that multiple OpenIDs could map to one Identity (UserID?). If that has to hold true, I need a new user id to be created so that I could map multiple OpenIDs onto it. But that seems to only exacerbate the problem that OpenID originally tried to solve. ie. the runaway number of userids that I seem to be having. Clearly something does not line up well here.

The one reasonable way I can imagine this working out fine is if the site itself focuses on the Avataar / Persona / OpenID and not necessarily the user's unique Identity. ie. The site just keeps track of Avataars and does not attempt to link it into one single user id, and does not create a password for the same. Now me as an independent professional in my individual capacity and myself as the agent of an organisation could log in into the site independently as different avataars, and the site could treat each of my avataars as simply equivalent to what would've otherwise been two different userids. Thus the site no longer needs to keep track of my userid or my unique identity. It simply keeps track of Avataars or Personas, and in general does not focus on specific person's identity. 

However there are some sites which require user identity specific information (e.g. Online Drivers License Renewal assuming thats allowed in your region). Clearly in such situations, additional attributes would need to get attached to the avataar which reflect the specific user identity (actually more likely - the OpenID would need to get attached to the specific user identity). Whether one should require a unique constraint around such additional attributes (eg. two avataars cannot have the same drivers license number) is probably an site / application specific decision.

It is also quite likely that I might want to change the OpenID I use for a particular avataar. So there probably needs to be an internal unique identifier for the avataar, with an ability to change the OpenID that is used to login as that avataar (similar to the ability to change the primary email address). This actually is something which the OpenID spec does not address, thus transferring the onus of maintenance of such a mapping between an OpenID and an application / site specific avataar id (which probably just would end up being an internally generated number). As an aside this is something "inames":http://www.inames.net/business.html does tackle explicitly 

> This means all global i-names have a corresponding global i-number that represents a permanent identity that will never be reassigned even if a global i-name expires or is sold.

### Do I need a password field / column ?

If you are supporting OpenID users only then you no longer need it. But if you are supporting both site specific userIDs and OpenIDs you will still need to conduct authentication for the site specific userIDs. If you are building in support for OpenID in an existing application, you will probably keep the encrypted password or the password hash in the password column for the site specific user ids.

If you are building a new application however, and you still want to support users who do not have an OpenID, consider building your own OpenID provider. Thus generate an OpenID for such users only when required. Have the User Account Maintenance capabilities simply depend upon an OpenID provider (one of which could be your own). Should you want to get started on using some open source OpenID providers here's a list of "OpenID Open Source Libraries":http://wiki.openid.net/Libraries .


### Summary

* Design for new user registrations using only their OpenIDs ie. no new externally known userid or password
* If migrating towards a OpenID based infrastructure, you will need both non OpenID and OpenID users to coexist. 
* Map the OpenID onto an internally generated and externally invisible id.
* If you want to allow users to link up their multiple OpenIDs, build for associating each such OpenID with the same internal Id. In this case users can freely add / remove other OpenIDs, subject to the constraint that at least one OpenID always remains associated with the account.
* If you would like to deal only with the user avataars and offer separate view of your site to each of his avataars, the ability to link multiple OpenIDs together may not be required. However do allow the user to change his OpenID (since he may find a good reason to want to change his OpenID provider in the future).
* If you are interested in supporting only one id per person consider using a different externally known identifier that uniquely identifies the person (could be the social security number or driving license number or something else). That should be the internal handle to the user which is enforced as a unique identifier (you can still choose to have another internally generated id). You can now associate one or more OpenIDs with that user and allow any one of them to be used as the authentication mechanism.
* If you are building a new application ground up, consider designing your application to work with OpenIDs alone. To cater to users not having OpenIDs, build a separate small application to allow them to create and maintain their OpenIDs

