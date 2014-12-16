---
published: false
---

I'm working on an android app that requires data to be scraped from a web page (because no API is available). I'm using [node.js](http://nodejs.org) to power the backend & [cheerio](http://npmjs.org/package/cheerio) for parsing HTML. I've tried libraries like [scraperjs](http://npmjs.org/package/scraperjs) but had no luck. I'm going to highlight the messiness of this real data (it's a book search result page from a library) and how I cleaned it up into nice JSON that I could cache & send to clients.

### Navigating complex html

The first problem I encountered was the incredibly complex HTML structure. I mean we're talking `divs` inside `divs` that would confuse the creators of Inception. If this HTML document was a human, it would be some genetically malformed mutant designed specifically to create chaos and bring upon the downfall of manking. It's that bad.

I finally managed to extract the little information I needed using [cheerio's](http://npmjs.org/package/cheerio) JQuery-like DOM parsing syntax. And then I noticed the following problem.

### Mostly but not always!

One issue that I saw coming up was that the data mostly came in the same format except in certain instances where one of the fields of interest was missing (and therefore blank). For instance, the call number was missing sometimes like in the following example.

This caused issues in the way the data was scraped and I had to add additional checks to ensure consistent JSON was being produced right before I sent the data to clients. My biggest concern with this, is the "future-proofability" of my scraping logic. It really makes you wonder how large scale web crawlers (aka search engines like Google) keep up with the ever-changing face of web data. 