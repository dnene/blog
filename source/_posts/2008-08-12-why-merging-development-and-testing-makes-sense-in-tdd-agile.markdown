---
date: '2008-08-12 19:05:49'
layout: post
slug: why-merging-development-and-testing-makes-sense-in-tdd-agile
status: publish
title: Why merging development and testing makes sense in a TDD / agile environment
wordpress_id: '45'
categories:
- agile
- blogging
- programming
- software
- testing
---

Interesting post in the Agile Journal, "[Software Testing in an Agile Environment](http://www.agilejournal.com/content/view/822/111/)". Great article and a reflection on how the software testing function would be influenced in an agile environment. IMHO, the author does a nice job, but perhaps could have gone a little bit further in terms of exploring how the development and testing functions could get merged within the same set of people, and this post attempts to do the same. Some comments I have are as follows.



> But, for the QA professional an Agile approach causes discomfort - In the ideal world they would have a 'finished' product to verify against a finished specification. To be asked to validate a moving target against a changing backdrop is counterintuitive



This is no different than developers adapting themselves from a situation where they would've expected a set of clean requirements and/or design specifications. To be asked to develop a moving target against changing backdrop is counter-intuitive as well.



> For some, the role of QA is now questionable citing Test Driven Development (TDD) as the key to testing.  But, what is most important is that QA is directly involved in the agile scrums all the way through, to be an integral part of the team designing the tests, at the same time as the requirements and solutions evolve. 



Absolutely.



> 1.       "You only need to unit test - TDD testing is sufficient"   
  


For the vast majority of commercial developments this simply isn't true. Even strong proponents of agile development recognize the need for their armory to include a range of testing techniques.




Here the assumption is that TDD is in some way meant to constrain itself to unit testing. While most examples of TDD do show unit testing, I haven't actually come across an opinion which says TDD is limited to unit testing. In fact this is one of the most important aspect to be understood if one needs to place QA in the right perspective in TDD environments.

TDD stands for test driven development. These tests could be white box or black box, unit or integration. Once it sinks in that TDD encompasses all forms of testing, it is easier to work out the workflow for the delivery teams. The most obvious implication is that the QA function needs to work very closely with development in deciding the tests of all types up front (prior to writing code). Given the agile proclivity for executable code and executable tests as the specification for code and unit tests, the same would apply to functional and acceptance tests as well. One issue here is usage of Record and Playback tools. I suspect these can no longer be used in a TDD environment simply because these assume the application under test is completely ready when writing the tests. (Record and Playback tools can be used as supporting tools additionally .. but they cannot meet the expectations of a Test Driven Development workflow). Coming back to executable tests whether these be coded in jUnit or HttpUnit or whatever one's choice of tools, these can still be coded upfront and checked in before the code gets written to ensure that the code that eventually comes out meets the QA expectations. 

Therein to me lies the biggest adaptations that testing functions shall need to adapt to. For most small to medium projects, in a TDD environment, the members (who perform both development and testing roles) write the tests and the code thus obviating the need of a separate testing department. Agile Environments will drive development and testing skills to be merged within the same set of people.  While this increases the learning curve, it also substantially increases productivity since the entire communication stream between development team and testing team is largely eliminated, the occasional blame game between the two now just needs to get resolved within one persons mind. 

However there are situations where I think classification of members into developers and testers might be called for eg. where a particular product needs to be tested on a v. wide range of hardware / software platforms independently, where writing the executable tests calls for a high level of capability and skill that is of a specialised nature etc. For such teams, their rhythm will now need to adapt. Instead of developers writing code and delivering it to testers, testers shall write tests and deliver these to developers who shall then need to write code to pass them. I believe this is the most important leap that the author of the article wasn't able to make.



> 3.       "Developers write the tests using open-source tools, so we no longer need testers, or automation tools"  
  


Professional testers fulfill a different and equally valid role from their development colleagues.



Heres the tradeoff . Separating developers and testers has benefits of increased specialisation but levies high costs in terms of communication overheads, resolution of mismatched understandings, and crossing departmental boundaries in some cases. Merging the responsibility into one team member (development + testing) helps reduce all this costs even though the same person now needs to be trained in terms of having both the skillsets. Importantly, it clearly identifies where the buck stops (and eliminates the occasional ping pong between development and testing) resulting in developers verifying their own code with a much higher level of vigour resulting in a higher quality. I would submit there would be a large number of projects out there (but certainly not all) where merging these functions and removing the separation between developers and testers makes sense.



> 
Often, TDD projects have at least as much test code as application code and, therefore, are themselves software applications. This test code needs to be maintained for the life of the target application.




All the more reason why development and testing responsibilities could get merged.




> 7.       "Developers have adequate testing skills"  
  


If testing was easy everybody would do it and we'd deliver perfect code every time. Sadly, many organizations that invest in increasing a developer's coding skills and provide them with the latest integrated development toolsets, fail to see the need to develop the equivalent testing skills for their QA team, or provide them with the tools to do the job either effectively or efficiently.



Even better option - train team members to do both development and testing and equip them with the appropriate tools.



> An independent testing team serves as an objective third-party, able to see "the big picture", to validate the functionality and quality of the deliverable. While developers tend towards proving the system functions as required, a good tester will be detached enough to ask "what happens if...?" When you include business user testing as well, you are more likely to have a system that is fit for purpose. 



Why would you want to have developers who cannot see the big picture. This is a training issue not a separation of function issue. Moreover I have always found it a bit strange that one can trust one person to write the code but not believe in him to be able to think through various combinations. I would argue that it is more cost effective to have the members simultaneously trained in "how to write code ..." and "what happens if ..." thinking.



> 10.   "Developers and Testers - are like oil and water"  
  


Since the dawn of time there has often been a "them and us" tension between developers and testers. This is usually a healthy symbiotic relationship which, when working correctly, provides a mutually beneficial relationship between the two groups resulting in a higher quality deliverable for the customer.



I would submit that this has been a high cost separation. Probably at least half (and perhaps many more) of the projects out there would benefit from these responsibilities getting merged in terms of increased productivity and quality. A small project should only have customers (or their representatives or proxies if not available directly) for specifying what is needed and conducting acceptance testing, and members (who have abilities to develop and test) who service these needs. Larger projects may choose to have a more classified team (domain experts, developers, tool builders, graphic designers, testers etc.), however such classification does and will make them lesser agile (as in the english word, not the methodology) to some extent. 





