---
layout: post
title: Fast, faster, Angular CLI - how I converted my Angular project to use Angular CLI 
description: "Tired of searching for the right command every time you need to back up a MySql database via mysqldump. Well make an alias out of it, and it should work for a while."
author: ama
permalink: /ama/fast-faster-angular-cli-how-i-converted-my-angular-project-to-use-angular-cli
published: false
categories: [javascript]
tags: [angular, angular-cli, webpack]
---

Last week I converted the [codingmarks](http://codingmarks.org) project from the initial Angular Seed based upon - [preboot/angular-webpack](https://github.com/preboot/angular-webpack),
  to use [Angular CLI](https://cli.angular.io/) and it's awesome. It all started when I wanted to add  Ahead-Of-Time compilation[^1] support and my webpack[^2] configuration 
  just got out of hand and I couln't take the pain anymore. In this post I am going to list the major steps I had to take to achieve this. 
  It is based on the [Moving your project to Angular CLI story](https://github.com/angular/angular-cli/wiki/stories-moving-into-the-cli) with my own needs and flavor added to it. 

[^1]: <https://angular.io/docs/ts/latest/cookbook/aot-compiler.html>
[^2]: <https://webpack.js.org/guides/get-started/>


<!--more-->

## Prepare conversion
The easiest way to move an existing project to Angular CLI is to copy your application files into a new, empty CLI project. 

I started with preparing my existing project folder - `bookmarks`. 
* commit and push existing changes.
* clean the folder from temporary files and ignored files using `git clean -fdx`.
* rename your project folder to old-awesome-app.

> Before you run `git clean -fdx`, perform a `git clean -fndx` "dry run" (`-n`). This will show you which files are going to be removed without actually doing it.

Now I made a new project on the same parent folder as `old-bookmarks` using Angular CLI.

* Verified the [Angular CLI prerequisites](https://github.com/angular/angular-cli#prerequisites).
* Install the CLI globally: `npm install -g @angular/cli`.
* Make a new app: `ng new bookmarks`.
* Move into the folder: cd bookmarks.
* Tested the app is working: `ng serve --open`.


## webpack.config.js

Gone. Under the hood the whole thing still works via Webpack but the configuration is done now via the _.angular-cli.json file_.
 This is much more readable and decently explained - [Angular CLI Config Schema](https://github.com/angular/angular-cli/wiki/angular-cli). 

## package.json

Compare the `old-bookmarks/package.json` to the new `./package.json` and add in my third party libraries and the `@types/* packages, and any other types. You can see it clearly 
in this [commit](https://github.com/Codingpedia/bookmarks/commit/e7edb064483927618b182cfc5054efb913ed205f#diff-b9cfc7f2cdf78a7f4b91a753d10865a2)


PS: By the way you can find most of links referenced here if you search for the angular-cli tag in codingmarks - [http://codingmarks.org/search?q=[angular-cli]](http://codingmarks.org/search?q=[angular-cli]) 








So, without further ado, let's see the alias:



or, same with three letters:

```

$ alias mbd='mysqldump db_name -u db_user -p -h 127.0.0.1 --port 3306 --single-transaction > path_to_back_up_directory/db_name_$(date "+%Y-%m-%d_%H:%M").sql'
$ mbd
```


The option `-u`, will ask you for the password. The port number is not mandatory (as it defaults to **3306**), but if you are using other port, you need to specify it.

> I personally prefer the whole name approach, as then it is more clear to me. I can start typing `mysql-` and then **there is auto complete**. Besides that I can always `alias-grep` it[^1] - (`alias alias-grep='alias | grep'`),
 if I need to see how it looks like - using the alias in this case as sort of documentation...

Below there is a concrete example, where I backup the MySQL keycloak database:

```
$ mysql-backup-keycloak-prod
    aka
$ mysqldump keycloak -u keycloak -p -h 127.0.0.1 --port 3306 --single-transaction > ~/backup/db/keycloak_db_$(date "+%Y-%m-%d_%H:%M").sql
```

That will generate a _.sql_ file in the specified path:

```
$ ls -lrt ~/backup/db
-rw-rw-r-- 1 ama ama 199219 Mar 10 07:04 keycloak_db_2017-03-10_07:04.sql
-rw-rw-r-- 1 ama ama 198489 Apr  3 06:28 keycloak_db_2017-04-03_06:27.sql
```

We can take things even further, and back-up a remote database from the local machine, after we build a ssh-tunnel[^3] with an alias of course:

```
alias mysql-tunnel-linode='ssh -L 127.0.0.1:3305:127.0.0.1:3306 ama@w.x.y.z -N'
alias mysql-backup-keycloak-prod='mysqldump keycloak -u keycloak -p -h 127.0.0.1 --port 3305 --single-transaction > ~/backup/db/keycloak_db_prod_$(date "+%Y-%m-%d_%H:%M").sql'
```

Same back-up command as before, only the port differs now and is pointing to the tunnel port - 3305.

With this I think I've made my point about the power and versatility of bash aliases.


[^3]: <https://en.wikipedia.org/wiki/Tunneling_protocol>

## References
