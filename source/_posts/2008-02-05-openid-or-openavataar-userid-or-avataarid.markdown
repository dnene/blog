---
date: '2008-02-05 15:39:39'
layout: post
slug: openid-or-openavataar-userid-or-avataarid
status: publish
title: OpenID or OpenAvataar ? UserID or AvataarID ?
wordpress_id: '14'
categories:
- java
- ruby
- software
- web
tags:
- openid
- software design
- web
---

### Introduction to OpenID

Lately with the prolific activity around "OpenID":http://openid.net and especially with a biggies like "Yahoo":http://openid.yahoo.com and "AOL":http://dev.aol.com/topic/openid , I was getting curious about how this will influence identity management both on the public internet and corporate intranets. One of the nice starting points to understand OpenID is "OpenID » What is OpenID":http://openid.net/what/ , and for developers is "OpenID » developers":http://openid.net/developers/

### How many OpenIDs per person ?

The "OpenID » What is OpenID":http://openid.net/what/ page says

> OpenID eliminates the need for multiple usernames across different websites, simplifying your online experience.

This is a wonderful thing to happen, since we have been sufficiently bothered with trying to keep track of a whole bunch of userids and the passwords. It further goes on to add

> For businesses, this means a lower cost of password and account management, while drawing new web traffic. OpenID lowers user frustration by letting users have control of their login.

There is some correlation that is now being drawn between an OpenID id and an "account". However it is still not yet clear what the OpenID reflects. Further it goes on to add 

> For geeks, OpenID is an open, decentralized, free framework for user-centric digital identity

Now I start wondering does my OpenID reflect my user-centric digital identity ? Turns out I can have as many OpenIDs as I want. So there seems to be a *many to one relationship* between *my OpenIDs* and *me*.

This then is further reinforced in one of the articles that are referred to from the developer page "A Recipe for OpenID-Enabling Your Site":http://www.plaxo.com/api/openid_recipe . This page contains the following text : 

> Here's an overview of what you're going to add to your site:
1. A new database table to map OpenIDs to your internal user IDs
* It's a many-to-one relationship (each user can have multiple OpenIDs attached to their account, but a given OpenID can only be claimed by a single user)

The "OpenID specification":http://openid.net/specs/openid-authentication-2_0.html of course provides the most appropriate and insightful definition of OpenID even though it kind of has us wondering - what is the ID in OpenID (and leaves us with the loosely comfortable with the thought that the ID is simply an ID in the web space and has nothing to do with User Identity).

> OpenID Authentication provides a way to prove that an end user controls an Identifier.

So it seems sufficiently clear that an OpenID is not meant to reflect my identity the way my Tax Identification Number or Social Security Number or Drivers License Number works (ie. exactly one valid identifier per person at any point in time).

It is now clear OpenID reflects what I own (to the exclusion of others) rather than who I am. My OpenID does is not a unique or exclusive reference to me or any of my identifying characteristics as much as it asserts the fact that I control the ID and therefore others don't. But that still leaves me thinking even harder - how many OpenIDs do I really need ?

### Why would I want to have multiple OpenIDs.


Sure having multiple ids is nice since I now have multiple providers, I am not tied to any particular one, I can have redundant ids. etc. etc. The reasons are quite similar to why I might have different email ids. Turns out at least in my case the most dominant reason why I would want to have different open ids is the same why I would want to have different email ids : *I have different facets to my identity and I would like each to be reflected differently*. Thus I might want to have _one_ OpenID to reflect my persona as a professional consultant, _another_ to reflect me in my personal and individual capacity, and _yet_ another to reflect my persona within the context of a particular of a client / project / organisation. This way I could use my personal openID across all my social networking sites, my professional one across a smaller number of professionally focused sites, and probably my organisation specific ID being used for sites hosted by a particular organisation which really isn't focused on my global identity but wants to create and independently manage a single ID within that organisation. The number of OpenIDs I would want to reasonably maintain is the number of personas or avataars I want to project on the web. Thus I probably need 3 *avataars* instead of 1 identity or the hundreds of site specific userid/password combinations I today have. Probably I can better understand the word OpenID if in my mind I was to call it *OpenAvataar*.  _My Submission here is that each person may have multiple openIDs and we shall probably be using each one to reflect one avataar. This is likely to be the primary reason why each person may have multiple OpenIDs"._ 

### Summary

* An OpenID does *not* exclusively identify a particular user. It simply asserts that users control over the OpenID.
* A user may have multiple OpenIDs. My hypothesis is that each of these is likely to reflect one of his avataars


