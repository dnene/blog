---
date: '2012-01-21 18:22:36'
layout: post
slug: conference-report-hasgeek-jsfoo-pune-2012
status: publish
title: Conference Report - Hasgeek jsFoo Pune 2012
wordpress_id: '1221'
categories:
- programming
- software
- web
tags:
- conference
- hasgeek
- javascript
- jsfoo
- pune
---

Just finished attending [jsFoo Pune 2012](http://jsfoo.in/pune2012/) organised by [HasGeek](http://hasgeek.com/). It was an interesting and a well spent day. And I've learnt if one wants to blog about a conference, it is best done immediately post the event else much gets lost with a recollection as weak as mine.

The conference was spread over 3 tracks. So the best I can do is talk about the talks I attended. 

The first talk I attended was [Node.js, HTML5 and Phonegap for high performant content site app](http://funnel.hasgeek.com/jsfoo-pune/179-node-js-html5-and-phoegap-for-high-performant-content-site-app) by [Prasoon Kumar](https://twitter.com/#!/prasoonk). I learnt some stuff about [phonegap](http://phonegap.com/) and [espresso](http://the-m-project.org/friends/espresso/). Good starting point, but hardly got to see any actual code. 

Next one was [Synchronized models using Backbone, Sockets and Node](http://funnel.hasgeek.com/jsfoo-pune/106-synchronized-models-using-backbone-sockets-and-node) by Ruben Stolk. He started up a demo and suggested that people in the room connect to it. It basically had a canvas with a image and amongst many things, one could turn the light on the light-stand on and off .. and any actions propagated to the backend and then to all the connected clients. Similarly with post-it kind of notes that one added on the canvas. Was a good example of using backbone for defining models both on the client and the server and using socket.io to stream events between the two. He went through a fair amount of code too and it was very understandable. I came back with a distinct impression that the model maintenance and ability to synchronise the model changes between a client and server and then between the server and all connected clients (thus in effect across all connected clients) was pretty cool. However I still felt there needed a fair degree of wiring up of the event related code, though I am not sure about it and need to experiment with it more.

The next one was [How to apply BDD and TDD practices, using Jasmine library?](http://funnel.hasgeek.com/jsfoo-pune/181-how-to-apply-bdd-and-tdd-practices-using-jasmine-library) by Anil Tarte. Anil and I have been colleagues in the past and I have a high respect for his skills. In the brief period he was able to convey how to use [jasmine](http://pivotal.github.com/jasmine/) to write BDD Test cases for the client side code. His server was over web sockets, and he was essentially intercepting calls into specific methods that eventually communicated with the backend. He also mentioned it would be possible to intercept the over the wire HTTP traffic instead of javascript functions, though it would create more difficulties if one was not able to precisely control the server's outputs. I quite frankly had not imagined that BDD could be used so effectively and especially localised strictly to the client. So it was an insightful talk, that definitely will have me thinking about javascript BDD the next time. However he did seem rushed and under pressure and perhaps the talk lengths could be extended from the 45 mins to 55 mins or so.

Next was [Building real-time web applications ... (Introduction to Websockets / Socket.IO)](http://funnel.hasgeek.com/jsfoo-pune/175-building-real-time-web-applications-introduction-to-websockets-socket-io) by [Aditya Y](https://twitter.com/#!/netroy). He talked a bit about evolution of websockets, various libraries that were developed along the way and the current state of websockets support. There was an interesting demo at the end where changes in one browser session were almost being reflected in real time into another browser session via websockets connections back to the server. One of the important points noted was that Websockets continues to ride over port 80 so can work across many firewalls, and while its initial handshake is HTTP like, the subsequent traffic is essentially TCP like over that HTTP connection.

A thoroughly enjoyable talk was [Advanced JavaScript Techniques](http://funnel.hasgeek.com/jsfoo-pune/170-advanced-javascript-techniques) by [Rajasekharan Vengalil](https://twitter.com/#!/avranju). He looked like he had enough stuff to talk about for the whole day, and delved into some of the OO related aspects of Javascript and the additions to ECMAScript 5. He spent some good amount of time in explaining how prototype based languages work. There are just so many intricacies to the language and so many really strange behaviours (like silently ignoring assignments etc.) that if I hated the language I would've just continuously muttered WTFs and if I happened to passionately like the language, I just might've facepalm'ed my way through the talk. Thankfully my outlook towards javascript is quite neutral, so I just enjoyed it . A lot.

[Node.js Patterns and How we build ActiveNode](http://funnel.hasgeek.com/jsfoo-pune/162-node-js-patterns-and-how-we-build-activenode) was the next talk by [Sreekanth Vadagiri](https://twitter.com/#!/sreeix). He talked about some of his experiences with node.js, his preference for using CoffeeScript rather than JS (despite it being harder to debug), many of the patterns he liked, some of the libraries he used and deployment matters. He was just as candid about some of the gotchas as well. A useful insight into the node.js system.

The last one of the day was [Amplify your stack](http://funnel.hasgeek.com/jsfoo-pune/180-amplify-your-stack) by [Sunil Pai](https://twitter.com/#!/threepointone). He talked about a lot of libraries that could be used for client side development and deployment, about using javascript for templating, ensuring rigorous unit test coverage at all stages. Gave me the feeling there's a lot I simply do not know about whats happening on the client side JS related libraries and frameworks. Couple of remarks I recollect - "You (the JS developer) own the browser" and "When you have a strong test case coverage, you can CODE BOLDLY". 

**Summary: ** There's a lot I need to learn about whats happening on the client side. Thats true about the server side as well. And the day helped me understand just a little bit better what I don't know. Yet I got the distinct feeling of discomfort with node.js. Not with the tool. But with the assumptions that seem to go with its usage. There was poor articulation about what kind of use cases it is good for. Except that it is really good for high thruput/no. of client connections. Either there was a misplaced understanding of it being the only good way to get this kind of thruput or there was inability to clearly articulate what other benefits developers could expect out of using node.js in situations where thruput/concurrent connections is not particularly important for them. Perhaps I was just at the wrong places when someone was offering a more insightful articulation of this. But I really did not hear it. 

On the whole, I enjoyed the sessions, and this was a day very well spent. Hats off to [Kiran](https://twitter.com/#!/jackerhack) and his team for organising a good event. Makes me look forward to the next one they organise.
