+++
date = "2017-07-25T22:54:16-05:00"
description = ""
categories = ['groovy']
tags = ['vertx']
title = "Develop with VertX"
+++

### This was a post writted for **Making Devs Blog**.

Recently we use the project **Vertx** for develop a project. Now I'm going to talk about the basic functionality.

> VertX is a tool-kit for building reactive applications on the **JVM**.

It has many interesting features:

1. Actor model based.
2. It's polyglot: you can use with Java, **Groovy**, Ruby, Scala, Ceylon, Javascript, and Kotlin.

### Verticles

For the beginning it's important know that VertX has a nervous system called **Event Bus**, where you can write and run **Verticles**.

> **Verticles** are chunks of code that get deployed and run by Vert.x.

In groovy, a verticle is called **consumer**.

Every verticle have an identifier, it's only a **String**. It could be deployed on a script or a class, and into him you can receive a message from another part of the Event Bus, and send a reply.

A Vertx Verticle:

  * Run on the event bus.
  * Have a address identifier (string)
* Have a deployment ID (deployment identifier)
  * It can receive a message and send a reply to the same message
  * It can send message for other verticles

  ![][1]

  A simple groovy consumer:

{{<gist carlogilmar cd95ccc0836f07e4df0b1853a8df0ba3 >}}

### Deploy Verticles

The **verticles** can be deployed for be available on the event bus. When a **verticle** be deployed, it will be available on the Event Bus. You can deploy one consumer, or N instances of the same consumer.

![][2]

### Sending Messages

You can send messages to verticle trough the verticle's address. And it could be point to point, or a broadcast. With this simple concepts we can make a simple design using VertX verticles.

Ping-Pong simple example.

{{<gist carlogilmar 14103ad6243d1dbc7f7be062e798958f >}}

![][3]

This are only the basics concepts of VertX, because this toolkit is much bigger and useful.

You can learn more on VertX site [Vertx Site](http://vertx.io/docs/)

Visit the [ VertX Google Group ] (https://groups.google.com/forum/#!forum/vertx)

Thanks for reading.

@carlogilmar

[1]: /blog/blog/vertx/vertx1.png
[2]: /blog/blog/vertx/vertx2.png
[3]: /blog/blog/vertx/vertx3.png

