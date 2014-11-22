---
layout: post
title: Nodemon is amazing but it crashes your heroku deploy
date: 2014-09-28 10:38
author: farezv
comments: true
categories: [programming, side projects, nodejs]

---

If you've created nodejs applications using the [express app generator](https://www.npmjs.org/package/express-generator) you're likely using the `npm start` command to launch your app. This can get annoying if you're constantly making changes.

If you're not aware of [nodemon](https://www.npmjs.org/package/nodemon), it's a simple module that monitors file changes. If any of your `.js` files change, it restarts your server without you having to type `npm start` again.

It's a neat solution and requires a simple change in the `start` key under `scripts` in your project's `package.json` file. In order for nodemon to work, you need to have `"start": "nodemon ./bin/www"`. This line is likely already there if you generated an express app (it says `node ./bin/www` by default).

This is a problem in production when you push your app to deploy on Heroku (and perhaps other services like Nodejitsu, I've only used Heroku so far, so I'm not sure). Be sure to change that line back to it's default `node ./bin/www` before you deploy.

Maybe heroku could add a check in their deployment stack somewhere to check for the default `start` command. If it's different, it should fail deploying instead of deploying successfully only to throw a `Application error: check app logs` when you go on the app's website.

Heroku is a wonderful service though, and I'm really enjoying it. It's incredibly simple and only takes a [few commands to deploy](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction).