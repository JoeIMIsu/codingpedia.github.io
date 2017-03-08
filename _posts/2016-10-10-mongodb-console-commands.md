---
layout: post
title: Practical MongoDB shell commands
description: "In this one I will try to implement a simple GET method that will deliver mocked bookmarks."
author: ama
permalink: /ama/practical-mongodb-console-commands
published: false
categories: [mongodb]
tags: [codingpedia bookmarks, mongodb]
---

The bookmarks from [bookmarks.codingpedia.org](https://bookmarks.codingpedia.org/) are persisted in a [MongoDB Server](https://docs.mongodb.com/manual/), currently version 3.2. Very often I find myself needing to modify something in the
 in the database, and the experience has taught me that doing it via the shell is one the best ways and it brings you sort of closer to the system. So, in this blog post I will list some of the console commands I use in MongoDB, as a reminder for later
  and maybe somebody else can take advantage of it.

  {% include source-code-codingpedia-bookmarks.html %}

> As a prerequisite you should have a Mongo Server instance running.

<!--more-->

## Start the mongo shell

To start the mongo shell and connect to the MongoDB instance running on localhost with default port, change to the MongoDB installation directory:

``` bash
> cd <mongodb installation dir>

```

and then type `./bin/mongo` to start mongo

> If you are like me, and hooked on aliases[^1], you might use something like `alias mongo-client-start='~/dev/mongodb/bin/mongo'`. You can also add <mongodb installation dir>/bin` to the `PATH environment variable and then you can just type `mongo

[^1]: <http://www.codingpedia.org/ama/a-developers-guide-to-using-aliases/>

## Quit the mongo shell

``` bash
> quit()
```

## Working with mongo shell

### Display database and collections

First print a list of all available databases on the server via `show dbs`:

``` bash
> show dbs
admin                  0.000GB
codingpedia-bookmarks  0.000GB
local                  0.000GB
```


Then use the _codingpedia-bookmarks_ collection:

``` bash
> use codingpedia-bookmarks
switched to db codingpedia-bookmarks
```

Then print a list of all collections for current database

``` bash
> show collections
bookmarks
```

### Find documents in collection

Find all documents from collection

``` bash
> db.bookmarks.find()
```

Response

```bash
{ "_id" : ObjectId("57f3f0ba5fac7f3264354144"), "name" : "Test bookmark", "url" : "some titi url", "description" : "some description", "category" : "test", "tags" : [ "testing", "setup" ], "__v" : 0, "updatedAt" : ISODate("2016-10-10T08:18:32.563Z") }
{ "_id" : ObjectId("57f3f1235fac7f3264354146"), "name" : "Second bookmark", "url" : "some other url", "description" : "some description", "category" : "test", "tags" : [ "testing", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f484ac1f991e33ab5e50bb"), "name" : "java bookmark", "url" : "java url", "description" : "some description", "category" : "java", "tags" : [ "testing", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5cd5975bf4e35310426f7"), "name" : "git bookmark", "url" : "git url", "description" : "some git description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5cd7c453b2a353ba33f73"), "name" : "js bookmark", "url" : "js url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5db18eab35f36151691f4"), "name" : "wildfly bookmark", "url" : "wildfly url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5dbd43910e93642b25d6d"), "name" : "jboss bookmark", "url" : "jboss url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5dcf6517aa1366b0526fa"), "name" : "linux bookmark", "url" : "linux url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }
{ "_id" : ObjectId("57f5dd573fed7e3677b20ca5"), "name" : "windows bookmark", "url" : "windows url", "description" : "some js description", "category" : "git", "tags" : [ "git command", "setup" ], "__v" : 0 }

```

#### Sorting

Sort documents by updatedAt Date (ascending and descending):
docs - https://docs.mongodb.com/manual/reference/method/cursor.sort/

``` bash
> db.bookmarks.find().sort({updatedAt:1});
> db.bookmarks.find().sort({updatedAt:-1});
```


Create index for userId
```
> db.bookmarks.createIndex( { userId: 1 } )
```

> (1 - is sort ascending); see doku for more information
Doku:
https://docs.mongodb.com/v3.2/reference/method/db.collection.createIndex/#db.collection.createIndex


After that show the newly created index:

```
> db.bookmarks.getIndexes()

