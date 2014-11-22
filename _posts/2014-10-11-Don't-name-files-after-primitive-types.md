---
layout: post
title: Don't name files after primitive types
date: 2014-10-11 11:21
author: farezv
comments: true
categories: [programming, code conventions]

---

At my current internship, I work on a large TypeScript codebase on the Visual Studio stack. We recently went through a pretty big refactoring where one of our team members moved the project code to incorporate the Asychronous Module Definition using [RequireJS](http://requirejs.org).

In doing so, when I merged the master branch into my feature branch, I came across various merge errors (as expected). What I didn't expect though, was a pretty bad case of [load timeout error](http://requirejs.org/docs/errors.html#timeout) caused by a file named `string.ts`.

The third point of the load timeout error documentation says
> If module IDs "something" and "lib/something" are both configured to point to the same "scripts/libs/something.js" file, and something.js only has one anonymous module in it, this kind of timeout error can occur. The fix is to make sure all module ID references use the same ID (either choose "something" or "lib/something" for all references)

My colleague and I finally found the culprit `<amd-dependency...string/>` where the `string.ts` file was referred to as differently (notice, the lack of the `.ts` extension, which is fine, if the filename is unique enough to be searchable) than everywhere else in the codebase, but this involved a considerable amount of work and lots of trial and error.

That alone is a big reason why file naming should be done very carefully. It's pretty easy to name something `string.ts` when the code inside contains methods that act on string objects. One should always consider how easily searchable the filename is and whether it conveys the action of the code within the file.