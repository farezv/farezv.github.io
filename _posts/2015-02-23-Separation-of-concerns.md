---
published: false
---

While working on my Android app, I came across a physical use for the idea behind separation of concerns and the benefits of having software run in "the cloud." Developing software modules separately enough, in a RESTful way such that clients don't consider that ever-changing natures of server code is incredibly useful in today's world of constantly connected devices.

I've always understood the need behind separation of concerns but this was my first, real-world experience where I noticed the presence of a bug in my Android app's functionality because I hadn't deployed my latest branch of server code. This changed very quickly, as I deployed my latest server code, and viola, the client functioned normally!

New functionality/bug fixes can be quickly addressed in this model. This is fascnicating because I grew up in the era of packaged software, where a buggy set of maps on Need for Speed 2 would never get fixed.