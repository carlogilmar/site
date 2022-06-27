+++
title = "Phoenix Live Views"
tags = ["elixir"]
categories = ["programming", "phoenix", "elixir"]
date = "2019-09-06"
+++

__The content below was generated for prepare a talk for the Elixir Meetup in CDMX 🇲🇽__


*All the drawings were drew my me.*


The **Phoenix Live Views** is an open-source project created since last October, 2018. The core maintainers are **Chris McCord, Jose Valim, Alex Garibay, Gary Rennie and Scott Newcomer**. [Join to the Elixir Community Slack!](https://elixir-slackin.herokuapp.com/)

In the next sections I'll explain what is and how I understood that this works 😅.

## A live view

![](https://res.cloudinary.com/carlogilmar/image/upload/v1567807754/carlogilmar/ElixirLiveViews_2_nfpzjy.png)

A **live view** could be understood as a process, this process keep an state, and modify his state if an event happens. Keep an eye here.

## Features and use cases

If you read the documentation you'll see similar content illustrated here.

A Live View:

- Use the `Phoenix Channels`, and it scales well vertically and horizontally
- The first rendered is by regular `HTTP` request
- Implements diff tracking
- Identify static and dynamic content
- React faster

![](https://res.cloudinary.com/carlogilmar/image/upload/v1567807755/carlogilmar/ElixirLiveViews_3_vmuoxl.png)

![](https://res.cloudinary.com/carlogilmar/image/upload/v1567807755/carlogilmar/ElixirLiveViews_5_qktpnl.png)

![](https://res.cloudinary.com/carlogilmar/image/upload/v1567807755/carlogilmar/ElixirLiveViews_4_i6ei5k.png)

## Life Cycle

When we are opening an url in the browser for show our **Live View** there happening many things:

1. A regular `HTTP Request` is sent
2. The assets are rendered
3. The **Live View** provides a signed **session data**
4. This **session data** is stored on the client
5. When the client is connected it's time to back to the server

![](https://res.cloudinary.com/carlogilmar/image/upload/v1567807756/carlogilmar/ElixirLiveViews_7_d3s8zv.png)

6. Inside the Live View the `mount/2` function is called, in this function we could assign the data for render our view
7. `render/1` function is called and it'll send regular HTML to the client
8. Our **Live View** will be completed and ready.
9. If any event happens, the same flow will be called: 1) `mount/2` and then `render/1`

![](https://res.cloudinary.com/carlogilmar/image/upload/v1567807756/carlogilmar/ElixirLiveViews_8_cfbftx.png)

## A Live View Anatomy

With this we could model the anatomy of a live view as:

- An Elixir Module
- It contains at least the follow functions:
  - `mount/2`
  - `render/1`
- It'll be render the content identified by the sigil `~L` or by a template `.leex`(remember that a template needs a view for works)

![](https://res.cloudinary.com/carlogilmar/image/upload/v1567807756/carlogilmar/ElixirLiveViews_9_pn8of2.png)

## Live View Setup

For make the setup you'll have to follow some steps: create your live view module, config a signing salt, create the socket `live` in your `Endpoint`, set as route in the `Router` or invoke in normal views, and finally connect the socket from the client in `JS`, you'll use JavaScript for only this step.

![](https://res.cloudinary.com/carlogilmar/image/upload/v1567807755/carlogilmar/ElixirLiveViews_10_bcq6ma.png)

When I was learning how to implement Live Views I followed many tutorials availables in the web, and I decided to use my Learning Drive Development flow using my version control, so while I'm following the tutorial and I was doing the setup step by step I created a git commit per step, with this way I'll be able to remember the steps and the code enough for every step. 😎

At the end of the  day I write this commits:

{{< highlight bash>}}
* 192c3cd  Step 9 Import live css
* 011d500  Step 8: Enable live reload in dev mode
* 5907daa  Step 7: Connecting the socket in app.js
* f02fdaf  Step 6: Adding JS live view dependency
* c493485  Step 5 Adding websocket in endpoint
* 196edd9  Step 4: Adding imports in main Web file
* 979d855  Step 3: Adding plugs in router
* 942aff2  Step 2 Adding signing salt and engine template
* ecb8c8d  Step 1: Adding live view dep
* 124a1bd  First commit
{{< /highlight >}}

If you want to follow this setup you have to clone my repo or see on GitHub and make a `git show` of every commit, or only see the commit content: [GitHub Repository](https://github.com/carlogilmar/live_view_demo/commits/master)

I learned this after read the documentation. I drew all the illustrations for understand better, this is my method for learn something, actually I opened an [Issue](https://github.com/phoenixframework/phoenix_live_view/issues/316) on the official repo at GitHub for try to become this images as open source contribution.

## Exploring the repository

When I'm learning a new **open source technology** I learned to explore the repository, create a fork, and try to understand what's happening there. With Elixir this practice is more funny because you have the **IEx Shell!** So I was very interested in discover how people as **Jose Valim and Chris** are creating elixir applications, so I decided to explore the **git commit history** and try to understand when and how the Live View project was developed. **This experience is so valuable because with this way you're understanding how the authors created a piece of software, and then you're learning how they are writting software.** And also using a tool as **git** you're able to discover some behaviours and patterns in the way of how a project was developed based on his version control history.

Exploring the commit history I found something like this:

{{< highlight bash>}}
* efb06a8  hace 9 meses Chris McCord Blur the focused input on form submits to ensure updates. Closes #6
* 40cd244  hace 9 meses Chris McCord Fix focus and click bugs
* 631f379  hace 9 meses GitHub Fix typo in live_view.js
* 2584eec  hace 9 meses GitHub Fix typos in phoenix_live_view.ex docs
* 2d16e59  hace 9 meses Chris McCord Fix keypress bug
* 86ec0a1  hace 9 meses José Valim Remove unused deps from lock

* 411ca1d  hace 9 meses José Valim Add live_eex <-- 😯

* b362612  hace 9 meses Chris McCord Fix binding bug
* fca9fdc  hace 9 meses Chris McCord Set probable active element to fix safari focus issue
* 24f0853  hace 9 meses Chris McCord Move to new socket change tracking
* ae84de7  hace 9 meses Chris McCord Improve diff tests
* 805edbf  hace 9 meses Chris McCord Fix diff tests
{{< /highlight >}}

And the first commit of **Jose Valim** makes me feel curious about how much code contains? Well, for my surprise the first commit contains the Live View Engine and about 600 lines of code.

This file allow me to understand something about the process implemented by a Live View, which contains tree modules:

- Phoenix.LiveView.Comprehension
- Phoenix.LiveView.Rendered
- Phoenix.LiveView.Engine

The `Phoenix.LiveView.Rendered` is a struct  which contains the **dynamic** and the **static** part of the view.

{{< highlight ruby>}}
%Phoenix.LiveView.Rendered{
  static: ["foo", "bar", "baz"],
  dynamic: ["left", "right"]
}
{{< /highlight >}}

It's important to mention that the Live View have another struct for consider a comprension:

{{< highlight ruby>}}
  <%= for point <- @points do %>
    x: <%= point.x %>
    y: <%= point.y %>
  <% end %>
{{< /highlight >}}

The engine will process this as a `Phoenix.LiveView.Comprehension`

{{< highlight ruby>}}
  %Phoenix.LiveView.Comprehension{
    static:
  ["\n  x: ", "\n  y: ", "\n"],
    dynamics:
  [
      	["1", "2"],
      	["3", "4"]
    	]
  }
{{< /highlight >}}

The `Live View Engine` is the module which generate this structs then to process a `.leex` file. This flow allow to identify which parts will be dynamic and sent to the client for update the view.

If we see how the Engine works, we have to execute the compiler from the iEX for
`Phoenix.LiveView.Engine.compile "path/to/your/file.leex", 0`, we can test simple templates:

{{< highlight ruby>}}
  Hola mundo! <%= name %>
{{< /highlight >}}

And then we'll see a tuple with Elixir syntax tree which represents the template compiled and ready to update in the client, in some part of this you'll see something like this:

{{< highlight ruby>}}
  {:%, [],
   [
     {:__aliases__, [alias: false], [:Phoenix, :LiveView, :Rendered]},
     {:%{}, [],
      [
        static: ["Hola mundo! ", "\n"],
        dynamic: [{:arg0, [], Phoenix.LiveView.Engine}],
        fingerprint: 120129061871015969357813258734644514847
      ]}
   ]}
{{< /highlight >}}

## Client Live View

This project contains some code wrote in Javascript, although you only have to connect the socket for implement a Live View, there are three goals:

1. Store the session signed, and manage the socket connection
2. Render and update the view
3. Bind the events for connect with the server through events

You will see a div which contains the session:

{{< highlight html>}}
<div data-phx-session="SFMyNTY.g3cFAluBC0buY7Iw...."
data-phx-view="CounterWeb.CounterLive"
id="phx-gR6+DLO3" class="phx-connected">
{{< /highlight >}}

If you see how the Live View are working, you have to open the HTML Elements tab in your browser and see how only the dynamic parts of the template are updating.

<iframe src="https://share.getcloudapp.com/mXuEQRYx?branding=true&amp;embed=true&amp;title=true" width="575" height="400" style="border:none" frameborder="0" allowtransparency="true" allowfullscreen="true">              </iframe>

For make this possible Live Views are using a project MorphDOM, a JS library for update the DOM without a Virtual DOM.


## Elixir Meetup CDMX

All of this content was created for share it in the local Elixir Meetup at CDMX 🇲🇽

<iframe src="//www.slideshare.net/slideshow/embed_code/key/G2oKSjHNTlNe5d" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>

<iframe src="https://www.facebook.com/plugins/video.php?href=https%3A%2F%2Fwww.facebook.com%2Fingresuelve%2Fvideos%2F611754096017791%2F&show_text=1&width=560" width="560" height="382" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowTransparency="true" allow="encrypted-media" allowFullScreen="true"></iframe>
