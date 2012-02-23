---
date: '2008-04-14 16:53:59'
layout: post
slug: java-if-compete-with-php-ruby-python-stop-fixing-the-syntax-and-start-fixing-the-runtime
status: publish
title: 'Java : if (compete with PHP / Ruby / Python) { stop fixing the syntax and
comments: true
  start fixing the runtime }'
wordpress_id: '24'
categories:
- java
- php
- python
- ruby
- software
- web
---

A lot of people are wondering whether Java is under threat from a set of nimble languages - PHP / Ruby / Python / Perl. There is a flurry of activity relating to [Whether Java is losing the battle for the modern web](http://www.theserverside.com/news/thread.tss?thread_id=49018) lot of which are being driven from the earlier post by Andi Gutman  [Java is losing the battle for the modern Web. Can the JVM save the vendors?](http://andigutmans.blogspot.com/2008/03/java-is-losing-battle-for-modern-web.html ).

My recent activities had me visit the same question and the following is how I would summarise the situation and the mechanisms for Java to better compete with these nimble languages. 



### Current scenario



Yes, I think there is some threat to the scope of use of Java as a server side web application development language. Given the high acceptance of Java in the corporate environments this threat will take some time to play itself out. The sales pitch of Java has often been supported heavily by the vendors, and this has led to lesser focus on making java compete at the non-corporate-enterprise end. The nimble languages compete quite well in this space and combined with the increasing power of hardware which makes them more and more relevant each day for high performance requirements, there is a clear threat of these languages starting from a lower end pushing java out of even the middle end.



#### Opinion: Dynamism of syntax is not the real issue



What I am not yet fully on board with is the characterisation that the real issue is that Java is lesser dynamic than say PHP / Ruby / Python. Java has a pretty strong set of capabilities and while some may desire more from a syntax perspective I don't know that thats the real issue. Lack of Closures, Dynamic Types, Duck Typing may make it difficult for java to compete in some contexts only to be at least partially offset in others. 



#### Issue : Shared hosting of java applications



The first real issue is the overhead to develop and then start hosting java applications. It is difficult for host providers to support allowing each user the ability to run their own JVM processes in a sustained fashion. I remember the days when using PostgresSQL required you to have a dedicated server whereas you could use mysql easily by just setting up yourself for a small multi-tenant plan on a web server out there. Today Java is like PostgreSQL of those days. There is no easy way to simply set yourself up to run a small Java application in a shared tenant environment. Even if you did set yourself up chances are that you would be far less than satisfied with the performance in a multi tenant situation even though Java is actually a really really fast language. Java has endeared itself to the corporate environments with their funding for creating large infrastructure stacks and simply hasn't offered enough opportunities for small enterprises / individuals to create and host their web applications on a shared infrastructure. This takes away an entire community of supporters who when they grew up would've carried java along into the larger of their applications. 



#### Issue: Compile cycle



The second issue is the nastily long compile-build cycle. This leads to a scenario where developers need to twiddle their thumbs for half their development times waiting for ant / maven / javac to do their work. It destroys their rhythm and hurts their productivity. The traditional argument of the speed / scale and enterprise-ness of Java over the nimble languages just seems lesser and lesser relevant as these languages start having a nice stack of their own enterprise frameworks and hardware developments whittle away the performance disadvantage. When someone very recently asked me why I was less likely to choose Java for my next project I just said _Java is not agile enough_. The agility of the nimble languages (no compile-build cycle) coupled with their having adequate performance profiles do lead Java to become lesser capable of competing in the space. By the way one of the real strengths of eclipse is its incremental compilation, but I cannot unfortunately use it with making code changes remotely the way I would be able to change say PHP.




### So how can java compete ? 



Assuming that java needs to compete with these languages on a better footing, some changes will be required (and some of them quite painful). These will be :



#### Make java easy to host on multi-tenant application servers



This would definitely require some changes to JVM to reduce the startup / shutdown overheads to make each processes really lightweight. It would also require some attention to how much memory gets utilised. Scenarios such as those I have used in the past to really make applications run faster by caching big time in the RAM are a no-no for multi-tenant hosting. We will thus need a version of JEE which loses the ApplicationContext. We will also need to be able to creatively work with other pools such as connection pools. Finally JEE application servers may need to be restructured to support rapidly dying processes (ie. process per web request). I am no JVM writer so wouldn't know if some changes might be required to the language but cant think of any major changes that might be required. 



#### Lose the compile cycle



Why can't "javac" be conditionally executed implicitly at the beginning of "java" process. JSPs generate java on the fly. Similarly python creates the .pyc compiled files on the fly. If the deployment can be done using .java files instead of .class, it would eliminate the compile cycle and allow a developer to change the code in one window, save it, do an alt-tab and go press the reload button on the browser. Compared to all the attention that is being paid to language features like closures and duck typing, I think this is a really really big deal. 



### How will this help ?



These steps are not targeted towards attracting the developers working in building the corporate enterprise applications (though I am certainly they will break out into three cheers if the compile cycle was done away with - especially in an optional way - ie. the production deployment would still require a war consisting of .class and not .java files). More and more development going forward is going to be characterised by a larger number of smaller applications rather than the old 60s-70s days of small number of large applications. Average applications will get smaller and these will communicate with each other more frequently using web service (REST?) calls. The trend will definitely be towards more in-place remote application reuse by remote invocation rather than through using class libraries. The nimble languages are well positioned to compete in this space. Java isn't. This will help Java attract and retain developers in a space where currently it is only likely to lose them increasingly. Moreover as application hosting infrastructure starts getting more outsourced and cloud computing gets more prevalent (eg. Amazon Web Services / Google App Engine) Java can at least compete in that space which is otherwise likely to be locked out for it.
