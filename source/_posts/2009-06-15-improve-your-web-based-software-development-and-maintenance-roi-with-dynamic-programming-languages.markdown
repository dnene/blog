---
date: '2009-06-15 13:13:53'
layout: post
slug: improve-your-web-based-software-development-and-maintenance-roi-with-dynamic-programming-languages
status: publish
title: Improve your web based software development and maintenance ROI with dynamic
comments: true
  programming languages
wordpress_id: '725'
categories:
- java
- php
- programming
- python
- scala
- software
tags:
- dynamic languages
---

_This is a cross post of my article which appeared in [PuneTech](http://punetech.com) in March 2009 [here](http://punetech.com/improve-your-web-based-software-development-and-maintenance-roi-with-dynamic-programming-languages/). The article is reproduced verbatim including the editor's notes (in italics). I had already posted the slides referred to in the talk that was proposed in this article in the blog post[Talk Slides : Programming Language Selection](http://blog.dhananjaynene.com/2009/03/talk-slides-programming-language-selection/)._



* * *

_After we carried a few quick articles on [why you should learn more about Ruby and Ruby on Rails](http://punetech.com/why-you-need-to-learn-ruby-and-rails/) ([take 1](http://punetech.com/why-you-need-to-learn-ruby-and-rails/), [take 2](http://punetech.com/why-ruby-is-cool-take-2/)) last month, we decided that we wanted to give people a much deeper article on why these new languages (Ruby, Python, PHP) and frameworks (Rails, Django) are setting the web world on fire. We invited [Dhananjay Nene](http://punetech.com/wiki/Dhananjay_Nene) to write an article with an in depth discussion of the technical reasons how these new languages differ from the older ones and when to choose one over the other. He responded with this article which, as an added bonus, also includes the business reasons for your decisions. At the [request of the community](http://twitter.com/d7y/statuses/1267599217), Dhananjay is also [giving a talk on the relative strengths and weaknesses of different programming languages](http://upcoming.yahoo.com/event/2166183) on Saturday, 28th March, 4pm, at SICSR. All those who found this article interesting should definitely attend.
_


### Introduction


Programing language selection is often a topic that elicits a lot of excitement, debate and often a bit of acrimony as well. There is no universally superior programming language that one can recommend, so I tend to generally disregard most language opinions which say ‘X language is the best’, without specifying the context under which it is superior. Finally most language debates often deal with the technical issues and not the ROI issues. Hopefully I shall be able to address this topic without being guilty of any of these problems.




**So what languages are we referring to here ?**







[![Official Ruby logo](http://upload.wikimedia.org/wikipedia/en/d/de/Ruby-%28programming-language%29-logo-2008.png)]




The range of languages that fall under [Dynamic Programming Languages](http://en.wikipedia.org/wiki/Dynamic_programming_language#Languages) category is rather extensive. My experience is primarily limited to [Python](http://en.wikipedia.org/wiki/Python_%28programming_language%29) and to a lesser extent [PHP](http://en.wikipedia.org/wiki/PHP), [Ruby](http://en.wikipedia.org/wiki/Ruby_%28programming_language%29), [Javascript](http://en.wikipedia.org/wiki/Javascript), and [Groovy](http://en.wikipedia.org/wiki/Groovy_%28programming_language%29). For the rest of this article, I shall be primarily referring to Python or Ruby when I use the word dynamic languages, though many of the references may continue to be applicable and relevant for a number of other [dynamic programming languages](http://en.wikipedia.org/wiki/Dynamic_programming_language).

As I describe the technical characteristics, I shall also continue to attempt to address the business aspects as well, so you might find this article at a little techno-business level. Assuming I am able to excite their interest, the tech guys would not find sufficient technical details and would be hungry to hunt for more, and while the business guys would get a little teased with the possibilities, they will not quite get the ROI served in the traditionally formatted excel spreadsheets. Being aware of that, I continue down this path with a feeling that this perhaps will be the most appropriate level for me to abstract this article to.





### Characteristics of Dynamic Programming Languages.


Let us quickly review some of the characteristics :







[![CPython](http://upload.wikimedia.org/wikipedia/commons/0/06/Python_logo.svg)




**[Object Oriented](http://en.wikipedia.org/wiki/Object-oriented) :** Many dynamic languages support full object orientation. There are many who don’t necessarily buy the benefits of Object Orientation, but it is my strong belief, that once a piece of software grows beyond a certain threshold of complexity and / or size, Object Orientation starts delivering very strong dividends. There are a few areas such as highly complex, algorithmic processing which might be better suited for functional programming. However a majority of the medium-to-large sized web applications are better served by OO. The empirical evidence at least bears out the fact that most of the most popular languages today (except C) are Object Oriented. However this still is a very very large class of languages which in them include [C++](http://en.wikipedia.org/wiki/C%2B%2B), Java, PHP, Python, Ruby etc. The one area where some dynamic languages separate themselves from the others is in the notion of “everything is an object”, ie. primitives such as numbers, functions are all objects by themselves.

_Business implications:_ OO code well designed and implemented allows for a substantial reduction in maintenance costs. When working with a team which is up the curve on OO, it is likely to lead to lower costs and time on inital coding as well. On the other hand, both training costs and skill requirements are higher for fully OO languages. If you are already using partialy OO / hybrid languages such as PHP, C++ or Java, and are convinced about OO, using fully OO languages such as Python or Ruby will help you leverage the OO capabilities even further.

**[Duck Typing](http://en.wikipedia.org/wiki/Duck_typing) :** In very loose terms, duck typed languages do not require you to declare an explicit interface. You send an object a message (ie. invoke a function or access an attribute) and if it can respond to it, it will, and if it can’t it will result in an error. Duck typing is a specific typing system which is a subset of a broader system called Dynamic Typing, which often makes for an interesting debate with its counterpart - Static typing : [Static and Dynamic Type checking in practice](http://en.wikipedia.org/wiki/Dynamic_typing#Static_and_dynamic_type_checking_in_practice). For people well grounded in static typing alone, this can sometimes seem to be sacrilegious. I am convinced that duck typing makes writing code much much faster for two reasons - a) You now require to write fewer lines of code and b) You often don’t have to keep on regularly waiting for the compiler to do its work. There is also a substantial capability enhancement that dynamic typing makes to the language type system, which allow the frameworks to build dynamic types on the fly. This in turn offers the framework users many more capabilities than frameworks written in other languages. That is why it is nearly impossible to write frameworks like Rails or Django in Java (You can modify the class loaders and use byte code generation to generate the new types, but the compiler can’t see them so you cant use them). That is also why there is a lot of anticipation of using [JRuby](http://jruby.codehaus.org/), [Jython](http://www.jython.org/) and [Grails](http://grails.org/) on the [JVM](http://en.wikipedia.org/wiki/Java_Virtual_Machine) since the languages underlying them (Ruby, Python and Groovy respectively) bring the dynamic typing capabilities to the JVM platform.

_Business Implications :_Writing code is much much faster. Maintenance depending upon the situation can sometimes be more or less difficult in case of dynamic typed languages. Refactoring is usually a lot more difficult in case of [dynamically typed](http://en.wikipedia.org/wiki/Type_system) languages since the underlying type system is not able to infer sufficiently about the code to help the refactoring tools, as is possible in case of statically typed languages. It is my opinion that a skilled and trained development team using dynamic languages can generally substantially outperform another equally capable team using static languages. Insufficiently or poorly skilled development teams however can lead to very very different kind of pitfalls in these class of languages. In both cases the code becomes difficult to change or maintain due to a) cryptic code in case of dynamically typed languages and b) extremely large code bases in case of statically typed languages. Both are undesirable situations to be in but if I had to choose between one of the two, I would go for being in the cryptic mess since it is at least manageable by bringing in external skilled help.




**Metaprogramming :** Metaprogramming is in loose terms the ability of programs to write programs. A large proportion of developers may not use this capability too frequently. Specifically in web application development it gets used as a mechanism to transform one set of datastructures which a programmer specifies into code at runtime. As I point out later in this article, it in fact is a very important element in designing common frameworks and libraries which in turn offer substantial capabilities including small code and easier maintenance. A quick note to state that metaprogramming is not code generation. In case of code generation, one uses the generator to generate code which is then compiled. A big limitation with this is the fact that often people modify the generated code leading to really tough maintenance nightmares and the fact that it is a two stage process which is prone to more errors. Metaprogramming results in new code “coming to life” so to speak while your program is running.


_Business Implications :_ Read on, they will get covered in the final roundup. They are large and they are positive.




**Function blocks/objects, iterators, closures, continuations, generators: ** I will not go into any substantial details of this issue except to say that small pieces of code logic can be handled in a much much more concise way than if these weren’t supported. While many situations may not need closures support, you will be glad to have them on your side when needed.

_Business Implications : _ Helps having shorter, cleaner code leading to lesser development and maintenance costs. Another significant positive is that your developers are just likely to be so much happier since they get some truly nice building blocks for concise and elegant expression of their logic. Can’t think of any significant negatives.

There are a full range of other capabilities, but none come to mind immediately as something that have strong business implications as well.


### The role of frameworks










[![Ruby on Rails](http://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Ruby_on_Rails_logo.jpg/202px-Ruby_on_Rails_logo.jpg)](http://commons.wikipedia.org/wiki/Image:Ruby_on_Rails_logo.jpg)
    Image via [Wikipedia](http://commons.wikipedia.org/wiki/Image:Ruby_on_Rails_logo.jpg)





When did these languages say Ruby and Python originate ? Most people are likely to be a little surprised if the answer is in the last millenium. Yet Guido von Rossum started working on Python in 1986 and Ruby was released in 1992. Python has been rather well known within the scientific community and perhaps a bit within the systems / OS utility programming communities for quite some time. However both languages grabbed a large mindshare only post 2005. A big reason for their popularity (especially in case of Ruby’s case) came from the popularity the frameworks which used them. [Ruby on Rails](http://rubyonrails.org/) for ruby and [Django](http://www.djangoproject.com/) (to the best of my knowledge) for python. These frameworks combined the language capabilities with the learnings of good design practices for internet applications (eg MVC, declarative validations, simple ORM etc) into a simple usable package, which developers could take and build web applications quickly. There are examples of people having built simple web apps within a day and medium complexity apps in 1-3 weeks using these frameworks. The languages are the ingredients, the frameworks are the cooks - a great combination for serving great meals. Now you will find many such frameworks in these languages, including some which have better capabilities for building more sophisticated / complex applications eg. [Merb](http://merbivore.com/) and [Pylons](http://www.pylonshq.com/).

I am not too sure of how many people are exactly aware of the role of metaprogramming in the frameworks’ successes. I am willing to believe that but for metaprogramming, these frameworks simply would not have achieved anywhere close to the success they achieved. It is metaprogramming which takes the datastructures as defined by a developer and converts it into runtime code implicitly, saving the developer lots of time and effort. So even if most developers don’t actively write metaprograms, their lives are so much easier. Metaprogramming capabilities are also the reason why it is virtually impossible to write similar frameworks in Java. However if you are on the .NET or JVM environments, things are definitely looking encouraging with the possibilities to use IronPython or IronRuby on .NET or JRuby or Jython or Groovy+Grails on the JVM.

_Business implications :_ If you are focused on scientific or desktop or highly algorithmic applications, where python especially is used extensively, you are likely to get benefits from these languages on their own merit alone. For web applications you will see the maximum benefits by using the web MVC frameworks along with the languages. I submit that on the whole you are likely to see very substantial reduction in development, enhancement and maintenance times - sweet music for any end user, investor or project manager.


### Increased Business Agility


There is one more reason why I believe these languages are especially helpful. They help by increasing development agility to an extent where it now allows for the business to be more agile. You can get a first prototype version up in weeks, take it around to potential users, and gather feedback on the same. Incorporate elements of this feedback into the next release of working code quickly. The business benefits of such a scenario are tremendous. You might wonder that this is a process issue, so what does it have to do with a language selection. I would submit, that languages which allow changes to be made faster, help support this process in a far superior way. Another equally important facet is the superior risk management. Since you are able to build features with lower investments, you are able to get a series of customer feedbacks into your decision making process much faster. This helps being able to come up with a product that really meets the customer expectations much earlier. This happens by allowing the better features to come in earlier and also by allowing the lesser important or lesser relevant features to be decided to be deferred earlier. That’s precisely the reason why the dynamic languages have found a strong acceptance in the startup world. I believe the increasing agility which is often required in the startup world, is and will continue to be increasingly required of established enterprises. Precisely the reason why I believe these languages will continue to do better in the enterprise space as well. Finally, these languages make it relatively easier to tell your business sponsor - We will work with you on imprecise requirements rather than spending months on nailing down requirements which anyways are likely to change later. This has both a pro and a con especially for outsourcing situations. It is likely to allow for tremendous customer delight in terms of a vendor that works with him in such a flexible manner, yet it does introduce challenges in terms of how the commercials and management of the project are handled.

The reason I would like to especially point out increased business agility is because programmers don’t often visualise or evangelise it much, but when I wear a manager’s hat, it is perhaps the most compelling benefit of these languages.





### Concluding


As I said earlier, there is no single universal language which is the best for all scenarios. There are some scenarios where using dynamic languages will not be helpful


[![Programming language book sales 4Q2008](http://farm4.static.flickr.com/3569/3377871146_700f422281_o.jpg)](http://radar.oreilly.com/2009/02/state-of-the-computer-book-mar-22.html)


A Treemap view of sales of programming language books by O’Reilly Media in 4Q2008. The size of a box represents the total sales of a book. The color represents the increase or decrease in sales compared to same quarter in 2007. Green = increase, bright green = big increase, red = decrease, bright red = large decrease. See [full article at O’Reilly Radar](http://radar.oreilly.com/2009/02/state-of-the-computer-book-mar-22.html) for lots of interesting details.





**When not to use these languages**








	
  * You are building a simple / small application and don’t have the available skill sets. One exception to this is where you decide to use it in a simple application to allow yourself a non risky mechanism of building these skillsets.







	
  * Extremely High performance requirements. However please make sure that you really need the high performance capabilities of say a C, C++ or Java. In my experience 80% of developers like to believe that they are building highly performant applications where the maximum speed is a must have. Yet the top 10% of them are facing far far more critical performance requirements than the remainder. Unless you are convinced you are in the top 10%, you should certainly consider dynamic languages as an option. Moreover in case of most high performance requirements, these can sometimes be boiled down to a few inner loops / algorithms. Consider implementing the same in C, / Java or other .NET languages (depending upon the choice of your dynamic language interpreter implementation)

	
  * You have an architecture standard in place which does not allow using these languages. If you are convinced your applications are better served by using dynamic languages both from your individual application and an overall enterprise perspective, consider taking the feedback to your standards setting body to see if you can pilot a different approach. Also evaluate if the .NET or JVM versions can help you comply with the architecture guidelines.

	
  * You are unable to commit to the retraining requirements. While these languages are easy and powerful to use, leveraging that power can require some amount of retraining. If that does not fit your business plans, since the retraining effort could impact immediate and urgent requirements, that could be a reason to not use these languages. However in such situations do consider investing in building this skill sets before you get to another similar decision point.




	
  * You need a very high levels of multithreadinging as opposed to multi processing support. While this is not a typical situation for web applications, you should be aware that most dynamic languages have some limitations in terms of multi threading support. This actually is not necessarily an issue with the language as with the implementation eg. the C implementation of python has the notorious Global Interpreter Lock which constrains you from being able to use more than a handful of threads per processes efficiently. However the same restriction is not present in Jython (the jvm implementation of python). This is likely to be an issue for a miniscule percentage of the web applications market for the primary reason that multi process / shared nothing architecture styles often work quite well for many web applications and they don’t really need multi threading.






**So where’s my return on investment ?**


First of all lets talk of the investment part. If you get into it in a paced approach, the investment may not be that great. Start with a team size of anywhere between 2-6 people (depending upon your organisation and project size). Think of 15 days of intensive training followed by a 2-6 months coming up the curve effort (more likely 2 than 6). Make sure your first project is not a critical one under tremendous business pressure. This can be subsequently followed by more people getting retrained as necessary. In the longer term it might actually help reduce your incremental investment, since it might be much easier to ramp up new programmers in Ruby or Python than say Java or C#.

Secondly lets look at the incrementally higher costs. You are likely to need people who are a little bit more capable in terms of understanding and debugging the same logic expressed in fewer lines of code (that sometimes can be a challenge) and then be able to modify and enhance the same. This may increase your testing and fixing costs in the earlier days. Finally while the fewer lines of code can make refactoring easier, you could find that your total refactoring costs are a little higher.

Now the returns part. I am convinced that the increased business agility is the strongest return in business terms. Immediately after that is the substantial reduction in development, enhancement and maintenance times. If neither of these benefits are appealing, when contrasted with some other issues that you might perceive, maybe considering dynamic languages in your context is not such a great idea.

One more factor that I would of course encourage you to evaluate from a business perspective are the implications for you if your competition (assuming it is not already using them) started using these languages. The implications would vary from case to case, but it could also help you decide how important this issue is for you.


### _About the author - Dhananjay Nene_


_Dhananjay is a Software Engineer with around 17 years of experience in the field. He is passionate about software engineering, programming, design and architecture. He did his post graduation from Indian Institute of Management, Ahmedabad, and has been involved in Senior Management positions and has managed team sizes in excess of 120 persons. His [tech blog](http://blog.dhananjaynene.com/), and [twitter stream](http://twitter.com/dnene) are a must read for anybody interested in programming languages or development methodologies. Those interested in the person behind the tech can check out his [general blog](http://dhananjay.nene.in/), and [personal twitter stream](http://twitter.com/d7y). For more details, check out [Dhananjay’s PuneTech wiki profile](http://punetech.com/wiki/Dhananjay_Nene)._
