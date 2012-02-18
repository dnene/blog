---
date: '2011-10-12 03:15:32'
layout: post
slug: which-risk-would-you-manage-what-would-you-want-to-prove-programming-languages-and-type-systems
status: publish
title: Which risk would you manage? What would you want to prove? Programming Languages
  and Type Systems
wordpress_id: '1202'
categories:
- architecture
- programming
- software
---

Debates across programming languages and type systems are not new. And this post does not attempt to shed new light on these (though it is hardly an un-opinionated view) 

Yet one point that keeps on bothering me time and again. That the lens used to visualise the many issues around these help clarify, magnify but lose the bigger picture. This post suggests a broader view to be applied. 

Every argument refers to either improved benefits, lower costs or better contained risks. And in fact much of the arguments focusing proving the correctness of the code, or compiler ensured verification and thus the resultant guarantees, or the extent of unit test required to cover a wide array of differently typed inputs all boil down to risk. 

The risk of code not behaving precisely like the way the author intended it to behave. 

Yet there are other risks. 

* The risk of spending too much time building something you eventually realise is either not required.
* The risk of not being able to provide rapid feedback to the product team, OR The risk of the customer not being able to partake and participate in your newest features to provide you critical inputs
* The risk of building a fantastic software, but a software that in hindsight needed to be very different.

When rewriting a legacy system, you often know exactly how you would like things to be rebuilt. When having adequate cash flows from alternate revenue streams, which can absorb product development costs, the life of your company may not depend upon the particular development in progress. When a part of a very large enterprise where anyways tons of internecine political empires rule the roost, a particular product / project development may hardly influence overall corporate risk. 

In each of these situations - the risk of the software being developed is largely the same as the risk of the software getting delivered. Thats when risk is measured in terms of the software not meeting its feature, performance or delivery schedule goals. Thats when risk management becomes very important. Thats when provability, robustness etc. become important metrics for predictively containing risk. 

Yet, not all environments have that luxury. Startups especially. Those that are dependent strongly on a piece of software - bet the farm on that software. Many actually have only a blur of an idea - but require tremendous market feedback to refine the vision. Yet others are operating on fixed constraints in terms of budgets before they decide whether the investments they made are worthy of further investments or whether to cut the losses. The risks here are very different. 

In such situations - the risk to manage is not the risk of software correctly meeting its specifications. Its the risk of the software specifications not correctly meeting the market expectations. The real risk here is agility. Can you create the minimal functionality needed for feedback in the quickest manner. Can you act on the received feedback to adapt in the most competitive manner. In short can you iterate. Again. And again. And again. Fast enough before the market throws you out, or your competitor eats your lunch or your budget runs out. Work backwards from these goals. And you'll find the right type system. And programming language. For you. 

Programming languages and type systems are but vehicles to goals. Work backwards from your customer satisfaction perspective, your particular organisational constraints (or lack of them), your imperatives to iterate, and your time to market pressures. Work backwards from these to understand what exactly your company's life depends upon. And of course factor in your current strengths (languages known / experience levels etc.). The right type systems and programming language should be relatively easy to decide. 

And yes, provability is important. So long as you know exactly what you wish to prove first. The software or your business model.

Oh, lest you feel confused which type systems I prefer, I am ambivalent. They exist because they all have a role to play. But I do believe each context has one right choice. Just make the right choice for your context. And yes the programming languages I program in currently are Python and Scala. And yes, I have found in my experience dynamically typed languages help very substantially when fast TTM or iteratibility is needed (a view not everyone shares)

