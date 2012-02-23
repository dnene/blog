---
date: '2009-08-14 14:05:45'
layout: post
slug: crud-is-not-only-good-for-but-is-the-only-consistent-way-to-build-rest-over-http
status: publish
title: CRUD is not only good for, but is the only consistent way to build REST over
comments: true
  HTTP
wordpress_id: '787'
categories:
- rest
tags:
- crud
- interface
---

This is to comment on a perception forming that REST encourages exposing basic data elements through CRUD and that it encourages development of dumb applications (applications with shallow business logic).

Apart from some tweets I saw on the topic and some twitter conversations, the blog posts which perhaps set off the thought were



	
  * [CRUD is bad for REST](http://dobbscodetalk.com/index.php?option=com_myblog&show=CRUD-is-bad-for-REST.html&Itemid=29) by Arnon Rotem-Gal-Oz

	
  * A summarisation of the same on InfoQ : [Is CRUD bad for REST?](http://www.infoq.com/news/2009/07/CRUDREST)

	
  * [Can We REST For A Minute? 6 Lessons From The CRUD vs. Hypermedia Debates](http://hinchcliffe.org/archive/2009/08/06/17119.aspx) by Dion Hinchcliffe


The underlying fear and rationale for these posts makes a lot of sense - the fear of creating real dumb passive and shallow applications. I submit, that the problem however is not CRUD - it is resource identification and scoping, and CRUD is not only good for but is the right way to build intelligent, active and deep applications.

**CRUD supports Uniform Interface :** The primary reason why CRUD gets used is because it supports a uniform interface. At the end of the day, a consistent Create/Read/Update/Delete or POST/GET/PUT/DELETE interface makes things easy. It makes things easy for the development team because of the consistency it introduces in their applications. It makes things easy for the clients who have a simple and consistent interface to deal with. At the interface level CRUD breeds consistency, and at the risk of broad generalisation, consistency is good.

**So why do we end up creating shallow applications at times with REST ? ** CRUD in general works with simple forms built on simple tables. Quite often this style of programming gets elevated into simple forms over simple domain objects. Standardised CRUD helps a lot at the lower end of application development and most database driven application developers are likely to have at some stage in their early development life attempted to build a small CRUD library or framework to help themselves substantially. The reason why we are likely to be ending up creating shallow applications is not because we apply CRUD, but because we continue to apply CRUD on tables or simple domain objects. And therein lie the distinctions




	
  * _REST is not about CRUD on tables - its about CRUD on resources_

	
  * _CRUD is the interface - not the implementation_



I attempt to bring up the difference in the example that I detail below.

**Simple Account Transfer Example**

Lets say we want to build the software to transfer amount X from account A into account B. Lets further specify that a transfer is not effected immediately and requires one more explicit approval. Lets also specify that while a transfer is waiting to be approved, it could be amended. Thats the simple scenario that we shall deal with.

In order to implement this, we shall define a datastructure / table / object for Account which shall contain a field called balance. Further there shall also be Transfer table / object which shall contain the fields sourceAccount, destinationAccount, amount and status. The possible status values shall be Initiated and Completed.

In a simple service oriented application we shall perhaps have a transfer service. Ignoring error handling, SOA wrapping etc., the service interface will probably boil down to the following equivalent Java interface.


    
``` java    
    public interface TransferService
    {
        public Long transfer (Long sourceAccountId, 
                                    Long destinationAccountId, 
                                    BigDecimal amount); 
        public Transfer get(Long transferId);
        public void amend(Long transferId, 
                                 Long sourceAccountId, 
                                 Long destinationAccountId, 
                                 BigDecimal amount);
        public approve(Long transferId)
    }
```
    



Lets think of these might get modeled in a REST environment. The important thing to remember is - don't think about services or functions or methods - think about what are the resources you choose to expose using a simple CRUD interface.

```
# The following creates a new transfer. The returned data shall include
# the URI of the new transfer, and the URI to approve it
POST /transfer 
# The following retrieves the status of a current transfer. If it has not
# been approved the returned data shall include the URI to approve it.
GET /transfer/${id}
# The following modifies the transfer. The returned data shall also
# include the URI to approve it
PUT /transfer/${id}
# The following approves and further processes the transfer. It shall
# return the URI for the transfer
POST /transfer/${id}/approve
```

While most of this seems all right - what sticks out like a sore thumb to me is the approve URI. Its just so SOAish / RPCish. Plus at least the way this particular interface has been implemented, there is no way to access  the approval specific information, without actually accessing the transfer. Hence I suggest that we define a new resource TransferApproval to account for the same.

```
# The following creates a new approval. If successfully executed
# the transfer is complete and no future amendments or approvals
# are allowed. the returned data shall include the TransferApproval
# URI and the transfer URI
POST /transfer/${id}/approval
# The following gets an existing approval
GET /transfer/${id}/approval/${approvalId}
```

Please note that the "${approvalId}" at the approval URI simply wasn't required - since there exists a 1-1 relationship with the transfer. I just included it for easier understanding. If I had to implement the functionality as is I would choose to skip it however if I knew I would very soon need to build in multi-stage approval (as in most banking systems), I would keep it so that each approval against a transfer can also be listed.

But the really interesting method above is the POST. This is a seemingly simple new (in RDBMS parlance) insert into TransferApproval table. But if you are building a REST service, you might be tempted to encourage your clients to not only create the new TransferApproval resource, but also go back and update the Transfer table to update a status to Approved. That would be a smell. Once the POST on the approval is processed, all side effects on other tables should be handled while servicing the POST request. In other words the POST request is not just an insert - its an insert with an associated trigger to conduct all the necessary downstream processing. And its essential one looks at request servicing in this manner so that CRUD can be used effectively. Servers should be designed this way, and clients should anticipate it and we should be on our way to build non-shallow applications.

So finally - _CRUD is good. It makes things easy for the clients. Stick to CRUD._ Just remember that it is CRUD on resources and not on tables, and the resources shall handle all the downstream changes necessary so that you don't have to. And finally _CRUD is the interface, not the implementation_.

Note: Some might notice that this post is just a much more detailed elucidation of one of my earlier posts - [REST is the DBMS of the internet](http://blog.dhananjaynene.com/2009/06/rest-is-the-dbms-of-the-internet/).


