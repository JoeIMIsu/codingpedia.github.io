---
layout: post
title: Practical MongoDB shell commands every developer should know
description: "Practical MongoDB shell commands every developer should know, or I at least should know..."
author: ama
permalink: /ama/practical-mongodb-shell-commands-every-developer-should-know
published: true
categories: [mongodb]
tags: [codingmarks, mongodb]
---

Public and private [codingmarks](https://www.codingmarks.org/) are persisted in a MongoDB Server](https://docs.mongodb.com/manual/) to persist private and
public bookmarks. Very often I find myself in the situation, where I need to modify or look for something in the mongo database.
 What do I do then? Well I google it, and most likely I am pointed to the right entry in the [Mongo manual](https://docs.mongodb.com/manual/).
 With this post I try to consolidate the commands I usually use so that I have one place to come to later...

> Experience has taught me that interacting with a system via shell commands helps you at understand it better,
and sort of brings you closer to it.

  {% include source-code-codingpedia-bookmarks.html %}

> As a prerequisite one should have a Mongo Server instance running.

<!--more-->

## Start the mongo shell

To start the mongo shell and connect to the MongoDB instance running on localhost with default port, change to the MongoDB installation directory:

``` bash
> cd <mongodb installation dir>
```

and then type `./bin/mongo` to start mongo

> If you are like me, and [hooked on aliases](http://www.codingpedia.org/ama/a-developers-guide-to-using-aliases/), you might use something like `alias mongo-start-client='~/dev/mongodb/bin/mongo'`.
 You can also add `<mongodb installation dir>/bin` to the PATH environment variable and then you can just type `mongo`

MongoDB does not enable access control by default. This might be fine for development, but for a production environment it is highly recommended
to employ [authorization](https://docs.mongodb.com/manual/core/authorization/). Please see the **Create database and users** section of
the [MongoDB Setup For Production](https://github.com/Codingpedia/codingmarks-api/wiki/MongoDB-Setup-for-Production) wiki page for commands related to this.

## Quit the mongo shell

You can type the following command to exit the mongo shell:

``` bash
> quit()
```

## Display database and collections

Once in, you can print a list of all available database on the server via `show dbs`:

``` bash
> show dbs
admin                  0.000GB
codingpedia-bookmarks  0.000GB
local                  0.000GB
```

To switch to the _codingpedia-bookmarks_ database, use the `use` command:

``` bash
> use codingpedia-bookmarks
switched to db codingpedia-bookmarks
```

Then print a list of all collections of the current database

``` bash
> show collections
bookmarks
```

## Find documents

### Find all documents
To find all documents from collection, type the following:

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
### Find documents with filter

Filter by one attribute (here filter by location):
```
> db.bookmarks.find({location: "http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/"});
{ "_id" : ObjectId("5948ab65ce8e01b7e330b330"), "name" : "Git magic", "location" : "http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/", "tags" : [ "free-programming-books-zh", "版本控制" ], "description" : "", "descriptionHtml" : "<p></p>", "createdAt" : ISODate("2017-06-20T04:58:13.192Z"), "shared" : true, "userId" : "2d6f6cb5-44f3-441d-a0c5-ec9afea98d39", "language" : "zh" }
```

```
> db.bookmarks.find(more attributes maybe with tag)
```

Check out the [find documention](https://docs.mongodb.com/manual/reference/method/db.collection.find/) for more details.

### Sort results

You can apply `sort()`` to the cursor before retrieving any documents from the database.

Example Sort documents by `updatedAt` Date (`1` for ascending and `-1` for descending):
docs

``` bash
> db.bookmarks.find().sort({updatedAt:1});
> db.bookmarks.find().sort({updatedAt:-1});
```

See [doku](https://docs.mongodb.com/manual/reference/method/cursor.sort/) for more details.

## Delete/Remove documents

Remove all documents from collection:
```bash
> db.bookmarks.remove()
```

Removes all bookmarks for user, identified by userId
```
> db.bookmarks.remove({userId:"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"});
WriteResult({ "nRemoved" : 4 })
```
Remove with attributes (all but shared true):
```bash
> db.bookmarks.remove({shared:{$ne:true}})
WriteResult({ "nRemoved" : 9 })
```

For more details see the [db.collection.remove() documentation](https://docs.mongodb.com/manual/reference/method/db.collection.remove/)

## Update documents

## Update field value
Update `githubURL` for document with the given `location` (we know that is unique):

```
> db.bookmarks.update({ location : "http://www.codingpedia.org/" }, { githubURL : "https://github.com/Codingpedia/codingpedia.github.io"} );
```

## Add new field

Add new `language` field and set it to `en` for all documents:
```
> db.bookmarks.update({}, {$set: {language: "en"}}, {multi: true});
```

For more `update` options, like `upsert`, `$unset` etc, please see the [db.collection.update() manual](https://docs.mongodb.com/manual/reference/method/db.collection.update/).

### Rename a field
To rename a field, call the `$rename` operator with the current name of the field and the new name:
``` bash
> db.bookmarks.updateMany({}, { $rename: {"url":"location"} })
```
This operation renames the field `url` to `location` for all documents in the collection.
To see how to rename embedded fields and more see the [documentation for $rename](https://docs.mongodb.com/manual/reference/operator/update/rename/)


## Indexing
[Indexes](https://docs.mongodb.com/manual/indexes/) support the efficient execution of queries in MongoDB.
 Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection,
  to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

### Show indexes

Before creating an index we should see the existing indexes on a collection,
by typing the following command:

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
        }
]
```

> By default, all collections have an index on the `_id `field

### Create index
MongoDB provides complete support for indexes on any field in a collection of documents. In addition to the index on the `_id`field applications and users may add additional indexes to support important queries and operations.

> Why you might want to create indexes - well, because indexes improve the efficiency of read operations by reducing the amount of data that query operations need to process. Please see the [Query Optimization](https://docs.mongodb.com/manual/core/query-optimization/) documentation entry for more details

Some exaples:

Create [**single** index](https://docs.mongodb.com/manual/core/index-single/) for the `userId` field
```
> db.bookmarks.createIndex( { userId: 1 } )
```

> Value `1` means the index is sort ascending on the field

Create [**unique** index](https://docs.mongodb.com/manual/core/index-unique/) for the `location` field:
```
> db.bookmarks.createIndex( { location: 1 }, { unique: true } );
```

Show the newly created indexes after:
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
                "key" : {
                        "userId" : 1
                },
                "name" : "userId_1",
                "ns" : "codingpedia-bookmarks.bookmarks"
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
        }
]
```

Verify if index is used for a query with the [explain](https://docs.mongodb.com/manual/reference/explain-results/) method:
```
> db.bookmarks.find({userId:"2d6f6cb5-44f3-441d-a0c5-ec9afea98d39"}).explain("queryPlanner")
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
	"serverInfo" : {
		"host" : "adrians-mbp.home",
		"port" : 27017,
		"version" : "3.2.9",
		"gitVersion" : "22ec9e93b40c85fc7cae7d56e7d6a02fd811088c"
	},
	"ok" : 1
}
```

### Drop index

We use `db.collection.dropIndex(index)` to drop or remove an index from a collection. Let's drop the unique "name" index from index list shown above.
To drop the index 'name', we can either use the index name:
```
> db.bookmarks.dropIndex("name_1");
```
Or we can use the index specification document (key) `{ "name" : 1 } `:
```
> db.pets.dropIndex( { "name" : 1 } );
```

The more I use MongoDB, probably the bigger this blog post will get.

  {% include source-code-codingpedia-bookmarks.html %}
