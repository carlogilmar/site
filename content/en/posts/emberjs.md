+++
title = "Hello Ember Js"
description = ""
date = "2018-02-15T09:13:50-06:00"
categories = ['developer', 'javascript', 'ember']
tags = ['ember']
+++

![](/blog/blog/ember/ember1.png)

This is my first post in this year.

Recently we have to develop a project about teacher training for a private university in Mexico City.

There are a lot of javascript frameworks and transpilers:   Angular JS, Vue JS, Meteor, Aurelia JS, React JS, Ionic JS, Typescript, CoffeeScript, Elm, etc. We decided learn and use **Ember JS** for develop our frontend, and **Spring/Groovy** for the backend.

##### About Ember Js
> Ember is a JavaScript front-end framework designed to help you build websites with rich and complex user interactions. It does so by providing developers both with many features that are essential to manage complexity in modern web applications, as well as an integrated development toolkit that enables rapid iteration.

Until now, I had has a little experience using **Coffee Script** and I could test a little of Angular. But now I’m in love with Ember, because I found more useful and interesting for build frontend apps, and their standards and conventions are so cool. **In this post I’going to explore the basics about ember. And then I will release many post about more content.**

## Philosophy
- Write less code

- Use web standards and common idioms

- Built for productivity and ergonomics

- Constantly growing and improving

- Friendly API


## Build with Ember is like this...
#### You have a complete blank canvas for build your frontend app
![](/blog/blog/ember/ember2.png)
#### ... add few routes and templates...
![](/blog/blog/ember/ember3.png)

## Ember Core Libraries
This framework had many libraries for extend his features.

Framework Libraries:

- **Framework** Ember JS

- **Data Persistence** Ember Data

- **Toolchain** Ember Cli

Helper Libraries

- **Batched Queue** Backburner.js

- **URL Handling** Route-recognizer.js

- **Routing** Router.js

- **Promises** Rsvp.js

- **Template Engine** handlebars

- **Asset Pipeline** Broccoli.js

## Ember App Toolbox
You can install Ember with npm, and then you can create a simple app with the ember cli:

> npm install -g ember-cli

> ember new my-first-app

The project structure contains the following directories:

- **App:** Application main source code
- **Config** Configuration and enviroments
- **Public** Assets (omg, fonts)
- **Tests**
- **Vendor** Dependencies that aren’t manage by npm.
- **ember-cli-build** How ember should build your app.
- **dist** Deployable assets

## Anatomy of an Ember App Folder
The **app** folder contains the main source code for build the app. This folder has the following structure:

- **app/templates**
- **app/routes**
- **app/components**
- **app/helpers**
- **app/models**
- **app/styles**
- **app/services**
- **app/controllers**
- **app/adapters**
- **app/serializers**
- **app/app.js**
- **app/router.js**
- **app/index.html**

## My Recommendations for Learning
I had to search a lot of material for learn Ember Js. Although there are many resources, I recommended the following material for a beginner:

#### (Book) ROCK AND ROLL WITH EMBER JS
[Rock and Roll with Ember.js](https://balinterdi.com/rock-and-roll-with-emberjs/) It’s a great book wrote by [Balint Erdi (@baaz)](https://twitter.com/baaz) where you can learn how to build an app. I consider this book as the best resource for start learning Ember Js, because It contains more core concepts, and this could help you to understand easy.

#### (Book) The Pragmatic Programmers DELIVER AUDACIOUS WEB APPS WITH EMBER 2 Matthew White
[Deliver Audacious Web Apps with Ember 2](https://pragprog.com/book/mwjsember/deliver-audacious-web-apps-with-ember-2) It’s other book for start to understand the framework, although maybe you will need to know some basic concepts for understand more fast the chapters.

#### (Video and Book) Packt Pub EMBER.JS SOLUTIONS and EMBER.JS COOKBOOK
[Ember.js Solutions](https://www.packtpub.com/web-development/emberjs-solutions-video) are videos about Ember.js, where you can watch the main concepts. I think that this is very useful when you already understand the main concepts.

#### (Course) Frontend Masters EMBER 2.X Mike North
[Ember 2.x](https://frontendmasters.com/courses/ember-2/) it’s a complete course about Ember.js, probably is the best material for start to learn ember. I recommended this course for beginners.
