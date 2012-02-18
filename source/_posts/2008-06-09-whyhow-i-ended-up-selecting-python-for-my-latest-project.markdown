---
date: '2008-06-09 19:48:00'
layout: post
slug: whyhow-i-ended-up-selecting-python-for-my-latest-project
status: publish
title: How I ended up selecting Python for my latest project
wordpress_id: '29'
categories:
- java
- php
- python
- ruby
- software
---

I have 8+ years experience on C++ and Java each and at least consider myself an expert on the latter and used to consider myself one on the former a long time back. For any programmer it is a difficult choice to move away from the platform and the environment in which one has both a substantial investment into and into another one where you essentially throw away years of experience and start as a novice (at least in pure programming terms). 

_This is not a post which is either to be interpreted as pro Python or anti Java or a pro/anti "name any language you wish". I will gladly and gleefully go back to C++ / Java when using them makes sense under a context. I am sharing my thoughts and not promoting or denigrating any languages or frameworks here._



### Context : 



Here's the shift of context (not all aspects are necessarily relevant to the shift in language). There are many details I have deliberately not got into for obvious protection required for any commercial activity.




	
  * From a relatively large programming team that I used to manage into a small one (yours truly only to begin with)

	
  * From building a commercially owned closed software into open source software development (what is at this stage intended to be though it will take a few months to get there)

	
  * From customers always being large corporates who could often afford substantial hardware into customers who can range from individuals to large corporates

	
  * From mostly internal (intranet) facing to internet facing

	
  * From performance requirements of upto a few hundred thousands of transactions per hour into a completely wide range of requirements based on each customer

	
  * From a very high percentage of writes to a much smaller percentage (ie. read percentages are now much higher)





### Initial Choice Set



Given the fact that this application is intended to be hosted on the internet and is primarily a browser based application my choices quickly narrowed down to Java / JEE (something I had a long experience on), PHP (I had developed one web based application with it), Ruby and Python (I only had academic exposure to these). C/C++ did not figure on the choice set for their obvious development overheads in the context of web applications. 

I went through a fair degree of thought and creation of dummy applications and the mental to and fro and the the process was not nearly as linear as I will describe below. The process is simplified below simply so that the reader can get at least some insight into my mind and my mind is made to seem a lot less confused than it actually is.



### 1st Elimination :



Java was the first one to get knocked off. One reason was that Java based applications typically require either a dedicated host or have to work under memory constraints in case of shared hosts. Simply put Java scales exceptionally well but it has a minimum hardware / investment requirement which was not acceptable within this context. The application should be able to worked on shared hosting environments. Another equally important reason was that the productivity of initial development and that of making changes to Java applications is much lesser than the other languages. I was only too acutely aware of the performance implications of this choice, but I believe I the appropriate choice here shall be to scale out especially where the read activity is especially large compared to the writes. Scaling out does require more complex architectures (compared to simply scaling up) but thats the way that is appropriate in this context.



### Subsequent thought process



This one was  much tougher. Let me delve for a moment into what I believed the key strengths of the various languages were.

**Ruby** : I really really loved the syntax. Compact, cute 'n' thoroughly OO. Strong metaprogramming capabilities
**PHP** : Massive developer base (especially important when the intention is to eventually open source the application). It is amongst the easiest languages to use. Another advantage in its favour is the 'C'ness of the syntax which makes it easier for anyone coming in from the C/C++/Java world.
**Python** : The metaprogramming and OO were almost but not as good as Ruby. I really love the indentation driven syntax. I know many might differ but I really like it and the neat paragraphs and lack of block braces make the code a lot more readable. Current production interpreters seem to be the best performant compared to Ruby and PHP. I know Ruby 1.9 is getting much faster but I suspect it is unlikely to be enough to make it much faster than Python already is.

As I considered the languages, it was important to look at the frameworks. I looked at CodeIgniter, CakePHP and Zend for PHP, Rails for Ruby and Pylons and Django for Python.

