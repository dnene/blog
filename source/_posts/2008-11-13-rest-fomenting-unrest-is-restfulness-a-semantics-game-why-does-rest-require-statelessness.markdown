---
date: '2008-11-13 06:56:56'
layout: post
slug: rest-fomenting-unrest-is-restfulness-a-semantics-game-why-does-rest-require-statelessness
status: publish
title: 'Fomenting unREST : Is RESTfulness a semantics game ? Why does REST require
comments: true
  statelessness ?'
wordpress_id: '154'
categories:
- architecture
- programming
- software
tags:
- rest
- restful
---

**Background :**

Roy Fielding recently wrote : [REST APIs must be hypertext-driven](http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven). It is an excellent writeup which actually focuses on _What REST is NOT_ and is written in the context of [SocialSite Web APIs](http://wikis.glassfish.org/socialsite/Wiki.jsp?page=FinalizeRESTAPI) which are an implementation of the[ OpenSocial Restful Protocol](http://www.opensocial.org/Technical-Resources/opensocial-spec-v081/restful-protocol). If you are interested in understanding what REST is in any substantial way I highly recommend Fielding's post, and if you can spare some time do read up the comments and also [Section 5](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) of his dissertation or perhaps the whole dissertation.

The essence of the post is found in the statement :



> What needs to be done to make the REST architectural style clear on the notion that hypertext is a constraint? In other words, if the engine of application state (and hence the API) is not being driven by hypertext, then it cannot be RESTful and cannot be a REST API. Period. Is there some broken manual somewhere that needs to be fixed?



I agree with it entirely, and that is not what makes me uncomfortable. Its just that the rules of figuring out what is compliant with REST are a little porous, and the whole requirement of statelessness I find a little orthogonal to the driving principles of REST design.

**Do semantics decide what is RESTful ?**

Roy essentially states he has no difficulties in people following non REST approaches, but suggests that if something is called or classified as RESTful, then it must adhere to what he has described as characteristics of REST architecture, and if it doesn't, thats allright, just don't call it RESTful. I think this is a perfectly fair stance. But it does beget a question, what do I call an architecture style that is strongly inspired by REST, fairly close to REST but does not actually meet all the expectations that Roy laid out. But an even more important question - apparently there is some amount of semantic jugglery that one can conduct to make an architecture seem RESTful. Does it in such a situation become RESTful ?

I think REST as a architecture style is great, it really simplifies things in many ways, but (_oh! the horror of it_) I cannot seem to agree with it entirely. However that is my individual perception and opinion and not the topic of this post. The topic is the fact that I feel I am modeling something that does not meet the REST requirements in spirit but yet can argue my way through it in letter. If anyone of you gets the same feeling or has figured out a way out of it do post a comment below.

**Setting the context. Quotes from Roy.**

A few quotes from Roy before we get into the details. As per the post, clearly REST wasn't intended to be a RPC substitute. 



> I am getting frustrated by the number of people calling any HTTP-based interface a REST API. Today's example is the SocialSite REST API. That is RPC. It screams RPC. There is so much coupling on display that it should be given an X rating.



and from section 5.1.3 of the thesis on the matter of statelessness,



> We next add a constraint to the client-server interaction: communication must be stateless in nature, as in the client-stateless-server (CSS) style of Section 3.4.3 (Figure 5-3), such that each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client.



and from section 5.2.1.1 of the thesis, with regards to what a resource is :



> The key abstraction of information in REST is a resource. Any information that can be named can be a resource: a document or image, a temporal service (e.g. "today's weather in Los Angeles"), a collection of other resources, a non-virtual object (e.g. a person), and so on. In other words, any concept that might be the target of an author's hypertext reference must fit within the definition of a resource. A resource is a conceptual mapping to a set of entities, not the entity that corresponds to the mapping at any particular point in time.



**A sample application and REST like API** :

Let us take a simple example - an ATM machine. A simple use case, user enters bank atm card, machine prompts for pin, user enters pin, machine shows menu, user selects make card payment, machine asks for card number and amount, user enters card number (destination account) and amount, machine transfers funds. Simple enough ?

Now lets build this using a restful (or not so restful) API. 
**Note:** _Purely for writing and reading simplicity I shall represent all HTTP requests using GET semantics._

_1. User enters card_ : Client level activity. No API required here. 
_2. Machine prompts for pin_ : Client level activity. No API required here
_3. User enters pin (and presses a OK / Submit button)_ :
Here's where we need to send the following data to the server - card number and pin. As per REST, we should always access a resource on a server. So here's how I am going to design the api for that 


    
    
    http://atm.bank.com/session?cardnumber=123456&pin;=1234
    



This will initialise a new session, and the response shall return me a new session. The response xml shall be :


    
    
    <session state="valid" authenticated="true" id="999" menuurl="http://atm.bank.com/menu/999"></session>
    



_4. Client requests menu :_
The response was hypertext and had the URI for the menu which the client shall now invoke. Note that in the example below, 999 is the session identifier, and it would be possible to pass 999 as a parameter rather than as a segment in the URI itself (take your pick). 


    
    
    http://atm.bank.com/menu/999
    



Here's a sample menu that might be expected


    
    
    <menu>
      <action url="http://atm.bank.com/withdraw/999" name="Withdraw"></action>
      <action url="http://atm.bank.com/deposit/999" name="Deposit"></action>
      <action url="http://atm.bank.com/cardpayment/999" name="Make Card Payment"></action>
    </menu>
    



_5. The user selects card payment. _


    
    
    http://atm.bank.com/cardpayment/999
    



The system detects that the required fields are not supplied responds with the necessary information that the client needs to supply


    
    
    <cardpayment status="not-initialised" id="" target="http://atm.bank.com/cardpayment/999">
      < field="destinationaccount" name="Card Number" type="Integer" currentvalue=""/>
      < field="amount" name="Amount" type="Decimal" currentvalue=""/>
    </cardpayment>
    



So the user now enters the two fields and submits the data


    
    
    http://atm.bank.com/cardpayment/999?destinationaccount=98765&amount;=111.22
    



The system verifies that all is OK (ACL, valid destination account, adequate balance exists etc.) and performs the transfer successfully


    
    
    <cardpayment status="done" id="777" menuurl="http://atm.bank.com/menu/999">
    



**Analysis :**

Either you might have found the above API allright, or you might have one or more of the following issues :
1. The session id is a state tracking identifier. REST is stateless. Clearly this is not RESTful
2. The card payment was clearly a call to a remote procedure which could otherwise be represented as : transferMoney(sessionId, toAccount, amount)
 
One of the early things I learnt was that when designing transaction processing systems, many user requests result into some activities that need to be triggered on the server side (as in the transfer funds to credit card above). Intuitively it is a procedure, and because one is in the client server world it is a remote procedure. So how can it be RESTful ? Again, from what I have learnt, its easy - just convert the verb into a noun and the remote procedure is now represented as a resource. How so ? Simple - instead of transferMoney procedure, I now have a cardpayment resource which I activate. What else changed, the return format of the procedure (oops resource) is now a hypertext document which contains necessary state information about the transfer and hyperlink to the other associated resources. Voila ! We are RESTful. But is it still RPC ? The essence of RPC has not gone away.

But isn't this a stateful architecture. By all means yes. Most certainly it is. But here's how I am going to argue my way out of it - I actually created a resource called session, and subsequently was simply using it (I could use it by making a reference to it as a segment in the URI as I did above, or by passing in the id as a parameter, or by passing in a addressable URI for the session itself as a parameter eg. http://atm.mybank.com/session/999). This session has associated with it on the server side the card number and account number which I can use readily. Stateful ? Yes. But it is still consistent with the overall Resource Oriented approach of REST where the session is but one instance of that generic thing called a resource. Thats semantic skullduggery you might argue. Quite right .. But I couldn't find a way to blow a hole in that argument. 

However there is a small hitch in the above argument. The REST dissertation explicitly excludes the session from being treated as a resource (see the last line of Roy's quote related to statelessness above). I still haven't quite gotten over the asymmetry of treatment to session data and other resources. I cannot quite understand why stateless = RESTful and stateful = RESTless. It is not as if one ends up specifying all the resource data in all the cases. For all the associated objects (eg the Account object corresponding to the ATM Card number in the case above), one simply passes in the resource URI / identifier, and the server side then goes fetches the additional data as might be required to complete the transaction. So if one passes an invalid card number, the service will ensure that the appropriate validations will reject the transaction. So why can one not treat the session as a resource and have it being treated symmetrically with all the other resources. Why single it out ?

**The need for statefulness** :

Here's one reason why I would really like to have statefulness - security. I do not want to keep on transferring again and again with each API call the ATM card number and the PIN, since these do not change frequently (if ever). I would like to transfer the same as few times as possible (notwithstanding any additional encryption or obfuscation I build on top of them). There have been cases where sniffing data over the wire has been suspected, and I would like to minimise the likelihood of a breach by minimising the number of times critical data is shipped over the wire. 

Here's another reason why I like statefulness - efficiency. In the above example, the (source) account number was never mentioned or passed, but yet I am sufficiently aware, it is actually required for every single non trivial activity. So why not just load it when the session is initialised, stuff it into the session and not worry about having to retrieve it from the database. 

Clearly it is very easy to overuse and abuse statefulness, and I try to keep state data to the absolute absolute minimum, but completely statelessness is something I have yet to feel comfortable with.

Finally - and this is an argument more in principle than in practice, the need for statelessness is context specific. If I have a hefty server, and expect only a handful of active sessions at any point in time, it is possible to argue that stateless architecture simply may not be required at all in such a particular context. But to then imagine that the otherwise RESTful architecture is now not so makes me restless.

**Final words** :

In this particular post I am going to assume that those with more knowledge of REST and RESTfulness than the limited amount I carry, are going to blow holes in my semantic jugglery, and maybe prove that there is no amount of semantic jugglery which will make the architecture RESTful. That does beget the first question I asked - If an architecture style is largely inspired by REST but makes a few deviations what should one call it ? Maybe RESTlike or RESTspired but you might come up with a better word. I am certain I am going to be far more concerned about whats required in the given context and what is most appropriate for the users and the sponsors rather than whether the APIs are pure REST APIs. How about you ?


