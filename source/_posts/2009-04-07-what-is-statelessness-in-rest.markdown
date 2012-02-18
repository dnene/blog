---
date: '2009-04-07 22:13:15'
layout: post
slug: what-is-statelessness-in-rest
status: publish
title: What is statelessness in REST ?
wordpress_id: '622'
categories:
- architecture
- programming
tags:
- rest
- statelessness
---

In my post [Fomenting unREST : Is RESTfulness a semantics game ? Why does REST require statelessness ?](http://blog.dhananjaynene.com/2008/11/rest-fomenting-unrest-is-restfulness-a-semantics-game-why-does-rest-require-statelessness/), I had commented on some of my thoughts on dealing with some of the constraints prescribed by REST one of them being statelessness. Some of those thoughts continued to churn in my mind, and I had a few helpful interactions along the way which led me to what I believe to be the "Aha moment" on statelessness in REST. That doesn't mean I'm necessarily right, feel free to comment on my thoughts in case you believe any differently or have a nuanced opinion.

**Background**

Per [section 3.4.3](http://www.ics.uci.edu/~fielding/pubs/dissertation/net_arch_styles.htm#sec_3_4) of Roy Fielding's dissertation



> The client-stateless-server style derives from client-server with the additional constraint that no session state is allowed on the server component. Each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is kept entirely on the client.



The same thought is further commented upon in [section 5.1.3](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_1).



> We next add a constraint to the client-server interaction: communication must be stateless in nature, as in the client-stateless-server (CSS) style of Section 3.4.3 , such that each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client.
...
Like most architectural choices, the stateless constraint reflects a design trade-off. The disadvantage is that it may decrease network performance by increasing the repetitive data (per-interaction overhead) sent in a series of requests, since that data cannot be left on the server in a shared context. In addition, placing the application state on the client-side reduces the server's control over consistent application behavior, since the application becomes dependent on the correct implementation of semantics across multiple client versions.



**The potential confusion areas**

Given the clear prohibition of storing client state on server, there are some typical idioms which do get challenged as REST unfriendly. eg.



	
  * Logging in to obtain a session token, which is subsequently a validation of an authentication status

	
  * Binding additional attributes to such a session token on the server side with variables such as :
             
	          
    * Computed and cached access privileges of the user

	          
    * Conversational state eg. fields entered on page 1 of a two page form

             
        



The question there is are all he above scenarios inconsistent with REST ? Many articles seem to suggest that that would indeed be the case. as an example one of them is [Ajax and REST, Part 1 Section: Violating the "stateless server" constraint](http://www.ibm.com/developerworks/xml/library/wa-ajaxarch/#N100F0)

**My thoughts**

Clearly in the above example computed data such as access privileges, or for that matter user's time zone information are available to the server even if these are not in the session so long as the server has the user id. So long as a user id is being supplied (or a proxy such as the session token id), such data being stored in the session is simply a server optimisation and thus does not violate REST guidelines since this is application state and not conversational state.

To the extent the data is storing conversational state such as the fact that the user has authenticated himself and the fields that the user may have entered in page 1 of a two page form, such is incompatible with REST guidelines, and if such practices are adopted the resultant design may not be termed fully REST compliant. 

So the existence of a user token does not make the design REST "uncompliant". Storing conversational state in a user session usually associated with such a token does.

One way to ask if a data is conversational or application, may be to ask oneself, that can the request be satisfied properly if it was accidentally routed to a different server in a situation where the cluster was configured for session affinity. In a REST compliant design, the other server will have a capability to still be able to service the request. However this requires the presumption that the cluster is configured for session affinity and session state is not available to other servers. Should one configure the cluster with shared and distributed sessions, the cluster will be able to service the requests even when the APIs were not REST compliant. 

The one thing that does leave me a little uneasy is that to be fully REST compliant one will need to always supply a userid / password with each API call instead of a server token. That does have implications on security, implications that I am not entirely comfortable with.

_Update :_Surya in a [comment in earlier post](http://blog.dhananjaynene.com/2008/11/rest-fomenting-unrest-is-restfulness-a-semantics-game-why-does-rest-require-statelessness/#comment-5656) refers to the fact that authentication need not be done through the URI but through the headers or through protocols such as OAuth. So what that leaves me with is the assessment that existence of a token or a session object is not a violation of REST but storage of conversational state in the same is.









