---
date: '2008-12-24 02:00:01'
layout: post
slug: will-rails-go-the-struts-1-way
status: publish
title: Will Rails go the Struts 1 way ?
wordpress_id: '312'
categories:
- architecture
- java
- programming
- ruby
- software
tags:
- frameworks
- java
- merb
- rails
- ruby
- struts
- webwork
---

Just saw the story [Merb gets merged into Rails 3](http://weblog.rubyonrails.org/2008/12/23/merb-gets-merged-into-rails-3). Quickly reminded me of another story that played itself out similarly around Nov. 2005 ie. [Webwork Joining Struts](http://blogs.opensymphony.com/webwork/2005/11/webwork_joining_struts.html). And there really are some parallels indeed. (Disclaimer: While I can claim to know struts quite well and webwork exceptionally well, my knowledge of Rails is limited and that of Merb is rather superficial)



	
  * Struts 1 was the grand daddy with a massive market share, whereas Webwork 2 was the upstart

	
  * Struts 1 established its dominance by being far ahead of its competition in its time. Webwork 2 was the exceptionally well designed, highly compartmentalised, granular and flexible MVC framework which clearly established a strong design lead over Struts 1

	
  * Struts 1 was monolithic, Webwork 2 was far more fine grained and pluggable

	
  * Struts 1 primarily had a single controller servlet, Webwork offered more fine grained controllers


In the post [Rails and Merb merge](http://yehudakatz.com/2008/12/23/rails-and-merb-merge/), Katz explicitly refers to the Struts / Webwork 2 Migration. However there was one big difference, while Webwork 2 made some movement towards being just a little bit more struts like during the migration, for all practical purposes the next version after Webwork 2.2.x became Struts 2, and Struts 1 got confined to legacy. In this case it just seems unclear how Rails and Merb will move towards each other. As a designer, I really cannot imagine easily merging two frameworks and have the best of both of them be reflected in the result (it is exceedingly tough but feasible). There is a practical driving necessity of ensuring consistency which is likely to favour one of the two frameworks. I am really left with this feeling that one of them will dominate the other in the end - and this whole effort will be quite unnecessary if Rails has to dominate eventually. Definitely an interesting space to watch.
