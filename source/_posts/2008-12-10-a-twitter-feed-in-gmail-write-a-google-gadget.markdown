---
date: '2008-12-10 11:53:20'
layout: post
slug: a-twitter-feed-in-gmail-write-a-google-gadget
status: publish
title: 'A twitter feed in gmail : Write a google gadget'
wordpress_id: '237'
categories:
- programming
- python
- software
- web
tags:
- gadgets
- gmail
- google
- rest
- twitter
---

_**Update:** I am not yet sure why, but TwitterPane (the tool described in this post) seems to have stopped working for the moment. It might help to know that there is another similar tool out there : [TwitterGadget](http://www.twittergadget.com/) that you can also take a look at. I would suggest that you use TwitterGadget since it is much better featured and is much more likely to be maintained. For figuring out out to write a gadget, welcome and read on.
_

**Update: Apr 2, 2009** Deleted most of the code on the post since the article now is a little old, the code is anyway a little out of date with twitter api and most importantly since due to formatting problems was generating some very spurious html links. I did not have the time to figure out exactly why the javascript code was generating strange HTML so have for now just removed it.

**The need for a twitter feed in gmail.**

Lately I have started using [twitter](http://twitter.com) quite a bit and find myself going back to check the feed every so frequently _(You can find my feed [here](http://twitter.com/dnene))_. The other page I also visit quite regulary is [gmail](http://mail.google.com). While I found TweetDeck an exceptionally nice client, it starts up in a different window (thus requiring the customary Alt-Tab to switch and then to switchback) and not in my default workspace (Firefox). I started using TwitterFox extension and every so often I used to look down in the bottom right corner to check if there are any new tweets in the feed. What I wished for was getting the tweets straight onto my gmail page.

**Add your own gadget to gmail.**

[Gmail Labs](http://mail.google.com/mail/#settings/labs) recently added a new capability to [add google gadgets](http://blog.go2web20.net/2008/10/how-to-add-gadgets-to-your-gmail.html) onto the the gmail workspace. I first checked out a few of the existing google gadgets which rolled in the twitter feeds, but soon realised that these were not designed for the narrow workspace one gets on the left hand sidebar in gmail (they were actually designed for the typical igoogle page). So I set out to create my own gadget which would be designed for the left hand sidebar of gmail and which could help serve me the twitter feeds. The real bonus about this would be the fact that I check gmail tab quite often, and I could now check the tweets at the same time.

**Types of Gadget**

[Getting Started: gadgets.* API](http://code.google.com/apis/gadgets/docs/gs.html) gets you the introductory start needed for the gadget world. Gadgets are of two types - client side and server side (my words). The client side type uses a host to serve the necessary files for the gadget as a static web server. Thus once the files are loaded onto your browser, the gadget runs on the browser and in turn makes API calls anywhere it needs on the cloud - your host is pretty much out of the way by then. The server side gadgets have some functionality that is running on the host (PHP / Python etc.). The gadget invokes these pages which in turn may make API calls from other services on the cloud. Since the twitter feed basically takes a feed from twitter [using its REST API](http://apiwiki.twitter.com/REST+API+Documentation), I just needed to build a client side gadget.

**Structure of a Gadget (client side)
**

Note that I describe a client side gadget here. A server side gadget would need all the elements I described here and would need additional scripts to be developed for execution on the server. A gadget primarily consists of an XML descriptor. In its simplest form, this descriptor can be embedded with HTML code which as we know in turn can be embedded with javascript and CSS. Thus one needs an ability to work with the languages above viz. XML / HTML / Javascript and CSS to get going with a gadget. While the google docs immediately start focusing on the XML descriptor first, I think thats really the last step in the process. You should have an idea of the screen area for which you are desgning a gadget (eg. sidebar, igoogle, full page etc.). You should also have an idea of what might be the configuration parameters that different users want to tweak when they install the gadget (eg. for twitter client an configurable parameter could be the refresh frequency). Once you have that covered, you basically are going to focus on getting the HTML/JS/CSS trio to work the way you desire. You can simply load the files off the local file system using the file:// URI, but I would recommend using a local web server such as apache.

**Gadget development**

**1. Basic HTML**

Lets first get started with the basic html file. We shall have an HTML file where we shall mark off the area using a <div> tag which shall be subsequently refreshed using javascript and the twitter REST API. we shall be using [jQuery](http://jquery.com) to both make the API calls and refresh the document content (the area within the "messagepanel" id). We shall also set the links to the javascript libraries, and while we are at it we shall also set a reference to the stylesheet page. _(Apologies for the fact that the syntax highlighter or wordpress seem to be having some serious problems for this HTML snippet .. hence it is being rendered in a very simple fashion)_

_Update : Code removed as stated earlier_

Thats all ? Yes, since the entire code to fetch the feed and update the display shall be in javascript. Note that you can actually bundle all this together (HTML/JS/CSS) into the outer xml that we shall write at the end, but I prefer to have it all separated out. The jquery.js refers to the downloaded jquery library file, tweetpane.js is the file where we shall be soon writing our javascript code, and tweetpane.css is where we shall be styling the display.

**2. The document on load function and refresh loop**
We want the javascript to kick in once the document is loaded. While javascript has a built in function for the same, I chose to use the one provided by jQuery. The first line below, asks jQuery to run the display() function after the document is loaded. The display function, selects the HTML segment identified by the id messagepanel, and embeds inside it a table tag and a span tag. The span tag has the id "messages" which shall be getting used subsequently to update the tweet data. For now we have provided a stub function getTweets() which we shall be enhancing to get the feed and update the display, but it should be noted that the setInterval() call will ensure that the feed is refreshed every three minutes. I would like to make the refresh interval configurable, but thats a todo item at the moment.

_Please note that HTML / CSS is not exactly stuff I do very often. So good designers are likely to find issues in my HTML code here which they can comment on in the comments for this blog post._

_Update : code removed as stated earlier_

**3. Getting the feed from twitter**
Well we need to be be making a call to twitter API to [retrieve the friends timeline](http://apiwiki.twitter.com/REST+API+Documentation#friendstimeline). It essentially requires basic HTTP authentication for the logged in user, but we shall not have to worry about in this context since the gadget runs in the browser, and the browser shall pop up the dialog box for the userid / password. Depending upon the userid supplied by the end user, and subject to successful authentication, that user's friends timeline shall be getting displayed. An important thing to be noted is that the url is "http://twitter.com/statuses/friends_timeline.<format>" where format is one of rss, xml, json or atom and since my favourite is json thats what we shall be using. It should be noted that when making AJAX calls, a callback javascript function needs to be provided which shall take two arguments viz. data and the error status. In this case that happens to be the populate function. Thats why you see getTweets() being only a one line function, which basically make the AJAX call, sets up the callback (_populate()_) and gets out of the way.

The populate() function is where much of the work gets done. We first get the HTML segment identified by the id "messages", clear any existing contents since we shall be refreshing it regulary. The data variable contains a list of json structures which are iterated using the each() function, which splits it up into individual items, and calls the anonymous sub function. This function first identifies the css style based on the message index to separate alternate lines. It also has three regular expression matchers which try to identify twitter users (identified by a word starting with the character '@'), hashtags (identified by a word starting with '#'), and hyperlinks (the typical http/ftp/mailto links), and wrap them with the necessary code to generate the anchor tag around them to be able to make each of them into an hyperlink. Finally it composes the entire HTML code and inserts it into the segment identified by "messages" id using the append function (it actually generates a div tag for each message along the way into which it inserts one message). Since we shall be plugging this gadget into a gmail sidebar, the target of the anchor tag is "_blank" so that the hyperlinks shall open themselves into a new window (or a new tab if your browser is so configured). Moreover because the sidebar is very narrow, and many links are very long without word breaks, I had to replace the link display with a fixed "(Link)" text just so that the resultant content wouldn't scroll off far too into the right.

Here's the javascript
_Sorry .. removed out as per update above_


Note that because twitter has a limitation on the number of requests per hour, and because I wasn't sure of how many attempts it would take me to get it right, I first use wget to obtain the output of the REST API call, stored the data in a file, and in the development stages used a link to that static file so that I wouldn't be making calls to twitter every time I was making some minor css / html changes.

**4. The gadget descriptor**
In this case there was relatively little complexity to describe the gadget using the gadget xml. Here's the xml which is largely self explanatory. The content url refers to the html page which contains the essential gadget functionality

You can pretty much get all the files by following the links from the gadget xml. These are :



	
  * Gadget XML :[ http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.xml]( http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.xml)

	
  * HTML : [http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.html](http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.html)

	
  * Javascript : [http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.js](http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.js)

	
  * CSS :[ http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.css]( http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.css)


In addition one basically needs a jQuery distribution to make it work.

**Screenshot**
Here's the customary screenshot of how it looks in the end

[caption id="attachment_248" align="alignleft" width="248" caption="Twitter Feed in GMail"]![Twitter Feed in GMail](http://blog.dhananjaynene.com/wp-content/uploads/2008/12/gmail-twitter-client-3.png)[/caption]

**Installation**
Should you want to tweak the code, feel free to download and modify the files referred to and self host the gadget elsewhere. But in case you want to check it out as is, in gmail, goto "Settings" select the "Labs" tab and Enable the "Add any gadget by URL" option and press Save. You will now see a new tab called "Gadgets" under "Settings". Go to the "Gadgets" tab and add the gadget by providing its url : [http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.xml](http://misc.dhananjaynene.com/gadgets/tweetpane/tweetpane.xml). Thats it and thats all it takes. It will ask you for your twitter id and password to get your feed for you (and for the cautious - the gadget host neither intercepts nor stores your userid/password - thats strictly between you and twitter.)

**Afterthoughts:**There's probably quite a few things I can still choose to do - clean up the HTML / CSS (some of it may not be quite upto the mark),  allow configurable refresh frequencies, allow configurable form factors such as for igoogle etc. But this is fairly functional at this stage and I now can get both my frequently accessed feeds from the same page.
