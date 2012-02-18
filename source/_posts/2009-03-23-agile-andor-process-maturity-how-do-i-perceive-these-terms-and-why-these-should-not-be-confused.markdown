---
date: '2009-03-23 16:51:02'
layout: post
slug: agile-andor-process-maturity-how-do-i-perceive-these-terms-and-why-these-should-not-be-confused
status: publish
title: 'Agility and/or Process Maturity : How do I perceive these terms and why these
  should not be confused'
wordpress_id: '575'
categories:
- agile
- management
- software
tags:
- agile
- apmm
- process maturity
---

[Agility](http://www.agilemanifesto.org/) and Process Maturity are two very different terms to me. I have been rather pleased with the results I have found after having started working with agile methodologies more than 4 years ago. That makes me a candidate for being a biased blogger. But it still does allow me the ability to state my views clearly on the topic after that explicit disclaimer.

Process Maturity (especially CMM and ISO oriented maturity) focus on ensuring consistent, measured and self improving quality of output. This is achieved through multiple checks and balances, and detailed documentation of processes, and regular audits to identify points of weakness and their redressal. At least to the extent I have been exposed to them, people are looked at as cogs in the assembly line (replaceable and faceless) and focus is on ensuring that the assembly line works at a consistent pace even as people move around from job to job (or from assembly line to assembly line). Thus the focus is on setting expectations at modest levels and ensuring that these expectations can be met even as various events occur which introduce risk into the process, since the process methodology and documentation is geared to help address many of these risks. 

Agility on the other hand probably stemmed from some of the concerns around the non-people-centricity and heavy-documentation-focus of these process maturity models. It instead ended up focusing on Individuals and their Interactions and on accepting the necessity of Change and thus adopting it wholeheartedly as an aspect that is inevitable and thus best accepted as a feature rather than a bug. Thus rather than slowing down the delivery, it attempts to substantially speed it up by removing many process steps which may seem to be impediments to progress. While substantially reducing most control and measurement points such as documentation, reviews, audits, they replaced them with steps which allowed quality to be maintained by introducing higher skills and discipline into the people (test driven development, pair programming, daily scrum). 

Thus if a development team tells me they follow a process maturity model, assuming they are being completely honest in that statement, it tells me that as a customer I can expect them to set a pace and generally largely stick to it with incremental improvements while shielding me from having to deal with many of the internal risks they battle with. 

If on the other hand a development team tells me they follow the agility model and again assuming they are being completely honest, it tells me they have a lot of respect in their own capabilities and to their ability to adaptively deal with internal risk through better earlier risk detection and redressal (Test Driven Development / Continuous Integration etc.) rather than through process oriented controls such as Documents / Audits / Reviews etc. I would also generally expect a much much higher productivity and quality from the team but with a higher statistical variance.

Ceteris Paribus, as a customer I would feel that the Agile Team is likely to introduce more schedule risks in situations of higher person turnover or lesser developer disciplines, while the team that focuses on Process Maturity is more likely to introduce schedule risks in situations of changes in requirements or in situations where a very quick time to market is called for. 

What is obvious to me is that these methodologies have been defined with very different attitudes. If we play a "one word that comes to your mind" game with the terms, its "power" with "agility" and "control" with "process maturity". Both are important and desirable and play a role, but the slider is positioned differently in each of the cases. 

It has been rather obvious that agile processes have been occupying a greater mindshare over the past 5 years (the same thing happened with process maturity methodologies in the decade before that). I had been concerned that, as with many other trends in Software, [the word Agile might also start getting abused ](http://blog.dhananjaynene.com/2009/01/software-it-terms-in-early-stages-of-abuse-or-ripe-for-abuse/#agile) where people start using it quite differently, to leverage on its momentum and the mindshare. It has been happening for a while, but the article [Strategies for Scaling Agile Software Development The Agile Process Maturity Model](http://www.ibm.com/developerworks/blogs/page/ambler?entry=apmm_overview) just blew me off. I have only the greatest respect for Scott Ambler and especially for all his writings on Persistence. But in this article all the agile methodologies have been clubbed together as "Level 1 development" and many of the rather heavy weighted processes some of which probably led to the formation of the agile alliance in the first place are listed in the "Level 2 (disciplined) delivery". I must clarify that agile software development covers all aspects of lifecycle including testing and measurement and agile software methodologies deliver as well (contrary to what the article "seems" to suggest) and rather than building discipline into processes, reviews and audits it builds discipline in the people that form a part of that process. 

Since agility and process maturity are based on different attitudes I can't see how one says I am going to put both the things together unless one intentionally and quite deliberately repositions the slider at a level with offers lesser power than agility and also dilutes process control compared to process maturity. But thats not the impression I got from the article. It seems to suggest that one can take these "level 1" agile processes add "level 2" disciplines to it and then add some uncertain "level 3" elements to them to make things better than where they stand today, something that I am a little skeptical about.

So when a development team tells me they are following the "agile process maturity model (APMM)" I am likely to believe that they are either (a) too confused, (b) want to appear to leverage the agile momentum while using the strict control regime or (c) successfully figured out a way to move their slider somewhere in between with the risk that they lost some of the best elements of the two methodologies while attempting to lose the worst.

Having said that, it will be interesting to read Scott's future articles on topic and to follow this trend. I have been proven wrong in the past, and it could happen again for me to have to eat my words. But moving that slider between power and control to attempt to achieve the best of both seems a little risky, and you can be sure I am unlikely to be an early adopter for the fear of getting severely burnt.

**Update :**I forgot to add that amongst the many items listed in Level 2, those such as (throwaway) sketching, whole team, test-driven development (TDD), continuous integration, daily standup meeting are accepted aspects of typical agile methodologies whereas those in the level 2 methodologies which agilists often avoid as actively managed and maintained charts are not even listed eg. in case of RUP - use case diagrams, class diagrams, sequence diagrams, state diagrams. So what is the contribution of other methodologies such as RUP in this case is largely unclear. I do not know the other Level 2 methodologies so can't comment on the same.