[
        {
                "v" : 1,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "codingpedia-bookmarks.bookmarks"
        },
        {
                "v" : 1,
                "unique" : true,
                "key" : {
                        "name" : 1
                },
                "name" : "name_1",
                "ns" : "codingpedia-bookmarks.bookmarks",
                "background" : true
        },
        {
                "v" : 1,
                "unique" : true,
                "key" : {
                        "location" : 1
                },
                "name" : "location_1",
                "ns" : "codingpedia-bookmarks.bookmarks",
                "background" : true
        },
        {
                "v" : 1,
                "key" : {
                        "userId" : 1
                },
                "name" : "userId_1",
                "ns" : "codingpedia-bookmarks.bookmarks"
        }
]
```
Doku:
https://docs.mongodb.com/v3.2/tutorial/manage-indexes/

Update Many

``` bash
> db.bookmarks.updateMany({}, { $rename: {"url":"location"} })
2016-10-11T06:31:45.390+0200 E QUERY    [thread1] uncaught exception: WriteError({
        "index" : 0,
        "code" : 11000,
        "errmsg" : "E11000 duplicate key error collection: codingpedia-bookmarks.bookmarks index: url_1 dup key: { : null }",
        "op" : {
                "q" : {

                },
                "u" : {
                        "$rename" : {
                                "url" : "location"
                        }
                },
                "multi" : true,
                "upsert" : false
        }
}) :
undefined
```

After dropping index
```
> db.bookmarks.updateMany({}, { $rename: {"url":"location"} })
{ "acknowledged" : true, "matchedCount" : 9, "modifiedCount" : 8 }
```

https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/

List all indexes
https://docs.mongodb.com/v3.2/tutorial/manage-indexes/

```bash
> db.bookmarks.getIndexes()
[
        {
                "v" : 1,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "codingpedia-bookmarks.bookmarks"
        },
        {
                "v" : 1,
                "unique" : true,
                "key" : {
                        "name" : 1
                },
                "name" : "name_1",
                "ns" : "codingpedia-bookmarks.bookmarks",
                "background" : true
        },
        {
                "v" : 1,
                "unique" : true,
                "key" : {
                        "url" : 1
                },
                "name" : "url_1",
                "ns" : "codingpedia-bookmarks.bookmarks",
                "background" : true
        }
]
```

Drop Index

```bash
> db.bookmarks.dropIndex({"url" : 1})
{ "nIndexesWas" : 3, "ok" : 1 }
```

Remove all documents from collection
```bash
> db.bookmarks.remove()
```

Remove with attributes (all but shared true):
```bash
> db.bookmarks.remove({shared:{$ne:true}})
WriteResult({ "nRemoved" : 9 })
```
Documentation - https://docs.mongodb.com/v3.2/reference/method/db.collection.remove/

## User management

https://docs.mongodb.com/v3.2/tutorial/enable-authentication/
https://docs.mongodb.com/v3.2/tutorial/manage-users-and-roles/

Create mongo admin user:
``` bash
> use admin
switched to db admin
> db.createUser({user: "admin", pwd: "admin", roles:[{role: "userAdminAnyDatabase", db: "admin"}]})
Successfully added user: {
	"user" : "admin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	]
}
```

https://docs.mongodb.com/manual/reference/built-in-roles/#userAdminAnyDatabase


Create new user on "codingpedia-bookmarks"
``` bash
> db.dropUser("codingpedia")
false
> db.getUsers()
[ ]
> db.createUser({user: "codingpedia", pwd: "codingpedia", roles:[{role: "read", db: "user-data"}, {role:"readWrite", db: "codingpedia-bookmarks"}]})
Successfully added user: {
	"user" : "codingpedia",
	"roles" : [
		{
			"role" : "read",
			"db" : "user-data"
		},
		{
			"role" : "readWrite",
			"db" : "codingpedia-bookmarks"
		}
	]
}
> db.getUsers()
[
	{
		"_id" : "codingpedia-bookmarks.codingpedia",
		"user" : "codingpedia",
		"db" : "codingpedia-bookmarks",
		"roles" : [
			{
				"role" : "read",
				"db" : "user-data"
			},
			{
				"role" : "readWrite",
				"db" : "codingpedia-bookmarks"
			}
		]
	}
]
>
```

https://docs.mongodb.com/manual/reference/command/connectionStatus/

## Show current user

Use the `connectionStatus`[^1] method to return information about the current connection, specifically the state of the authenticated users and their available permisssions:

[^1]: <https://docs.mongodb.com/manual/reference/command/connectionStatus/>

```
> db.runCommand({connectionStatus : 1})
{
	"authInfo" : {
		"authenticatedUsers" : [
			{
				"user" : "mongo-admin",
				"db" : "admin"
			}
		],
		"authenticatedUserRoles" : [
			{
				"role" : "userAdminAnyDatabase",
				"db" : "admin"
			}
		]
	},
	"ok" : 1
}
```



## References
