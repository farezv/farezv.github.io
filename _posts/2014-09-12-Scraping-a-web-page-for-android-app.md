---
layout: post
title: Scraping a web page for android app
date: 2014-09-12 1:23
author: farezv
comments: true
categories: [programming, side projects, android]

---

So I'm working on an android app that requires accessing some search results from a web page and scraping those results for the relevant information I'm looking for. There are two ways I can approach this problem.

## Android Client + Node.js server
This is the ideal scenario because I would like my android app to be as thin as possible. I would also like an elegant solution where the data between the app and the server is passed in json and therefore light on the phone's network footprint. I have a simple prototype of this running using socket.io for node.js and [this socket.io java library](https://github.com/francoisTemasys/socket.io-java-client).

I can use [scraper.js](https://github.com/ruipgil/scraperjs) for the scraping part. The major problem with this approach that I've encountered so far is the redirect loop problem in the [request](https://github.com/mikeal/request/issues/311) module that scraperjs uses. Apparently, the url I'm trying to scrape doesn't recognize the http request coming from any common browser (which is true) and ends up sending me through a loop of 301 or 302 redirects (which is dumb).

## Scraping on the client itself
This would be the simplest approach and if I can't figure out a node.js solution above then I'll use this as the last resort. The biggest problem with this approach is that it requires using some sort of scraping mechanism on the client itself. It also means that the app now has to receive a bunch of html, most of which it won't use.

Let's hope the request bug above gets fixed or scraper can let me explicitly set the user-agent header as a common browser as [this comment](https://github.com/mikeal/request/issues/311#issuecomment-17230969) explains.

Guess I'll spend some time looking for web scraping java libraries for android...