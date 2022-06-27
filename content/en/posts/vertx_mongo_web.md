+++
date = "2016-10-09T12:29:51-05:00"
description = ""
title = "Connect Vertx with MongoDB using Groovy"
categories= ['groovy']
tags = ['programming', 'vertx']
thumbnail = "vertx.png"

+++

![vertx](/blog/vertx.png)

##### Create a Simple Connection With Vertx, MongoDB and Groovy

+ You have to install VertX 3.3.3, Groovy, and Mongo DB.

+ Create a simple groovy script

+ Include the next imports

```java
import io.vertx.groovy.core.Vertx
import io.vertx.groovy.ext.mongo.MongoClient
```

+ Add the configuration for Mongo DB.

``` java
	def config = Vertx.currentContext().config()

	def uri = config.mongo_uri
	if (uri == null) {
		uri = "mongodb://localhost:27017"
	}

	def db = config.mongo_db
	if (db == null) {
	db = "test"//<---------------database name
	}

	def mongoconfig = [
	connection_string:uri,
	db_name:db
	]

def mongoClient = MongoClient.createShared(vertx, mongoconfig)

```

+ Add some data in the script.

```java
def person1 = [
	itemId:"12345",
	name:"Carlo",
	blog:"carlogilmar12@gmail.com",
	twitter:"karlosins"
]
```

+ Use the mongo methods for save and find the data.


```java
mongoClient.save("people", person1, { id ->
		println("Inserted id: ${id.result()}")

		mongoClient.find("people", [
				itemId:"12345"
		], { res ->
		println("Name is ${res.result()[0].name}")

	})

})
```

+ Please, run the script with *vertx run script.groovy*

![vertx](/blog/vertxdb2.png)

+ You need to consult your Mongo Server

![vertx](/blog/vertxdb1.png)

##### That's a simple way for connect Mongo Db with VertX, thats the script complete:

[Script Complete](https://github.com/carlogilmar/ExamplesVertx/blob/master/server.groovy)


##### REVIEW "How to connect VertX and Mongo with Groovy"

```
1.- Include imports.
2.- Add mongo configuration.
3.- Add some data.
4.- Use mongo methods.

All at the same script.
Run as: vertx run script.groovy
```
