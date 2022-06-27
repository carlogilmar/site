+++
title = "Phoenix Simple App using Web Sockets"
description = ""
date = "2018-03-30"
categories = ['elixir' ]
tags = ['programming', 'phoenix']
+++

Hi! This post is for explain how to create a simple application with Phoenix and Elixir using Web Sockets.

### Web Sockets

 > The Web Socket protocol based on TCP produce a bidirectional communication between client-server app, and it doesn’t need to use HTTP protocols.

I created a phoenix project without ecto `mix phx.new sampler --no-ecto` for show a simple connection using web sockets.

### Create the two-way communication

![](/blog/blog/phoenix/phoenix1.png)

For start, we have to create the communication through the web socket.

We have to make the connection to phoenix in every browser through `channels` (phoenix Implementation for web sockets).

For make this possible it’s necessary enable the Socket Library in `assets/js/app.js` , and you only have to uncomment the socket import.

Then you have to join to the channel in `assets/js/socket.js`, we are going to print a console log with the response.  For this case the channel name is **“example”.**

``` js
socket.connect()

let channel = socket.channel("example", {})

channel.join()
  .receive("ok", resp => { console.log("Joined to Example Channel!!", resp) })
  .receive("error", resp => { console.log("Unable to join", resp) })

```


Until here, we are make a connection request to “example” channel, but now we have to create this channel for complete this connection.

We have to create an elixir file with the channel in `lib/sample_web/channels/example.socket.ex`

The following function `join` allow the connection to **“example”** channel.

``` elixir
defmodule Siample.ExampleChannel do

  use Phoenix.Channel

  def join("example", payload, socket) do
    {:ok, socket}
  end

end
```

The last step is  update our `user_socket.ex`adding the next line:

``` elixir
  ## Channels
   channel "example", SampleWeb.ExampleChannel

```

If we run the application and check the browser in `localhost:4000` we will see the connection result in the console.

![](/blog/blog/phoenix/demo1.png)

### Sending Messages

We have the connection created. So we can start to pass messages.

In the client side we are going to send a simple message to **”example:broadcast”** channel.  We need the `channel`object (with the connection), and the channel direction `example:broadcast` and a message `{message:"Hello Phoenix!"}`

`socket.js`

``` js
  channel.push('example:broadcast', {message:"Hello Phoenix!"})
```

In our example channel, we can create a function for response to this direction below to the `join` function.

`example_socket.ex`

```
  def handle_in("example:broadcast", payload, socket) do
    Logger.info ":: Example:Broadcast receive a message!::"
    {:noreply, socket}
  end
```

### Sending Broadcast

![](/blog/blog/phoenix/phoenix2.png)

We can modify out last snippet for send a broadcast message to other channel `example:alert`adding: `broadcast! socket, "example:alert", payload`

```
 def handle_in("example:broadcast", payload, socket) do
    Logger.info ":: Example:Broadcast receive a message!::"
    broadcast! socket, "example:alert", payload
    {:noreply, socket}
  end
```

We are sending a message from elixir script to **”example:alert”**, we can create this channel in the client side.

In our `socket.js`we can add this below to the channel join connection:

``` js
 channel.on("example:alert", msg => {
  alert("Can you see me?!!!!")
 })
```

![](/blog/blog/phoenix/phoenix3.png)

And this channel are going to receive all messages to **”example:alert”** direction. For this case we are sending a broadcast message, so, we can open many browsers and wait the alert when the message are sended. ( this appears when you reload your application in `localhost:4000`)

![](/blog/blog/phoenix/demo2.png)

#### With this you will have a simple application using sockets connection, join a channel in client side, sending messages and broadcast messages.

##### This is very simple and useful, recently the Making Devs team develop a simple Type Racer team using this for pass messages and reload the sessions.

#### You could see the complete example here:  [Github Repository](https://github.com/carlogilmar/Elixir-Simple-Socket)

### Thanks for reading!
