---
date: '2011-04-22 15:11:20'
layout: post
slug: the-cloud-just-got-stronger-even-as-aws-went-down
status: publish
title: The cloud just got stronger, even as AWS went down
wordpress_id: '1082'
categories:
- architecture
tags:
- aws
- iaas
- outage
---

So some parts of the AWS EC2 specifically related to EBS were [non responsive or down yesterday](http://www.bloomberg.com/news/2011-04-21/amazon-com-says-some-web-services-for-businesses-not-available.html). This was of course also supposed to be [judgement day after skynet became self aware](http://blog.zap2it.com/pop2it/2011/04/april-21-2011-skynet-attacks-we-explain-the-terminator-date-confusion.html). Links between facts and fiction aside, the number of sites that got impacted was just quite large and at least some (not most) started wondering if cloud indeed is the way to go.

I think the cloud just became stronger. Even as many services go impacted, some didn't. As an example Netflix was able to continue with its services. See Slide 33 in a recently conducted [presentation](http://www.slideshare.net/adrianco/netflix-in-the-cloud-2011) of its architecture. I am certain there were many others who also were able to ride the storm. Since they had not just planned for redundancy of equipments but the architecture had accounted for disaster recovery (redundancy of locations). And many others who got impacted and put their customers through undeserved anguish still perhaps learnt a lot. A lot that they could've done differently. 

What is interesting about "a lot that they could've done differently" is that cloud infrastructure as a service opens up a few more options to ensure high availability. Amazon AWS apparently has 5 regions and the US East region which got affected has at least four availability zones. It was one of that availability zone which got substantially impacted. Though in fairness the impact seems to have spread into other availability zones, something that shouldn't ideally have happened. I am sure there probably were a lot of things AWS probably learnt and perhaps would do differently. But the same applies now to AWS customers as well. Some may choose to lose faith in the cloud. But many others might choose to realise that cloud infrastructures have the potential to offer so much more redundancy and options to high availability. Its just that one needs to realise that cloud data centers are not infallible, and after being aware of all the redundancy options, one just needs to design the right way to leverage them. 

And how does one leverage them ? AWS has multiple availability zones. An application should ideally leverage at least two. If you read the Netflix presentation I referred to, Netflix apparently uses three. Do not assume the servers will not go down. Assume it is possible that at least one availability zone could go down. Make sure you have the systems to quickly activate, systems in the alternative availability zone. For that you will need to find ways to keep data current across availability zones. Also find ways to ensure you have the ability to quickly switch to and fro between availability zones. More advanced options could include concurrently active systems across availability zones or those spread across AWS regions or even between AWS and other vendors.

There's little if any learnings here that are specific to AWS. They are indeed applicable in general to cloud based infrastructure as a service providers. But unless you are a part of a Fortune 500 or equivalent it is very unlikely that your internal infrastructure will offer as many options for redundancy as at least some of the larger infrastructure as a service providers could. Which is why I believe even if some may choose to switch back to the seeming comfort of private infrastructure, many currently using private infrastructure are likely to look at today's events and realise that the public cloud actually offers so many better options for building highly available systems. The issues are not yet fully resolved. And many customers still perhaps are not being served. And while one hopes not, it could so happen that there could be further disruptions. But I find them really aspects of dealing with a learning curve as one transitions across class and scale of infrastructures. Ignoring the short term pain, and looking at it a little bit in the longer term, whichever way one really looks at it, I think the cloud just became stronger.
