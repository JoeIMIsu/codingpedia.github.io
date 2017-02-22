---
layout: post
title: How to use Showdown in Angular and NodeJS
description: "This post shows how to dynamically fill in data in a reactive form field, based on other field's data"
author: ama
permalink: /ama/how-to-use-shodwon-in-angular-and-nodejs
published: false
categories: [angular, nodejs]
tags: [coding bookmarks, angular, nodejs, showdown, markdown]
---

When I manage my coding bookmarks via [bookmarks.codingpedia.org](https://bookmarks.codingpedia.org/), I often have the need to place in bookmark's note either a code snippet (might be a command),
or add some ordered or unordered list - perfect match for using Markdown[^1]. This post is all about how I enabled Markdown support with the help of ShodownJS[^2], both in front end (developed with Angular[^3]) and in backend, developed with NodeJS[^4].

[^1]: <https://daringfireball.net/projects/markdown/>
[^2]: <https://github.com/showdownjs/showdown>
[^3]: <https://angular.io/>
[^4]: <https://nodejs.org/en/>

{% include source-code-codingpedia-bookmarks-frontend.html %}

<!--more-->

## What is Showdown?
So what it Showdown? Well according to the its authors "showdown is a Javascript Markdown to HTML converter, based on the original works by John Gruber. Showdown can be used client side (in the browser) or server side (with NodeJs)."[^1]

## How to use it Angular?


Well initially I did not need to use in front-end, because I would transform it in backend (see next section), but then I realized that the Bookmarkstores don't get updated rightfully..*[]:


```js
  "dependencies": {
    "@angular/common": "2.4.2",
    "@angular/compiler": "2.4.2",
    "@angular/core": "2.4.2",
    "@angular/forms": "2.4.2",
    "@angular/http": "2.4.2",
    "@angular/platform-browser": "2.4.2",
    "@angular/platform-browser-dynamic": "2.4.2",
    "@angular/router": "3.4.2",
    "@types/showdown": "^1.4.31",
    "bootstrap": "^4.0.0-alpha.5",
    "core-js": "^2.4.1",
    "immutable": "^3.7.6",
    "keycloak-js": "2.3.0",
    "reflect-metadata": "^0.1.3",
    "rxjs": "5.0.1",
    "showdown": "^1.6.4",
    "zone.js": "^0.7.2"
  },
```

import `showdown/dist/showdown.js` in _vendor.ts_

```typescript
// Angular 2
import '@angular/platform-browser';
import '@angular/platform-browser-dynamic';
import '@angular/core';
import '@angular/common';
import '@angular/http';
import '@angular/router';

import 'rxjs';
import '@angularclass/hmr';

// Other vendors for example jQuery, Lodash or Bootstrap
// You can import js, ts, css, sass, ...
import 'bootstrap/dist/css/bootstrap.css';
import "keycloak-js/dist/keycloak.js";
import "showdown/dist/showdown.js";
```

Require

```typescript
import {Component, OnInit} from "@angular/core";
import {Bookmark} from "../../model/bookmark";
import {FormGroup, FormBuilder, Validators} from "@angular/forms";
import {KeycloakService} from "../../keycloak/keycloak.service";
import {UserBookmarkStore} from "../../personal/store/UserBookmarkStore";
import {Router} from "@angular/router";
import {BookmarkService} from "../../bookmark/bookmark.service";

const showdown = require('showdown');
const converter = new showdown.Converter();
```

And then call the `makeHthml` method of the `convertor` :

```typescript
  saveBookmark(model: Bookmark) {
    model.tags = model.tagsLine.split(",");
    var newBookmark = new Bookmark(model.name, model.location, model.category,model.tagsLine.split(","), model.description, null);

    newBookmark.userId = this.userId;
    newBookmark.shared = model.shared;

    newBookmark.descriptionHtml = converter.makeHtml(newBookmark.description);

    let obs = this.userBookmarkStore.addBookmark(this.userId, newBookmark);

    obs.subscribe(
      res => {
        console.log(res);
        this.router.navigate(['/personal']);
      });
  }
```

> Same is valid for update bookamrk. See this git commit where the deltas are highlighted ..*[]:

## How to use Showdown in NodeJS?


If you found this useful, please star it, share it and improve it:

{% include source-code-codingpedia-bookmarks-frontend.html %}

## References
