---
layout: post
title: Hyperlink syntax in jade templates
date: 2014-09-21 11:45
author: farezv
comments: true
categories: [programming, side projects, nodejs]

---

I've been working on a nodejs + express app that uses jade for templating and I went through a frustrating few hours of just finding the right syntax to get a hyperlink working. 

The [documentation](http://jade-lang.com) is quite lacking and most of the stackoverflow threads I found were from a couple years ago before jade hit 1.0 when the syntax was different. Here's the correct syntax.

```
p 
	| #[a(href='http://github.com') GitHub]
```		
Note those sneaky square brackets

If you wanted to use a variable in your js file and specify that in your jade template, use the following instead.

```
#[a(href='http://#{link}') Text you want showing up as link]
```
Or simply

```
#[a(href='#{link}') Text you want showing up as link]
```

You can remove the `http://` of course but I have it hardcoded up there because I clean my urls by splicing out any `http` or `https` prefixes and any irregular `/` suffixes. Then I can use the clean `example.com` formatted urls however I want.