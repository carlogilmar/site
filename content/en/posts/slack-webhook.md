+++
date = "2016-10-09T14:04:47-05:00"
title = "How to send Slack Messages with Groovy"
categories= ['groovy']
tags = ['programming', 'Slack']
+++

Recently I have been searching projects for send messages to Slack Channel, and I found many purposes, but I had choose one, and I'll show you how to use it. It's very easy!

+ First, you have to go to your slack team, and click on "Apps & Integrations".

   ![Go to Slack](/blog/slack1.png)

+ Search and choose "Incoming WebHooks".

   ![Go to Slack](/blog/slack2.png)

+ Add Configuration: choose a channel, and Add Integration.

   ![Go to Slack](/blog/slack3.png)

+ You will get a Webhook URL.

   ![Go to Slack](/blog/slack4.png)

That's all that we need, now we going to write the groovy script.

##### Use the Project and Send Message with Groovy

You can find the original project here [github](https://github.com/ashwanthkumar/slack-java-webhook), and you can find the author Ashwanth Kumar by [twitter](https://twitter.com/_ashwanthkumar).

+ Create a simple groovy script.

+ Add the project

```java
@Grapes(
  @Grab(group='in.ashwanthkumar', module='slack-java-webhook', version='0.0.7')
		)

import in.ashwanthkumar.slack.webhook.Slack
import in.ashwanthkumar.slack.webhook.SlackMessage
```

+ Add yout WebHook URL

```java
def webhookUrl = "http:/webhokks.tag.something"

```

+ Add a simple method from Original Project, with:

```java
new Slack(webhookUrl)//
	.icon(":bangbang:") // Emoji
	.sendToChannel("General") //<-------------------Slack Channel
	.displayName("Bot de Prueba") <----------------Bot Name
	.push(new SlackMessage("Salu2 ").bold("Soy el Bot"));<---------Message

```

+ Run the script as *groovy script.groovy*

+ Check your message at Slack

   ![Go to Slack](/blog/slack5.png)

##### Try this example, and Thanks @_ashwanthkumar !

[GitHub Example](https://github.com/carlogilmar/Slack-Message-Groovy/blob/master/send.groovy)
