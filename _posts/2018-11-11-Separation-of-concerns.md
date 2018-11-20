---
layout: post
title: Separation of concerns and element binding
date: 2018-11-11 23:34
author: farezv
comments: true
categories: [essays]

---

Is this thing still on? Phew, it's been a while.

While working on my (now unmaintained/unlisted) [Android app](http://farez.ca/bookfish/) 4 years ago, I stumbled across an example of separation of concerns and the benefits of having software run in "the cloud." Developing software modules separately enough, in a RESTful way such that clients don't consider that ever-changing nature of server code is incredibly useful in today's world of constant connectedness and SHIP SHIP SHIP CODE!

![image](https://i.imgur.com/pB0VXKq.png)

With many software services' `1-to-n` sync, and a surge of wearables over the last few years we're increasingly getting the same data synced to our brains through a network of personal devices and all this happens faster than you can say "SAMOSA!"

I've always understood the need behind separation of concerns but this was my first experience where I noticed the presence of a bug in my Android app's client code because I hadn't deployed my latest branch of server code. This changed very quickly, as I deployed my latest server code, and viola, the client functioned normally! Clearly that was before I understood how to develop backwards compatible client code -__-

## Element Binding for Mobile Apps

When I worked as a front end dev on [FIFA Mobile](https://www.ea.com/games/fifa/fifa-mobile), we handled UI component placement in a similar manner. Handling funky device resolutions (shout-outs to Apple!) was really important without having to do a client update, since it requires a careful internal (EA) and external (App Store/Google Play) review process.

There was front end logic that read XML components through common element names in your typical `ClassType=fieldName` format that used the XML specified placement and size parameters to place the components on a particular screen. 

__There were two major benefits to this.__

1. __No client update__ in case there's a new device/resolution released in the near future. Just update the XML and deploy the new XML as a content update. 

2. __Faster iteration__ because engineers were not sitting there writing XML code and non-engineers such as producers and UX/UI folks could iterate over this without rebuilding the client a gajillion times.

New functionality/UI bug fixes can be quickly addressed in this model. This is fascinating because I grew up in the era of packaged software, where a buggy set of maps on Need for Speed 2 would never get fixed because they were already on a disc.

But now it's like "f--k it, we'll do it live!" Look how far we've come.