I believe one of the important aspects of architecture decision making today is that you bring in the available toolsets / frameworks into the decision making process even when you are attempting to select a language. You are in effect evaluating a package involving both the language and the framework. A specific issue to be noted in my context was that regardless of what framework I chose I was completely sure I would need to change it / extend it due to the fact that some of the basic requirements of the application I am about to build - no well known existing framework supports them. I am certain and convinced this is not a "Not Built Here" syndrome that I am suffering from but simply a necessity of the domain I am attempting to work with.

In terms of ease of use for simpler applications I would rate Rails very highly. Not only does it make the actual programming simple, it gives you a nice set of tools around it to make a lot of typical activities really easy. 



### 2nd elimination



Ruby and Rails went out. The reasons were as follows. 





	
  * Ruby is just so well designed from a syntax and OO perspective, that coming from so many years of C++/Java background with a substantial grounding in Object Orientation as implemented by these languages, I really did not get the confidence that I could do it sufficient justice. My fear was that I would keep on finding ways of doing things in a better way in this language (and given my inherent compulsions would feel inclined to refactor code rather than focus on newer development).

	
  * I perceived that Rails was really focused on typical much simpler use cases. What I intend to do requires getting into an immense amount of complexity and I felt fairly certain Rails wasn't designed for that and that I would spend a very very substantial time reinventing or hacking through Rails.

	
  * One of the strong features of Rails was something I couldn't really completely come to terms with was "Convention over Configuration". I still can't get over some of the implicitness in the environment.







### 3rd Elimination



Clearly PHP is such a widely used language with so many developers who are already trained on using it, that using it for an as yet intended to be open source application should be a no brainer - Right ? Not in this case. Two reasons why PHP went out.



	
  * I intend to pull off some really complex programming. Given the better OO and metaprogramming capabilities of python - I thought I would be able to keep my code much better concise, structured and readable if I was to use Python (this would've been true with Ruby as well!).


	
  * Django - This framework simply came closest to being the framework I would really like to end up with. Thus the gap between what it has vs. what I would like to have was the smallest, and the general design principles were those that I was extremely comfortable with. 





### In summary : Why these got eliminated





	
  * JEE : The difficulty of using JEE in shared hosting environments and the long development times required made it tough to use it.

	
  * Ruby : I like the language. I was a little intimidated by it. Rails however seemed limited to simpler use cases

	
  * PHP : Wonderful developer base. However didn't give me the same comfort in terms of its OO capabilities and metaprogramming and did not believe the resultant code would be the most compact, concise, and readable.





### In summary : Python and Django : What do I hope to get from them



Now that I have made the choice - what do I expect from these choices at this stage :




	
  * Excellent framework to start off with - provide the maximum initial boost

	
  * Ability to write concise, structured and readable code 

	
  * Ability to make changes rapidly

	
  * Reasonably performant application 






### Other choices that I did not spend too much time on



During this process I also evaluated other languages such as C# and Groovy/Grails. I actually was quite impressed with the recent architecture trends coming from Microsoft and was actually tempted to spend more time on them. However the same reasons that eliminated JEE quickly made these choices unviable as well. I wish I had the luxury to consider other languages as well, but these were what were at the top of my mind, and I did not consider any other languages in the process given the time constraints.



### I am missing JEE



I really really will miss JEE. I would love to have used it for the simple fact that I know it so well and this one will require me to move from an expert to a novice category. I will also miss it for the fact that with JEE I knew what it took to deliver exceptionally high performance. Even though I am feeling the JEE separation pangs, I believe Python is the right choice since it will allow for the fastest development, will allow for some really rapid changes (agility in coding), and will allow me to get the developers who shall eventually be joining us come up the curve much faster and be able to deliver more features much faster.



### Final thoughts



One thing I realised through the whole process was that there are two strong influencing factors to any architecture choices. The first one obviously is the context. There is no way to compare or contrast any architecture or design choices without putting a context around it. Secondly and this was a little bit more of a surprise to me was that you simply cannot remove personal proclivities from such a choice making process. Since I as an individual am more comfortable with some styles of design than others, and since I am likely to be substantially involved with the initial development, it only seems to make sense that the choice set gets evaluated in this subjective context of individual comfort and therefore the projected individual productivity and effectiveness as well. Note that while in this case I was evaluating it strictly from my own perspective, one could evaluate the choices in the context of Team styles, culture and comfort as well.





