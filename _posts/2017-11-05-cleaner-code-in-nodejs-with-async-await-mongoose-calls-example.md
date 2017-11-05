---
layout: post
title: Cleaner code in NodeJs with async-await - Mongoose calls example
description: "Example showing migration of Mongoose calls from previously using callbacks to using the new
async-await feature in NodeJs"
author: ama
permalink: /ama/cleaner-code-in-nodejs-with-async-await-mongoose-calls-example.md
published: false
categories: [nodejs]
tags: [nodejs, mongoose, async-await]
---


url commit:
https://github.com/Codingpedia/codingmarks-api/commit/0c56b456ba3185cbbc0505bcf9cdb33dfb81dbe6


This post presents the configuration snippet from nginx that redirects all request to **https://www.codingmarks.org**:

### Create

Before
```
router.post('/:id/bookmarks', keycloak.protect(), function(req, res, next){
  const descriptionHtml = req.body.descriptionHtml ? req.body.descriptionHtml: converter.makeHtml(req.body.description);

  console.log(req.body);

  var bookmark = new Bookmark({
    name: req.body.name,
    location: req.body.location,
    language: req.body.language,
    description: req.body.description,
    descriptionHtml: descriptionHtml,
    category: req.body.category,
    tags: req.body.tags,
    publishedOn: req.body.publishedOn,
    githubURL: req.body.githubURL,
    userId: req.params.id,
    shared: req.body.shared,
    starredBy: req.body.starredBy
  });

  console.log('Bookmark to create ' + bookmark);

  bookmark.save(function (err, updatedBookmark) {
    if (err){

      if(err.name == 'ValidationError'){
        var errorMessages = [];
        for (var i in err.errors) {
          errorMessages.push(err.errors[i].message);
        }

        var error = new Error('Validation MyError', errorMessages);
        console.log(JSON.stringify(error));
        res.setHeader('Content-Type', 'application/json');
        return res.status(409).send(JSON.stringify(new MyError('Validation Error', errorMessages)));
      }

      console.log(err);
      res.status(500).send(err);
    } else {
      res.set('Location', 'http://localhost:3000/' + req.params.id + '/bookmarks/' + updatedBookmark.id);
      res.status(201).send({response:'Bookmark created for userId ' + req.params.id});
    }
    // saved!
  });

});
```

After 
```
router.post('/:id/bookmarks', keycloak.protect(), async (req, res) => {
  const descriptionHtml = req.body.descriptionHtml ? req.body.descriptionHtml: converter.makeHtml(req.body.description);

  const bookmark = new Bookmark({
    name: req.body.name,
    location: req.body.location,
    language: req.body.language,
    description: req.body.description,
    descriptionHtml: descriptionHtml,
    category: req.body.category,
    tags: req.body.tags,
    publishedOn: req.body.publishedOn,
    githubURL: req.body.githubURL,
    userId: req.params.id,
    shared: req.body.shared,
    starredBy: req.body.starredBy
  });

  try{
    let newBookmark = await bookmark.save();

    res.set('Location', 'http://localhost:3000/' + req.params.id + '/bookmarks/' + newBookmark.id);
    res.status(201).send({response:'Bookmark created for userId ' + req.params.id});

  } catch (err){
    if (err.name === 'MongoError' && err.code === 11000) {
      res.status(409).send(new MyError('Duplicate key', [err.message]));
    }

    res.status(500).send(err);
  }

});
```

### Read

#### Before
```
/* GET bookmarks for user */
router.get('/:id/bookmarks', keycloak.protect(), function(req, res, next) {
  if(req.query.term){
    var regExpTerm = new RegExp(req.query.term, 'i');
    var regExpSearch=[{name:{$regex:regExpTerm}}, {description:{$regex: regExpTerm }}, {category:{$regex:regExpTerm }}, {tags:{$regex:regExpTerm}}];
    Bookmark.find({userId:req.params.id, '$or':regExpSearch}, function(err, bookmarks){
      if(err){
        return res.status(500).send(err);
      }
      res.send(bookmarks);
    });
  } else {//no filter - all bookmarks
    Bookmark.find({userId:req.params.id}, function(err, bookmarks){
      if(err){
        return res.status(500).send(err);
      }
      res.send(bookmarks);
    });
  }

});
```

