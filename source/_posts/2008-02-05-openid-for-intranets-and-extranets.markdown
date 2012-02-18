---
date: '2008-02-05 15:41:48'
layout: post
slug: openid-for-intranets-and-extranets
status: publish
title: OpenID for Intranets and Extranets
wordpress_id: '16'
categories:
- java
- ruby
- software
---

This post continues from "OpenID or OpenAvataar ? UserID or AvataarID ?":http://blog.dhananjaynene.com/archives/14, and "Implications of OpenID on software design":http://blog.dhananjaynene.com/archives/18 and specifically looks at how the OpenID specification could be used within corporate intranets and extranets. 

h3. Why would a corporate even want to implement OpenID

The problem OpenID is attempting to solve is widely prevalent within corporates as well. There are multiple applications, databases, web sites etc. which seem to want to create their own userid / password combinations. There is an enormous amount of activity and effort that a corporate has to invest in identity management. Additionally there is today a compelling need to for a user to transparently navigate across a wide variety of corporate applications especially in situation where each application performs its identity management tasks independently. This is precisely one reason why identity management, and Single Sign On are terms which are in many cases far more important to a software designer within a corporate context than on the public internet.

h3. What is different about corporate environments

For starters, there is a much stronger need within corporate environments to be able to associate a person's identity with the authentication mechanism (eg. OpenID). I argued earlier that OpenID should reflect avataars and not necessarily a specific person's identity especially within the public internet's context. However many social interactions on the internet are relatively casual in nature and in most cases are likely to be sufficiently non-risky at least when looked at strictly from a commercial transaction perspective. The internet is a very democratic environment where most people are treated as fairly equal to each other. Within a corporate environment however each person has varying roles and along with that comes a varying set of responsibilities. It is fairly unlikely that corporate environments would easily allow any arbitrary OpenIDs (such as one created by a employee from one of a plethora of Internet based OpenID providers or even by creating a self hosted provider on his desktop himself). Corporates will be compelled to define ACLs around various corporate resources and these will need to be based on user identities and *not* their avataars.

h3. How could a corporate implement OpenID ?

First there would need to be sufficient conviction that this indeed helps solve the problem more effectively than many other Identity Management solutions out there (Some of the competing strategies are based on LDAP and SAML). Assuming that one reaches that conclusion, the way forward would be to either identify specific public OpenID providers or more likely create an internal OpenID provider (which may in turn be a layer on top of the Directory Services). The URLs as registered / provided from this internal providers would serve as a mechanism for the user identity presentment and verification. 

Would there be a conviction that OpenID would be able to provide better identity verification solutions than the other solutions out there ? I suspect not always. However it is more likely in scenarios such as follows :

(a) Its a highly decentralised and large corporate with independent identity management functions being carried out by a variety of sub units.
(b) There is a necessity to establish broader extranets and expose the corporate application to other partners or consume applications and services provided by various partners. 

Even in both the above situation there are other identity management solutions that do exist. However I do believe that OpenID is better placed at being able to find its place in these situations given the _relative_ simplicity of implementing it and more importantly the notion of standardisation that it brings with it. Moreover in a heterogenous world especially with all the various partner organisations and the identities that these spawn (which could be maintained in fairly diverse ways using different technology platforms) also starting to play a role, there will be a necessity for identity management, presentment and verification solutions to start talking one lingua franca and OpenID just might be it. There are other claims to be already having the common language (eg. SAML, LDAP), but I suspect the advantage OpenID brings in terms of a standard, widely used specification and especially in terms of it riding primarily on HTTP(s) will help it hold its own in many situations. It is imjportant to note here that SAML perhaps provides a much more structured mechanism of data exchange and providing more sophisticated assertions about the user identity and it may be so that it is more appropriate in a given context. However I believe OpenID is likely to be used more often than SAML in most less intensive cases primarily because of its simplicity and given the presumption that OpenID is far far more likely to be successful in the internet than SAML.

However OpenID only solves a part of the problem - ie. identity of the users. Within extranets, sometimes its important to establish the identity of the partner organisations as well. OpenID is unlike to be able to solve the same by itself. However there are other initiatives such as "inames":http://www.inames.net which are at least attempting to solve that piece of the puzzle though it would be important in such cases that the individual inames and OpenIDs be seamlessly integrated.



