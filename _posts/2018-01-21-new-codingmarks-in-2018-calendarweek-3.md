---
layout: post
title: New codingmarks added in 2018 so far. Keywords - git, nodejs, design-patterns, mongodb etc.
description: "New codingmarks added this year so far. They were marked with the following keywords:
 clean-code, debugging, design-patterns, expressjs, git, http, mongodb, mongoose, nodejs, npm, rxjs, ssh, testing, typescript, unit-testing, utils and vscode"
author: ama
permalink: /ama/new-codingmarks-in-2018-calendarweek-3-nodejs-mongodb-git-design-patterns-and-more
published: true
categories: [codingmarks]
tags: [codingmarks]
---
New [codingmarks](https://www.codingmarks.org) added this year so far. They were marked with the following keywords: 

* TOC
{:toc} 

<!--more-->

## clean-code 

[Template method pattern - Wikipedia](https://en.wikipedia.org/wiki/Template_method_pattern)

  * tags: [design-patterns](../tags/design-patterns.md), [clean-code](../tags/clean-code.md)

What problems can the Template Method design pattern solve? [4]

* The invariant parts of a behavior should be implemented only once so that subclasses can implement the variant parts.
* Subclasses should redefine only certain parts of a behavior without changing the other parts.

Usually, subclasses control how the behavior of a parent class is redefined, and they aren't restricted to redefine only certain parts of a behavior.

[The Principles of OOD - Object Oriented Design](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

  * tags: [clean-code](../tags/clean-code.md), [design-patterns](../tags/design-patterns.md)

The first five principles are principles of class design. The first three package principles are about package cohesion, they tell us what to put inside packages. The last three principles are about the couplings between packages, and talk about metrics that evaluate the package structure of a system.

[Single responsibility principle - Wikipedia](https://en.wikipedia.org/wiki/Single_responsibility_principle)

  * tags: [clean-code](../tags/clean-code.md), [design-patterns](../tags/design-patterns.md)

The **single responsibility principle** is a computer programming principle that states that every module or class should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class. All its services should be narrowly aligned with that responsibility. Robert C. Martin expresses the principle as, "A class should have only one reason to change."

[Writing Your F.I.R.S.T Unit Tests - DZone Java](https://dzone.com/articles/writing-your-first-unit-tests)

  * :calendar: published on: 2017-03-17
  * tags: [testing](../tags/testing.md), [unit-testing](../tags/unit-testing.md), [clean-code](../tags/clean-code.md)

When writing unit tests in Java, stick to FIRST. Your tests should be fast, independent, repeatable, self-validating, and timely (unless you're using TDD).


## debugging 

[Debug Node.js Apps using VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

  * tags: [nodejs](../tags/nodejs.md), [vscode](../tags/vscode.md), [debugging](../tags/debugging.md)

The Visual Studio Code editor includes Node.js debugging support. Set breakpoints, step-in, inspect variables and more.

[Error: Permission denied (publickey) - User Documentation        ](https://help.github.com/articles/error-permission-denied-publickey/)

  * tags: [git](../tags/git.md), [debugging](../tags/debugging.md)

## design-patterns 

[Template method pattern - Wikipedia](https://en.wikipedia.org/wiki/Template_method_pattern)

  * tags: [design-patterns](../tags/design-patterns.md), [clean-code](../tags/clean-code.md)

What problems can the Template Method design pattern solve? [4]

* The invariant parts of a behavior should be implemented only once so that subclasses can implement the variant parts.
* Subclasses should redefine only certain parts of a behavior without changing the other parts.

Usually, subclasses control how the behavior of a parent class is redefined, and they aren't restricted to redefine only certain parts of a behavior.

[The Principles of OOD - Object Oriented Design](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

  * tags: [clean-code](../tags/clean-code.md), [design-patterns](../tags/design-patterns.md)

The first five principles are principles of class design. The first three package principles are about package cohesion, they tell us what to put inside packages. The last three principles are about the couplings between packages, and talk about metrics that evaluate the package structure of a system.

[Single responsibility principle - Wikipedia](https://en.wikipedia.org/wiki/Single_responsibility_principle)

  * tags: [clean-code](../tags/clean-code.md), [design-patterns](../tags/design-patterns.md)

The **single responsibility principle** is a computer programming principle that states that every module or class should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class. All its services should be narrowly aligned with that responsibility. Robert C. Martin expresses the principle as, "A class should have only one reason to change."


## expressjs 

[RESTful API In Node & Express With TypeScript & MongoDB - YouTube](https://www.youtube.com/watch?v=XqbBv1i9Yhc)

  * :calendar: published on: 2017-07-03
  * tags: [nodejs](../tags/nodejs.md), [expressjs](../tags/expressjs.md), [mongodb](../tags/mongodb.md), [mongoose](../tags/mongoose.md), [typescript](../tags/typescript.md)
  * :octocat: [github url](https://github.com/iamclaytonray/tes)

This video covers writing a RESTful JSON API using Node, Express, MongoDB, Mongoose, and TypeScript. My other Node...


## git 

[Setting your username in Git - User Documentation        ](https://help.github.com/articles/setting-your-username-in-git/)

  * tags: [git](../tags/git.md)

```
$ git config --global user.name "Mona Lisa"
$ git config --global user.email mona.lisa@paris.com
```

[Error: Permission denied (publickey) - User Documentation        ](https://help.github.com/articles/error-permission-denied-publickey/)

  * tags: [git](../tags/git.md), [debugging](../tags/debugging.md)
[Adding a new SSH key to your GitHub account - User Documentation        ](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

  * tags: [git](../tags/git.md), [ssh](../tags/ssh.md), [utils](../tags/utils.md)

```
$ pbcopy < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your clipboard
```


## http 

[Helmet](https://helmetjs.github.io/docs/)

  * tags: [nodejs](../tags/nodejs.md), [http](../tags/http.md)
  * :octocat: [github url](https://github.com/helmetjs/helmet)

Helmet helps you secure your Express.js apps by setting various HTTP headers. It's not a silver bullet, but it can help!


## mongodb 

[javascript - Mongoose Model.save() hangs when called from node.js app - Stack Overflow](https://stackoverflow.com/questions/12030371/mongoose-model-save-hangs-when-called-from-node-js-app)

  * tags: [mongoose](../tags/mongoose.md), [mongodb](../tags/mongodb.md), [nodejs](../tags/nodejs.md)

You haven't created a connection for Mongoose to use by default. Replace this:
```
db = mongoose.createConnection('localhost','nextrak')
```

With this:
```
db = mongoose.connect('localhost', 'nextrak');
```

[MongoDB Node.js Driver](http://mongodb.github.io/node-mongodb-native/)

  * tags: [mongodb](../tags/mongodb.md), [nodejs](../tags/nodejs.md)
  * :octocat: [github url](https://github.com/mongodb/node-mongodb-native/)

The official MongoDB Node.js driver provides both callback-based and Promise-based interaction with MongoDB, allowing applications to take full advantage of the new features in ES6. 

[RESTful API In Node & Express With TypeScript & MongoDB - YouTube](https://www.youtube.com/watch?v=XqbBv1i9Yhc)

  * :calendar: published on: 2017-07-03
  * tags: [nodejs](../tags/nodejs.md), [expressjs](../tags/expressjs.md), [mongodb](../tags/mongodb.md), [mongoose](../tags/mongoose.md), [typescript](../tags/typescript.md)
  * :octocat: [github url](https://github.com/iamclaytonray/tes)

This video covers writing a RESTful JSON API using Node, Express, MongoDB, Mongoose, and TypeScript. My other Node...

[GitHub - szokodiakos/typegoose: Typegoose - Define Mongoose models using TypeScript classes.](https://github.com/szokodiakos/typegoose)

  * tags: [mongoose](../tags/mongoose.md), [mongodb](../tags/mongodb.md), [nodejs](../tags/nodejs.md)
  * :octocat: [github url](https://github.com/szokodiakos/typegoose)

typegoose - Typegoose - Define Mongoose models using TypeScript classes.


## mongoose 

[javascript - Mongoose Model.save() hangs when called from node.js app - Stack Overflow](https://stackoverflow.com/questions/12030371/mongoose-model-save-hangs-when-called-from-node-js-app)

  * tags: [mongoose](../tags/mongoose.md), [mongodb](../tags/mongodb.md), [nodejs](../tags/nodejs.md)

You haven't created a connection for Mongoose to use by default. Replace this:
```
db = mongoose.createConnection('localhost','nextrak')
```

With this:
```
db = mongoose.connect('localhost', 'nextrak');
```

[RESTful API In Node & Express With TypeScript & MongoDB - YouTube](https://www.youtube.com/watch?v=XqbBv1i9Yhc)

  * :calendar: published on: 2017-07-03
  * tags: [nodejs](../tags/nodejs.md), [expressjs](../tags/expressjs.md), [mongodb](../tags/mongodb.md), [mongoose](../tags/mongoose.md), [typescript](../tags/typescript.md)
  * :octocat: [github url](https://github.com/iamclaytonray/tes)

This video covers writing a RESTful JSON API using Node, Express, MongoDB, Mongoose, and TypeScript. My other Node...

[@types/mongoose-promise does not override mongoose's default promise lib · Issue #10743 · DefinitelyTyped/DefinitelyTyped · GitHub](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/10743)

  * tags: [mongoose](../tags/mongoose.md), [typescript](../tags/typescript.md)

Apparently Typescript 2 doesn't allow assigning properties on an imported module. So this:

```
import * as mongoose from 'mongoose';
mongoose.Promise = Promise;
```

leads to error `TS2450: Left-hand side of assignment expression cannot be a constant or a read-only property`. To get your code to compile you will need to choose from one of the options below:

* `(<any>mongoose).Promise = Promise`
* `require('mongoose').Promise = Promise`
* `import mongoose = require('mongoose'); ... mongoose.Promise = Promise`


[GitHub - szokodiakos/typegoose: Typegoose - Define Mongoose models using TypeScript classes.](https://github.com/szokodiakos/typegoose)

  * tags: [mongoose](../tags/mongoose.md), [mongodb](../tags/mongodb.md), [nodejs](../tags/nodejs.md)
  * :octocat: [github url](https://github.com/szokodiakos/typegoose)

typegoose - Typegoose - Define Mongoose models using TypeScript classes.


## nodejs 

[javascript - Mongoose Model.save() hangs when called from node.js app - Stack Overflow](https://stackoverflow.com/questions/12030371/mongoose-model-save-hangs-when-called-from-node-js-app)

  * tags: [mongoose](../tags/mongoose.md), [mongodb](../tags/mongodb.md), [nodejs](../tags/nodejs.md)

You haven't created a connection for Mongoose to use by default. Replace this:
```
db = mongoose.createConnection('localhost','nextrak')
```

With this:
```
db = mongoose.connect('localhost', 'nextrak');
```

[MongoDB Node.js Driver](http://mongodb.github.io/node-mongodb-native/)

  * tags: [mongodb](../tags/mongodb.md), [nodejs](../tags/nodejs.md)
  * :octocat: [github url](https://github.com/mongodb/node-mongodb-native/)

The official MongoDB Node.js driver provides both callback-based and Promise-based interaction with MongoDB, allowing applications to take full advantage of the new features in ES6. 

[RESTful API In Node & Express With TypeScript & MongoDB - YouTube](https://www.youtube.com/watch?v=XqbBv1i9Yhc)

  * :calendar: published on: 2017-07-03
  * tags: [nodejs](../tags/nodejs.md), [expressjs](../tags/expressjs.md), [mongodb](../tags/mongodb.md), [mongoose](../tags/mongoose.md), [typescript](../tags/typescript.md)
  * :octocat: [github url](https://github.com/iamclaytonray/tes)

This video covers writing a RESTful JSON API using Node, Express, MongoDB, Mongoose, and TypeScript. My other Node...

[Helmet](https://helmetjs.github.io/docs/)

  * tags: [nodejs](../tags/nodejs.md), [http](../tags/http.md)
  * :octocat: [github url](https://github.com/helmetjs/helmet)

Helmet helps you secure your Express.js apps by setting various HTTP headers. It's not a silver bullet, but it can help!

[Debug Node.js Apps using VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

  * tags: [nodejs](../tags/nodejs.md), [vscode](../tags/vscode.md), [debugging](../tags/debugging.md)

The Visual Studio Code editor includes Node.js debugging support. Set breakpoints, step-in, inspect variables and more.

[GitHub - szokodiakos/typegoose: Typegoose - Define Mongoose models using TypeScript classes.](https://github.com/szokodiakos/typegoose)

  * tags: [mongoose](../tags/mongoose.md), [mongodb](../tags/mongodb.md), [nodejs](../tags/nodejs.md)
  * :octocat: [github url](https://github.com/szokodiakos/typegoose)

typegoose - Typegoose - Define Mongoose models using TypeScript classes.

[npm install gets stuck at sill install loadIdealTree · Issue #17228 · npm/npm · GitHub](https://github.com/npm/npm/issues/17228)

  * tags: [nodejs](../tags/nodejs.md), [npm](../tags/npm.md)

nodejs hangs when installing some packages 

Moste voted solution:

**Try to remove 'package-lock.json' file from directory where 'package.json' locate.**


## npm 

[npm install gets stuck at sill install loadIdealTree · Issue #17228 · npm/npm · GitHub](https://github.com/npm/npm/issues/17228)

  * tags: [nodejs](../tags/nodejs.md), [npm](../tags/npm.md)

nodejs hangs when installing some packages 

Moste voted solution:

**Try to remove 'package-lock.json' file from directory where 'package.json' locate.**


## rxjs 

[forkJoin · learn-rxjs](https://www.learnrxjs.io/operators/combination/forkjoin.html)

  * tags: [rxjs](../tags/rxjs.md)

This operator is best used when you have a group of observables and only care about the final emitted value of each. One common use case for this is if you wish to issue multiple requests on page load (or some other event) and only want to take action when a response has been receieved for all. In this way it is similar to how you might use [Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all).


## ssh 

[Adding a new SSH key to your GitHub account - User Documentation        ](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

  * tags: [git](../tags/git.md), [ssh](../tags/ssh.md), [utils](../tags/utils.md)

```
$ pbcopy < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your clipboard
```


## testing 

[Writing Your F.I.R.S.T Unit Tests - DZone Java](https://dzone.com/articles/writing-your-first-unit-tests)

  * :calendar: published on: 2017-03-17
  * tags: [testing](../tags/testing.md), [unit-testing](../tags/unit-testing.md), [clean-code](../tags/clean-code.md)

When writing unit tests in Java, stick to FIRST. Your tests should be fast, independent, repeatable, self-validating, and timely (unless you're using TDD).


## typescript 

[RESTful API In Node & Express With TypeScript & MongoDB - YouTube](https://www.youtube.com/watch?v=XqbBv1i9Yhc)

  * :calendar: published on: 2017-07-03
  * tags: [nodejs](../tags/nodejs.md), [expressjs](../tags/expressjs.md), [mongodb](../tags/mongodb.md), [mongoose](../tags/mongoose.md), [typescript](../tags/typescript.md)
  * :octocat: [github url](https://github.com/iamclaytonray/tes)

This video covers writing a RESTful JSON API using Node, Express, MongoDB, Mongoose, and TypeScript. My other Node...

[@types/mongoose-promise does not override mongoose's default promise lib · Issue #10743 · DefinitelyTyped/DefinitelyTyped · GitHub](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/10743)

  * tags: [mongoose](../tags/mongoose.md), [typescript](../tags/typescript.md)

Apparently Typescript 2 doesn't allow assigning properties on an imported module. So this:

```
import * as mongoose from 'mongoose';
mongoose.Promise = Promise;
```

leads to error `TS2450: Left-hand side of assignment expression cannot be a constant or a read-only property`. To get your code to compile you will need to choose from one of the options below:

* `(<any>mongoose).Promise = Promise`
* `require('mongoose').Promise = Promise`
* `import mongoose = require('mongoose'); ... mongoose.Promise = Promise`


[Coding guidelines · Microsoft/TypeScript Wiki · GitHub](https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines)

  * tags: [typescript](../tags/typescript.md)

These guidelines are **mainly meant for contributors to the TypeScript project**. Feel free to adopt them for your own team.


## unit-testing 

[Writing Your F.I.R.S.T Unit Tests - DZone Java](https://dzone.com/articles/writing-your-first-unit-tests)

  * :calendar: published on: 2017-03-17
  * tags: [testing](../tags/testing.md), [unit-testing](../tags/unit-testing.md), [clean-code](../tags/clean-code.md)

When writing unit tests in Java, stick to FIRST. Your tests should be fast, independent, repeatable, self-validating, and timely (unless you're using TDD).


## utils 

[Adding a new SSH key to your GitHub account - User Documentation        ](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

  * tags: [git](../tags/git.md), [ssh](../tags/ssh.md), [utils](../tags/utils.md)

```
$ pbcopy < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your clipboard
```


## vscode 

[Debug Node.js Apps using VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

  * tags: [nodejs](../tags/nodejs.md), [vscode](../tags/vscode.md), [debugging](../tags/debugging.md)

The Visual Studio Code editor includes Node.js debugging support. Set breakpoints, step-in, inspect variables and more.

