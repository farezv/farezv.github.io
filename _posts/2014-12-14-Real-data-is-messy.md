---
layout: post
title: Real data is messy
date: 2014-12-16 15:45
author: farezv
comments: true
categories: [programming, nodejs, android]

---

I'm working on an android app that requires data to be scraped from a web page (because no API is available). I'm using [node.js](http://nodejs.org) to power the backend & [cheerio](http://npmjs.org/package/cheerio) for parsing HTML. I've tried libraries like [scraperjs](http://npmjs.org/package/scraperjs) but had no luck. I'm going to highlight the messiness of this real data (it's a book search result page from a library) and how I cleaned it up into nice JSON that I could cache & send to clients.

### Navigating complex html

The first problem I encountered was the incredibly complex HTML structure. I mean we're talking `divs` inside `divs` that would confuse the creators of Inception. If this HTML document was a human, it would be some genetically malformed mutant designed specifically to create chaos and bring upon the downfall of mankind. It's that bad.

![Complex HTML](https://s3-us-west-2.amazonaws.com/farezcablog/img/scalingRobotComplexHTML.png)

I finally managed to extract the little information I needed using [cheerio's](http://npmjs.org/package/cheerio) JQuery-like DOM parsing syntax. And then I noticed the following problem.

### Mostly but not always!

One issue that I saw coming up was that the data mostly came in the same format except in certain instances where one of the fields of interest was missing (and therefore blank). For instance, the call number was missing sometimes like in the following example.

![No Call Numbers Sometimes](https://s3-us-west-2.amazonaws.com/farezcablog/img/callNumMissing.png)
`In the second JSON object, Status ends up in the Call Number field`

This caused issues in the way the data was scraped and I had to add additional checks to ensure consistent JSON was being produced right before I sent the data to clients. My biggest concern with this, is the "future-proofability" of my scraping logic. It really makes you wonder how large scale web crawlers (aka search engines like Google) keep up with the ever-changing face of web data. 

I will have to come up with ways to periodically check this format and ensure it hasn't changed. If it does change, I will need to modify the backend of my app service ASAP. 

### Caching

One way to mitigate the changing face of data would be to cache it, but there's a large tradeoff here, especially if your application provides data that benefits from being as real-time as possible (which mine does).

Looking up books to see if they're available at a library nearby is useless if the data (aka availability of the book) changes & that change isn't reflected in the app. So I also need to come up with ways to cache things smartly, where I can store most of the info needed, and only check for availability upon request. This is a big task and will probably require a post of its own.

### Weird strings

Another thing I noticed was all kinds of strange white spaces & carriage returns in the strings I scraped but thankfully JavaScript functions like `.trim()` took care of most of it!

And finally, here it is. Nice clean JSON!

![Clean JSON](https://s3-us-west-2.amazonaws.com/farezcablog/img/cleanBRJSON.png)

Here's how it looks in the (very primitive version of the) Android app

![Serach Results](https://s3-us-west-2.amazonaws.com/farezcablog/img/bookResults_v0.1.png)