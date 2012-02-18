---
date: '2008-11-25 20:24:30'
layout: post
slug: day-1-indicthreadscom-java-technology-conference
status: publish
title: 'Day 1 : IndicThreads.com Java Technology conference'
wordpress_id: '198'
categories:
- java
- software
tags:
- conference
- indicthreads
---

Here's my notes from the Day 1 of the IndicThreads conference on Java Technology. The first session focused on the state of IT industry in India, the second on some of the important developments in IT landscape, and the remainder started to get into the specific technology issues.

**Session 1 : Inaugural Address : Ganesh Natarajan, Chairman NASSCOM**

The day started off with Mr. Ganesh Natarajan, President NASSCOM delivering the inaugural address. With regards to the current state of the Indian IT-BPO industry some of the points he noted were (the words are mine)



	
  * Fantastic track record for last 4 years - however now under cloud of uncertainty

	
  * 41 billion USD exports (contrasted to 2 billion for China)

	
  * 62% BPO in India is not voice (call centers) as per popular perception but is in other areas such as transaction processing.

	
  * The Indian industry now offers a comprehensive set of services covering the entire continuum of the requirements from IT - BPO

	
  * Cost pressures forcing Indian IT to start focusing on Tier III cities. 43 tier III cities identified for growth in IT-BPO industries.

	
  * Training and skills is a huge issue. While education in Tier I colleges is very good, education in Tier III colleges still leaves a lot to be desired. While 200000 new jobs are expected to get added this year, and 6 million over the next 10 years, India will still have manpower surplus in 2020. The important challenge facing the industry was training and skills development.


**Session 2 : Keynote Address : Anand Deshpande, CoFounder and MD Persistent Systems**

The next session was by Mr. Anand Deshpande of Persistent Systems. He focused on providing a contextual changes within which the software development community shall be expected to operate in. This 10 contextual changes he elaborated are :



	
  1. Multicore

	
  2. Mobile Telephony

	
  3. Cloud and SaaS

	
  4. Web 2.0 and Social Networking

	
  5. Rich Internet Applications

	
  6. Large Volumes of Diverse Data (including BI and analytics)

	
  7. Open Source

	
  8. Gaming and Entertainment boom

	
  9. Green IT

	
  10. Community Software Development (contribute back to the community please!)


One point he made which was not as optimistic as Mr. Natarajan's opinion was that there could be significant job cuts in IT industry in India over the next 6-9 months. The rationale was if customers simply told the vendors cut 20-25% of the costs - vendors would have no choice but to have some job cuts.

**Session 3 : The Future of the Web: HTML 5, WebSocket, Java and Comet : Sidda Eraiah, Director of Management Services, Kaazing **

The session focused on HTML 5 and the WebSockets capability. Sidda discussed how WebSockets rides on HTTP/HTTPS for the initial handshake but then upgrades itself into a fully duplex (bidirectional) channel between the browser and the server supporting server initiated communication as well (post the channel being established). While he mentioned that HTML 5 wont be ready till 2022 (see related link here), browsers were already starting to support WebSockets. Opera currently supports it and Firefox has a patch which might get into the mainstream sometime soon. He demoed an application with a server side infrastructure using the [open source kaazing server](http://www.kaazing.org/confluence/display/KAAZING/Home) and JMS which eventually fed stock quotes to the browser. WebSocket lends itself to a small number of applications but can help reduce client update intervals and network bandwidth requirements substantially where it is relevant.

**Session 4 : Getting Events and Web2.0 into SOA based Solutions : Ramesh Loganathan, MD, Progress Software India**

Ramesh talked a lot about how getting an event based design was going to be important in SOA based deployments. Many points he made were rather pertinent about the overall SOA landscape. Some of these were :



	
  * SOA infrastructure has become unduly complex. He later on mentioned that in most cases where it is used today probably a simple workflow engine would suffice.

	
  * REST is a good 80/20 example of SOA - the most important functionality at minimal cost

	
  * SOA is past the hype curve and into serious adoptions

	
  * In many cases SOA is being used on the boundary for integration purposes. As an example he mentioned it being used only for batch file transfer (breaking a batch into individual transaction) at one large financial institution. SOA needs to move beyond integration and into a bit of a paradigm shift

	
  * While early SOA deployments were on the border, we need to now start designing applications using SOA ground up to leverage SOA capabilities.

	
  * Various mediation elements in the SOA landscape (transformation, security, audit etc.) were now going to be an important challenge area.

	
  * Three important things to think about going forward for SOA architectures - Event Handling, Virtualisation and Web 2.0

	
  * In the context of Virtualisation and Cloud based computing (where the SOA elements could be massively distributed), performance would be key and SOA implementations would need to be fast, reliable, scalable and secure

	
  * Again in the context of Virtualisation and Cloud based computing, these would not really impact the SOA implementations themselves since not much really changed except that one would need to account for the fact that these services could be running on a much wider (global) scale network.

	
  * In the context of events, we should build events into the SOA processes. While events are raised by services or processes, services and processes should respond to these events. These events would eventually be consumed by BAM (Business Activity Monitoring) dashboards. These would take SOA into a scenario where rather than it acting in the classic request response model, the events would trigger the requests which other services or humans (in case of BAM dashboards) would respond to. Thus we need to have events onto the ESB.


	
  * In this context one needs a good event engine with good pattern recognition capability to work on the events. Thus services could raise events, which the event engine could analyse and in turn feed new events back into other services. As an example he mentioned a case where events from a weather monitoring system were analysed in turn leading to events / requests being triggered into an airline scheduling system.

	
  * In the context of Web 2.0, he mentioned that we need to put users in the middle. I am not sure if I understood it correctly but I guess what he was opining was that we need to keep the user in mind much more when designing SOA based system sets, in a manner which would help increase the capabilities and choices at the disposal of the user.


**Session 5 : Spring 2.5: Enhanced productivity and production power : Nik Jones, Consultant, SpringSource Australasia**

This session was about various Spring capabilities with a special focus on some of the newer things added in Spring 2.5. Since much of it was a description of Spring capabilities which is probably adequately covered by Spring documentation, I will only note that I was particularly impressed by Spring Osgi capabilities in Spring 2.5 and the fact that one can build Spring based POJOs and have them hosted in a J2EE server (Tomcat) or with an EJB container or an OSGI server provided by SpringSource (which would actually host one or more web tier application server such as Tomcat).

**Session 5 : Mock Objects in Action : Paulo Caroli, Agile Coach, Thoughtworks and Snehal Jha, Thoughtworks**

The session focused on three mock object testing frameworks - jMock, easymock and mockit and demonstrated an otherwise identical set of test cases implemented using each of the three frameworks. A very nice attempt I thought at bringing out the nuanced differences across the mock testing frameworks. Interestingly Paulo refused to take the bait on what his favourite framework was, but instead referred the audience to how things were different in some of the code snippets being displayed and to a comparison on Mockit home page. The final word was - it depends - just try them out and figure out for yourself what works best for you. However I suspect it could have had a slightly better impact if the audience was prepared with a better understanding of junit (some of the questions and diversions were about junit and unit testing, and were not about mock testing), and if perhaps some more time had been spent upfront on what exactly a mock testing usecase entails in terms of writing code.
