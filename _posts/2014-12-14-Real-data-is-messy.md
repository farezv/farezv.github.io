---
published: false
---

I'm working on an android app that requires data to be scraped from a web page (because no API is available). I'm using [node.js](http://nodejs.org) to power the backend & [cheerio](http://npmjs.org/package/cheerio) for parsing HTML. I've tried libraries like [scraperjs](http://npmjs.org/package/scraperjs) but had no luck. I'm going to highlight the messiness of this real data (it's a book search result page from a library) and how I cleaned it up into nice JSON that I could cache & send to clients.

### Navigating complex html

The first problem I encountered was the incredibly complex HTML structure. I mean we're talking `divs` inside `divs` that would confuse the creators of Inception. If this HTML document was a human, it would be some genetically malformed mutant designed specifically to create chaos and bring upon the downfall of manking. It's that bad.

I finally managed to extract the little information I needed using [cheerio's](http://npmjs.org/package/cheerio) JQuery-like DOM parsing syntax. And then I noticed the following problem.

### Mostly but not always!

