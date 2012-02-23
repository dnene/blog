---
date: '2009-08-13 03:06:23'
layout: post
slug: the-microsoft-word-injunction-has-nothing-to-do-with-xml
status: publish
title: The Microsoft Word injunction has nothing to do with XML
comments: true
wordpress_id: '778'
categories:
- software
tags:
- microsoft
- patents
- word
---

Obviously the development community is buzzing loudly about a injunction issued by a court against Microsoft disallowing it apparently from marketing Word. However what foxes me is that most articles give the impression that it is an issue with XML documents. As an example :




	
  * [CNet.com : Judge orders Microsoft to stop selling Word](http://news.cnet.com/8301-10805_3-10308013-75.html)

	
  * [Crunchgear : Texas Judge rules Microsoft can’t sell Word anymore](http://www.crunchgear.com/2009/08/12/texas-judge-rules-microsoft-cant-sell-word-anymore/)

	
  * [Techdirt.com : Judge Bars Sale Of Microsoft Word For Patent Infringement (Though It Won't Stick)](http://techdirt.com/articles/20090811/2330285852.shtml)



I also came across many other tweets bemoaning the verdict and expressing the opinion that it is a sad turn of events - it being related to storage of documents as XML. I did take a quick look at the said patent :[ Patent 5787449](http://patft.uspto.gov/netacgi/nph-Parser?Sect1=PTO2&Sect2=HITOFF&p=1&u=%2Fnetahtml%2FPTO%2Fsearch-bool.html&r=12&f=G&l=50&co1=AND&d=PTXT&s1=5,787,449&OS=5,787,449&RS=5,787,449).

And therein I thought there's seems to be a big misunderstanding (at least to my lay reading). The patent itself has nothing to do with storing data or documents or XML. Its got to do with a particular implementation of data storage which requires maintaining a <del>metadata</del>metacode map. I shall use an example from the patent itself.

Lets say the XML is as follows :



> <Chapter><Title>The Secret Life of Data</Title><Para>Data is hostile. </Para>The End</Chapter>



The patent  suggests a storage which would maintain a metacode map as follows :






Element NumberElementCharacter Position


1
<Chapter>
0



2
<Title>
0



3
</Title>
23



4
<Para>
23



5
</Para>
39



6
</Chapter>
46



The metacode map essentially stores a tag along with its position. You can also see the same clearly on page 15 of the [patent document on google patents](http://www.google.com/patents?id=y8UkAAAAEBAJ&printsec=abstract&zoom=4&source=gbs_overview_r&cad=0#v=onepage&q=&f=false).

My reading suggests that the patent and alleged infringement if any has got nothing to do with storage of XML documents per se. In fact, that the tags being used are XML/SGML like is probably completely coincidental - these could very well be ${chapter} instead of <chapter>. And XML documents are stored with the tags embedded along with the content - this patent actually refers to maintaining a map of these tags and the positions they should be inserted into.

Could I be wrong in my interpretations ? Perhaps, since I haven't seen anyone else point this out and would prefer to be corrected. But the fact remains that at this point in time as I write this post, I believe that XML and storage of XML documents are completely orthogonal to the patent and the case around it - that centers around a metacode map, and metacode maps are not a characteristic of typical XML storage at all. So there's probably one big misunderstanding about what this case is about and if the injunction upsets you because you are against patents in principle, thats fair. But if you are disappointed about the possibility that this somehow substantially impacts XML storage, the way I interpret it - there's no such implication.

I shall keep my fingers crossed and hope no one points out a seemingly obvious flaw in my interpretation.


