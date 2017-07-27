---
layout: post
title: How to insert a document in mongodb in java - example
description: "Simple example showing how to insert a document in mongodb with the help of Java"
author: ama
permalink: /ama/how-to-insert-a-document-in-mongodb-in-java-example
published: false
categories: [java, mongodb]
tags: [java, mongodb, example]
---

Add the java mongo driver to your class path, or if you use maven to the dependencies in your pom file:

```
<dependencies>
    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongodb-driver</artifactId>
        <version>3.2.2</version>
    </dependency>
    ......
</dependencies>
```

Prepare mongo client:

```java
MongoClientURI connectionString = new MongoClientURI("mongodb://codingpedia:codingpedia@localhost:27017/codingpedia-bookmarks");
MongoClient mongoClient = new MongoClient(connectionString);
```

Get the Mongo database:
```java
MongoDatabase database = mongoClient.getDatabase("codingpedia-bookmarks");
```
> TODO - what is mongo database - 

Now we need to select the collection

> TODO - what is a mongo collection

Prepare the document:
```

```

> TODO - what is a document

 <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" />
     Source code for this example is available on <a href="https://github.com/Codingpedia/codingmarks-free-programming-books-importer" target="_blank">Github</a>
 </p>  
