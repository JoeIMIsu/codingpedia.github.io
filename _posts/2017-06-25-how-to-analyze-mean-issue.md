---
layout: post
title: How to configure Nginx in production to serve an Angular app and reverse proxy NodeJS
description: "Install Nginx on Ubuntu Server, understand configuration files, configure SSL, serve static files, 
 reverse proxy Keycloak and NodeJS servers"
author: ama
permalink: /ama/how-
published: false
categories: [mean]
tags: [codingmarks, nginx, ssl, tls, cerbot, keycloak, nodejs]
---

First when you need to optimize, it is usually great news. And indeed it is we managed have now over 1 MB of really great programming resources on the 
[codingmarks.org](http://codingmarks.org) website. We've reached that by importing [free-programming-books](https://github.com/EbookFoundation/free-programming-books) directory into
[codingmarks.org](http://codingmarks.org) - see the [codingmarks-free-programming-books-importer](https://github.com/Codingpedia/codingmarks-free-programming-books-importer)
for that. Now 

The application is easy - it loads the public bookmarks, so you can search through some of the best programming related
resources out there, or you can search through your own bookmarks.

After importing the 

Mongo DB indexes the main culprit, so I thought.
First thing inspect the indexes
ADD there mongo examples 
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
				"executionTimeMillisEstimate" : 0,
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

PUblic bookmarks:
```
> db.bookmarks.find().explain("executionStats");
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "codingpedia-bookmarks.bookmarks",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"$and" : [ ]
		},
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"$and" : [ ]
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 2734,
		"executionTimeMillis" : 1,
		"totalKeysExamined" : 0,
		"totalDocsExamined" : 2734,
		"executionStages" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"$and" : [ ]
			},
			"nReturned" : 2734,
			"executionTimeMillisEstimate" : 0,
			"works" : 2736,
			"advanced" : 2734,
			"needTime" : 1,
			"needYield" : 0,
			"saveState" : 21,
			"restoreState" : 21,
			"isEOF" : 1,
			"invalidates" : 0,
			"direction" : "forward",
			"docsExamined" : 2734
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


Then within node, mongoose itself:
Code before:
```
    Bookmark.find({'shared':true}, function(err, bookmarks){
      if(err){
        return res.status(500).send(err);
      }
      res.send(bookmarks);
    });
```
After:
```
    //Bookmark.find({'shared':true}, function(err, bookmarks){
    Bookmark.find({'shared':true}).lean().exec(function(err, bookmarks){
      if(err){
        return res.status(500).send(err);
      }
      console.log('here');
      res.send(bookmarks);
```

Before:
```
bookmarks-api.codingpedia.org:server Listening on port 3000 +0ms
OPTIONS /api/users/55d49696-c07d-4b31-929c-29f7c8f1a10a/bookmarks 200 5.045 ms - 13
here
GET /api/bookmarks/ 304 511.807 ms - -
GET /api/users/55d49696-c07d-4b31-929c-29f7c8f1a10a/bookmarks 304 24.830 ms - -
```

After (with mongoose.lean) dramatic increase:
```
[nodemon] starting `node ./bin/www start`
  bookmarks-api.codingpedia.org:server Listening on port 3000 +0ms
here
GET /api/bookmarks/ 200 87.038 ms - 1040147
OPTIONS /api/users/55d49696-c07d-4b31-929c-29f7c8f1a10a/bookmarks 200 3.082 ms - 13
GET /api/users/55d49696-c07d-4b31-929c-29f7c8f1a10a/bookmarks 304 37.497 ms - -
```



In this guide we are going to:

* install the latest NGINX version in Ubuntu 16.04.1
* understand configuration files
* generate SSL certificates and configure them in NGINX
* configure NGINX as reverse proxy

NGINX is a free, open-source, high-performance HTTP server and reverse proxy, as well as an IMAP/POP3 proxy server.
We use it in the #codingmarks project as web server to serve static files and as a reverse proxy for the NodeJS API and Keycloak Server:

![Network Diagram](https://raw.githubusercontent.com/wiki/Codingpedia/bookmarks-api/images/network-diagram.png)

<!--more-->

* TOC
{:toc}

The server we are setting up Nginx for, is an Ubuntu 16.04.1 LTS:

```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.1 LTS
Release:	16.04
Codename:	xenial
```

Nginx is available in Ubuntu's default repositories. But the version available there is a little bit out-dated - when I initially installed  via

```
$ sudo apt-get update;
$ sudo apt-get install nginx
```

 I got the version `1.10.0`. Please see [here](http://nginx.org/en/CHANGES) the Nginx changelog.

# Install Nginx latest version

To install the latest version, (at the time of this writing [2017.05.15] is `1.12.0`),
  we will add the official Nginx repositories. At the end of the _/etc/apt/sources.list_ file,
   we append the following stanza(=config snippet):

```
deb http://nginx.org/packages/ubuntu/ xenial nginx
deb-src http://nginx.org/packages/ubuntu/ xenial nginx
```

**Note**
* **xenial** is the Ubuntu release we are using (see command above). For a mapping of Ubuntu versions to release names, please visit the [Official Ubuntu Releases page](https://wiki.ubuntu.com/Releases).
* if there is concern about persistence of repository additions (i.e. DigitalOcean Droplets), the appropriate stanza  may instead be added to a different list file under _/etc/apt/sources.list.d/_, such as _/etc/apt/sources.list.d/nginx.list_.

Ok, now we can install nginx by issuing again the following commands:

```
$ sudo apt-get update
$ sudo apt-get install nginx
```

You will probably be prompted for your user's password. Enter it to confirm that you wish to complete the installation. The appropriate software will be downloaded to your server and then automatically installed.


# References
* https://nginx.org/en/docs/
* https://www.nginx.com/resources/admin-guide/configuration-files/
* http://nginx.org/en/docs/ngx_core_module.html
* http://nginx.org/en/docs/http/ngx_http_core_module.html
* https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04
* https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04
* https://www.linode.com/docs/web-servers/nginx/how-to-configure-nginx
* https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-virtual-hosts-server-blocks-on-ubuntu-12-04-lts--3
* https://www.digitalocean.com/community/tutorials/how-to-implement-browser-caching-with-nginx-s-header-module-on-centos-7
* https://certbot.eff.org/all-instructions/#ubuntu-16-04-xenial-nginx
* https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html
* https://cipherli.st/
* https://www.nginx.com/resources/admin-guide/reverse-proxy/
