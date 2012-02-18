---
date: '2009-02-12 14:31:42'
layout: post
slug: an-experienced-programmer-doesnt-use-solid-as-a-checklist-he-internalises-it
status: publish
title: 'An experienced programmer doesn''t use SOLID as a checklist - he internalises
  it. '
wordpress_id: '450'
categories:
- programming
- software
tags:
- bob martin
- design
- design principles
- ood
- quality
- solid
---

Reading [The Ferengi Programmer](http://www.codinghorror.com/blog/archives/001225.html) by Jeff Atwood really made me quite concerned. Here's clearly an opinion which to me seems not grounded in sustained experience in applying the principles and is likely poor message going out to junior programmers.

In the post the author treats [SOLID principles](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod) by Bob Martin as a ruleset that programmers apply from time to time. Once you get yourself into that frame of mind it is difficult to then contest the rest of the post. I would want to re-present the same topic with a different frame of mind. 

SOLID principles are principles that you learn in your early days as a designer. These are formative stages when you are honing your skills and attempting to review your designs in terms of specific checklists of items to go through as a mechanism of validating your design. But each time you apply them, you internalise a part of them, and soon in 3 or more years of regular application, their application becomes internalised and ingrained. At this stage you might even well forget that they exist, since you apply them subconsciously, day after day, time after time, and sometimes referring back to them only when debating or reviewing your designs with other designers. 

The analogy to the painter is in the post referred to : [Are You Following the Instructions on the Paint Can?](http://www.codinghorror.com/blog/archives/000568.html) is also quite instructive. The instructions on the paint can are for one time painters, hobbyists, amateurs etc. No experienced painter is likely to be reading them since he's probably internalised them. But each seasoned painter would want every junior painter to learn the instructions and the costs of not following them before stepping up to deciding whether and when not to follow them. He is unlikely to teach a new painter in the making - follow the instructions that make sense. 

Sure you do make tradeoffs at times in design. Most people tradeoff guidelines in all spheres. However the keyword is to understand that you are breaking a guideline and then do so explicitly knowing its costs fully well. 

My big difficulty with the post is that it is an advice which may do more harm to junior programmers than good. It might encourage them to make tradeoffs before they learn the cost and implications of making the tradeoffs. And it might set themselves away from a path that requires careful and judicious application (which requires a lot of effort in the early days) and helps them internalise the principles. 

I would not recommend the post I refer to to any junior and upcoming programmer. My advice is as follows. If you have grown to a stage where you are applying these rules implcitly - don't worry, you have the experience on your side to generally make the right judgement calls and you are likely to anyway apply them under most of the cases. In such a situation, this post and the one it refers to are probably inconsequential to you. If you are at a stage where you still need to review your design with respect to the SOLID principles (or other appropriate design principles) - please take your time to apply the principles, learn if you are breaking them, understand the costs of doing so (I would recommend that involve a senior programmer / designer in the process) and then by all means make the best judgement. Principles distilled over time and experience should be adjusted preferably by those who understand the cost and implications of doing so, the rest should strive to reach that state first.

To clarify, the reason why I upfront made the statement "seems not grounded in sustained experience in applying the principles", is that those who have internalised them hardly every feel the burden (if at all) of applying them, and are unlikely to ever ever treat it as an explicit checklist, and they seem like checklists to those who haven't internalised them. Precisely the audience to whom you want to craft a more careful and nuanced message.
