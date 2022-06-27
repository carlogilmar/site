+++
title = "Ember JS First Steps"
tags = ["javascript"]
categories = ["ember"]
date = "2018-05-03"
+++

In the last post [Learning Ember](http://carlogilmar.me/post/learning-ember/) I talk about how to start with Ember.

Now, after nights studying Ember I think that learning this framework is difficult because it require think first in **Ember Concepts**.

 After nights studying Ember I can say that learning this framework is difficult because you have to think first in their main concepts, and if you have some experience with other frameworks or transpilers ( coffee script, typescript, elm) probably you will be confused.

> In my opinion Javascript is hard and difficult, so, it needs frameworks for be more efficient and simple. Frameworks as Angular JS are good for start to create applications easily and faster, so, why Ember is a special framework?

My main recommendation for persons who are learning Ember is start with understand the main concepts, and then, try to think how to create an application in this terms.

#### This are for me the main concepts that you have to understand:

![](/blog/blog/embersteps/ember1.png)

### The Main View

 Please think always that Ember use conventions. This is so important for understand how Ember render a view. It uses Handlebars, so it could be good understand this first, although you can review the ember documentation about it, this will be enough.

When you create an app:

> ember new example-app

You could see that it will create a file `app/templates/application.hbs`, the templates are Handlebars Files in `app/templates` directory.

The `application.hbs`are an important file.

This is the main view that render all in the `app/index.html`.  The application file contains the tag `{{outlet}}`. This refers where will be render other views.

> By default ember put a welcome view in application

### Basic Routing

Probably the main file for explore an Ember App is the `app/router.js`, this file contains all routes.

For create a route we can use the `Ember Cli`

> ember generate route page1

And this are going to generate three files: **a JS file, a HBS file, and a test JS file.**  This is because **the route is the main ember unit**. Every Ember Application would be created using routes.

### Render Views
All routes created by us will start in the **application** view (the father view), and this routes will be render into the application view.

![](/blog/blog/embersteps/ember2.png)

> If we go to **localhost:4200** we will see only the application view. But if we go to **localhost:4200/page1** we will see the **application** view and the **page1** route view.

We can use the handlebars tag for link views:

```
 {{# link-to ‘view-name’}} Go to link{{#link-to}}
```

### Hello World Example

**application.hbs**
```
<h1> Hi I'am the application View! </h1>
<br> {{#link-to 'page1'}} Page1 View {{/link-to}}
{{outlet}}
```

**page1.hbs**
```
<h3> I'm the page1 route </h3>
<br> {{#link-to 'application'}} Return {{/link-to}}
```

![](/blog/blog/embersteps/ember3.png)

We can create more with the templates and routes, but this is a basic example about it, and I think that this would be clear for understand this Framework.

I hope write more about Ember.

### Thanks for reading!


