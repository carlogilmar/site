+++
title = "First iOS App: Sending Messages to Slack"
description = ""
date = "2017-09-29T00:23:08-05:00"
categories = ["swift"]
tags = ["programming", "ios"]
+++

Hey! I have been learning how to program swift because I want to write iOS apps.

For the first, I considered start with recognize my previous concepts in this new language for me, here is my "fizzbuzz" example:

{{<gist carlogilmar f27e90a89d588dad9e9f800cdfd1c1e0>}}

My guide to learn Swift and iOS are the follow videos and books:

1. [Swift Mastering The Core Concepts](https://www.packtpub.com/application-development/swift-mastering-core-concepts-integrated-course)
2. [Building iOS 10 Applications with Swift](https://www.packtpub.com/application-development/building-ios-10-applications-swift-video)
3. [iOS Programming The Big Nerd Ranch Guide 6Th Edition](https://www.bignerdranch.com/books/ios-programming/)

### My First App

First, I need to understand this:

1. How to create a single view iOS app.
2. How to use the story board.
3. How to put a buttons and labels.
4. How to connect the view and the controller.
5. How to make an event (push a button)

With these learnings, I was be able to create a basic iOS app.

I created a simple view with many input text, labels and buttons. My first approach was make a println in console when pushed the button.

![][1]

### Finding a REST Library

I need find libraries for add to my iOS App, so I installed [Cocoa Pods](https://cocoapods.org/) for this job.

```
gem install cocoapods
pod init
```

I will use a library for make REST request, so I found Alamofire.

The init command produces a Podfile, where I add my dependency:

```
platform :ios, '10.0'
target 'HomeOfficeBotApp' do
  use_frameworks!
  pod "Alamofire"
  target 'HomeOfficeBotAppTests' do
    inherit! :search_paths
  end
  target 'HomeOfficeBotAppUITests' do
    inherit! :search_paths
  end
end
```

### Sending a message to slack

Previously I wrote a post where I describe how to send messages to slack channel, you can read again [here](http://carlogilmar.me/post/slack-webhook/).

For send a post request with alamofire library, I need to write this:

{{<gist carlogilmar ca592bdce90a77f99e00d3c59b44ff5a>}}

![][2]

This was my first iOS app, although I have a lot of errors, I'm so excited because I can do a simple post request.

Thanks for reading!

[1]: /blog/blog/homebot/ios1.png
[2]: /blog/blog/homebot/ios2.png

