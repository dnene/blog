---
date: '2009-11-25 20:31:49'
layout: post
slug: rinse-and-repeat-with-tdd
status: publish
title: Rinse and Repeat with TDD
wordpress_id: '921'
categories:
- agile
- programming
- software
- testing
tags:
- tdd
---

I must confess having had a awed love relationship with TDD ([Test Driven Development](http://en.wikipedia.org/wiki/Test-driven_development)). This is the style where you first write the test cases and then write the code to make them pass. I've always felt awed by it. I have always firmly believed that it is the right way to offer sustainable quality. As someone who hasn't been particularly awed by separate QA teams, I have always imagined TDD to be the silver bullet to offer controlled, sustained, high quality. Well, the silver bullet is a bit of an exaggeration, but sounded nice while I was on a roll :). A number of times I wrote automated test cases post facto. But when the rubber met the road - I didn't ever implement TDD. 

Its probably a function of style. I like to write a piece of code and then keep on testing it against different scenarios and often keep on remoulding it substantially. My focus is at this stage on the code structure - the design aspects. Trying to do it with TDD always created a mess. The amount of intense refactoring I do always meant I soon got tired of also simultaneously refactoring the test cases and gave up on them soon. 

Anyways, I finally think I found a formula that works - at least for me. I do not write production code in the first pass. I just write prototype code. I keep on playing with it, shaping it, challenging it, cajoling it, recasting it. Until I'm satisfied with the overall structure and internal design. Now when thats done, I rewrite it. Completely. Umm, I copy/paste maybe 40-50% of the code. But this time I write it on a completely new fresh set of files. And I write the test cases before I write the code. I find I can now really write tons of test cases and code quite rapidly. And with the tremendous satisfaction that at the end of the day, I now have the control harness to be able to keep on adding new features without having to continuously worry if I broke something else. With substantially detailed test cases, now I can actually feel quite confident that if I did break something - I'll get to know it. Not next month, not next week, not even tomorrow - in the next few minutes.

So this approach seems to be working for me - the initial efforts have delivered great results and I'm quite satisfied. Now just wondering what to call it - for lack of a better term I shall term it "Rinse and Repeat with TDD".

So if TDD works for you - great. You have one of the most important coding disciplines nailed down. But if you're wanting to try to do TDD but haven't been able to successfully implement it - you may just want to check out the Rinse and Repeat with TDD style. Who knows, you might find it useful too.
