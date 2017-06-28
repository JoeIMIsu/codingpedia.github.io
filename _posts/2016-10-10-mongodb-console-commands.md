---
layout: post
title: Practical MongoDB commands every developer should know
description: "Practical MongoDB commands every developer should know"
author: ama
permalink: /ama/practical-mongodb-console-commands
published: false
categories: [mongodb]
tags: [codingpedia bookmarks, mongodb]
---

The bookmarks from [www.codingmarks.org](https://www.codingmarks.org/) are persisted in a [MongoDB Server](https://docs.mongodb.com/manual/), currently version 3.2.
 Very often I find in the situation where I need to modify or look for something in the mongo database. Experience has taught me that doing it
  via the shell is one the best ways and it brings you sort of closer to the system. So, in this blog post I will list some of the practical MongoDB shell commands I use,
   as a reminder for later use and maybe somebody else can take advantage of it.

  {% include source-code-codingpedia-bookmarks.html %}

> As a prerequisite you should have a Mongo Server instance running.

<!--more-->

## Start the mongo shell

To start the mongo shell and connect to the MongoDB instance running on localhost with default port, change to the MongoDB installation directory:

``` bash
> cd <mongodb installation dir>
```

and then type `./bin/mongo` to start mongo

> If you are like me, and hooked on aliases[^1], you might use something like `alias mongo-start-client='~/dev/mongodb/bin/mongo'`. You can also add <mongodb installation dir>/bin` to the `PATH environment variable and then you can just type `mongo`

[^1]: <http://www.codingpedia.org/ama/a-developers-guide-to-using-aliases/>

## Quit the mongo shell

Once in the mongo shell, type the following command to exit it:

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

Find documents via attribute filtering:
```
> db.bookmarks.find({location: "http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/"});
{ "_id" : ObjectId("5948ab65ce8e01b7e330b330"), "name" : "Git magic", "location" : "http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/", "tags" : [ "free-programming-books-zh", "版本控制" ], "description" : "", "descriptionHtml" : "<p></p>", "createdAt" : ISODate("2017-06-20T04:58:13.192Z"), "shared" : true, "userId" : "2d6f6cb5-44f3-441d-a0c5-ec9afea98d39", "language" : "zh" }
```
https://docs.mongodb.com/manual/reference/method/db.collection.find/


#### Deleting

Removes all bookmarks for user, identified by userId
```
> db.bookmarks.remove({userId:"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"});
WriteResult({ "nRemoved" : 4 })
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
https://docs.mongodb.com/manual/core/index-single/
https://docs.mongodb.com/manual/indexes/

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

Verify if index is used for a query with the explain method:
https://docs.mongodb.com/manual/reference/explain-results/

```
> db.bookmarks.find({userId:"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"}).explain("executionStats");
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "codingpedia-bookmarks.bookmarks",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"userId" : {
				"$eq" : "2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"
			}
		},
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"userId" : 1
				},
				"indexName" : "userId_1",
				"isMultiKey" : false,
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 1,
				"direction" : "forward",
				"indexBounds" : {
					"userId" : [
						"[\"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39\", \"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39\"]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 2714,
		"executionTimeMillis" : 4,
		"totalKeysExamined" : 2714,
		"totalDocsExamined" : 2714,
		"executionStages" : {
			"stage" : "FETCH",
			"nReturned" : 2714,
			"executionTimeMillisEstimate" : 10,
			"works" : 2715,
			"advanced" : 2714,
			"needTime" : 0,
			"needYield" : 0,
			"saveState" : 21,
			"restoreState" : 21,
			"isEOF" : 1,
			"invalidates" : 0,
			"docsExamined" : 2714,
			"alreadyHasObj" : 0,
			"inputStage" : {
				"stage" : "IXSCAN",
				"nReturned" : 2714,
				"executionTimeMillisEstimate" : 10,
				"works" : 2715,
				"advanced" : 2714,
				"needTime" : 0,
				"needYield" : 0,
				"saveState" : 21,
				"restoreState" : 21,
				"isEOF" : 1,
				"invalidates" : 0,
				"keyPattern" : {
					"userId" : 1
				},
				"indexName" : "userId_1",
				"isMultiKey" : false,
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 1,
				"direction" : "forward",
				"indexBounds" : {
					"userId" : [
						"[\"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39\", \"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39\"]"
					]
				},
				"keysExamined" : 2714,
				"dupsTested" : 0,
				"dupsDropped" : 0,
				"seenInvalidated" : 0
			}
		}
	},
	"serverInfo" : {
		"host" : "adrians-mbp.home",
		"port" : 27017,
		"version" : "3.2.9",
		"gitVersion" : "22ec9e93b40c85fc7cae7d56e7d6a02fd811088c"
	},
	"ok" : 1
}
```

See 

REsult:


Drop the unique "name" index:
via name
```
> db.bookmarks.dropIndex("name_1");
```
or via key
```
> db.pets.dropIndex( { "name" : 1 } );
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

Update - add new field to all entries
```
> db.bookmarks.update({}, {$set: {language: "en"}}, {multi: true});
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