#### After
```
/* GET bookmarks for user */
router.get('/:id/bookmarks', keycloak.protect(), async (req, res) => {
  try{
    let bookmarks;
    if(req.query.term){
      var regExpTerm = new RegExp(req.query.term, 'i');
      var regExpSearch=[{name:{$regex:regExpTerm}}, {description:{$regex: regExpTerm }}, {category:{$regex:regExpTerm }}, {tags:{$regex:regExpTerm}}];
      bookmarks = await Bookmark.find({userId:req.params.id, '$or':regExpSearch});
    } else {//no filter - all bookmarks
      bookmarks = await Bookmark.find({userId:req.params.id});
    }

    res.send(bookmarks);
  } catch (err) {
    return res.status(500).send(err);
  }
});
```

### Update

#### Before
```
/**
 * full UPDATE via PUT - that is the whole document is required and will be updated
 * the descriptionHtml parameter is only set in backend, if only does not come front-end (might be an API call)
 */
router.put('/:userId/bookmarks/:bookmarkId', keycloak.protect(), function(req, res, next) {
  if(!req.body.descriptionHtml){
    req.body.descriptionHtml = converter.makeHtml(req.body.description);
  }
  Bookmark.findOneAndUpdate({_id: req.params.bookmarkId, userId: req.params.userId}, req.body, {new: true}, function(err, bookmark){
    if(err){
      if (err.name === 'MongoError' && err.code === 11000) {
        res.status(409).send(new MyError('Duplicate key', [err.message]));
      }

      res.status(500).send(new MyError('Unknown Server Error', ['Unknow server error when updating bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId]));
    }
    if(!bookmark){
      return res.status(404).send('Bookmark not found for user');
    }
    res.status(200).send(bookmark);
  });

});
```

#### After
```
/**
 * full UPDATE via PUT - that is the whole document is required and will be updated
 * the descriptionHtml parameter is only set in backend, if only does not come front-end (might be an API call)
 */
router.put('/:userId/bookmarks/:bookmarkId', keycloak.protect(), async (req, res) => {
  if(!req.body.descriptionHtml){
    req.body.descriptionHtml = converter.makeHtml(req.body.description);
  }
  try {
    let bookmark = await Bookmark.findOneAndUpdate({_id: req.params.bookmarkId, userId: req.params.userId}, req.body, {new: true});

    if (!bookmark) {
      return res.status(404).send(new MyError('Not Found Error', ['Bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId + ' not found']));
    } else {
      res.status(200).send(bookmark);
    }
  } catch (err) {
    if (err.name === 'MongoError' && err.code === 11000) {
      res.status(409).send(new MyError('Duplicate key', [err.message]));
    }
    res.status(500).send(new MyError('Unknown Server Error', ['Unknow server error when updating bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId]));
  }
});
```

### Delete

#### Before
```
/*
* DELETE bookmark for user
*/
router.delete('/:userId/bookmarks/:bookmarkId', keycloak.protect(), function(req, res, next) {
  Bookmark.findOneAndRemove({_id: req.params.bookmarkId, userId: req.params.userId}, function(err, bookmark){
    if(err){
      return res.status(500).send(new MyError('Unknown server error', ['Unknown server error when trying to delete bookmark with id ' + req.params.bookmarkId]));
    }
    if(!bookmark){
      return res.status(404).send(new MyError('Not Found Error', ['Bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId + ' not found']));
    }
    res.status(204).send('Bookmark successfully deleted');
  });

});
```

#### After
```
/*
* DELETE bookmark for user`
*/
router.delete('/:userId/bookmarks/:bookmarkId', keycloak.protect(), async (req, res) => {
  try {
    let bookmark = await Bookmark.findOneAndRemove({_id: req.params.bookmarkId, userId: req.params.userId});

    if (!bookmark) {
      return res.status(404).send(new MyError('Not Found Error', ['Bookmark for user id ' + req.params.userId + ' and bookmark id '+ req.params.bookmarkId + ' not found']));
    } else {
      res.status(204).send('Bookmark successfully deleted');
    }
  } catch (err) {
    return res.status(500).send(new MyError('Unknown server error', ['Unknown server error when trying to delete bookmark with id ' + req.params.bookmarkId]));
  }
});
``

{% include source-code-codingpedia-bookmarks.html %}

