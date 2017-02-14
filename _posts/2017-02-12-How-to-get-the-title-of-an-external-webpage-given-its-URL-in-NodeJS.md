---
layout: post
title: How to get the title of an external web page given its URL from NodeJS
description: "This post shows how to dynamically fill in data in a reactive form field, based on other field's data"
author: ama
permalink: /ama/patching-my-way-through-formgroups-and-formcontrols-in-angular-reactive-forms
published: false
categories: [nodejs]
tags: [coding bookmarks, web scrapping, cheerio, angular]
---

Not long time ago I wrote my first Angular post, . I think it's a good time to write a NodeJS/Express post, then I'll write one about Mongo, and
have a complete starting kit for MEAN posts...

Initial problem
get the

the work done


the solution

{% include source-code-codingpedia-bookmarks-frontend.html %}

<!--more-->

## The component template

The HTML template is pretty standard. I use reactive forms[^1] to get the data:

[^1]: <https://angular.io/docs/ts/latest/cookbook/form-validation.html#!#reactivel>

```js
var express = require('express');
var request = require('request');
var cheerio = require('cheerio');
var router = express.Router();
var Bookmark = require('../models/bookmark');
var MyError = require('../models/error');

/* GET title of bookmark given its url */
router.get('/scrape', function(req, res, next) {
  if(req.query.url){
    request(req.query.url, function (error, response, body) {
      if (!error && response.statusCode == 200) {
        var $ = cheerio.load(body);
        var webpageTitle = $("title").text();
        var webpage = {
          title: webpageTitle
        }
        res.send(webpage);
      }
    });
  }
});
```

If you found this useful, please star it, share it and improve it:

{% include source-code-codingpedia-bookmarks-frontend.html %}

> A note on web scraping from Wikipedia - "the legality of web scraping varies across the world. In general, web scraping may be against the terms of use of some websites, but the enforceability of these terms is unclear"[^]. Since I am using
the method to get the title for bookmarking, hope I don't do anything illegal, but be wary when trying to get more data...

[^]: <https://en.wikipedia.org/wiki/Web_scraping#Legal_issues>

## References
