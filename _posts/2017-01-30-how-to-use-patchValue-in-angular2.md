---
layout: post
title: How to use patchValue
description: "This post is an example on how to deploy a Java EE application to a WildFly or JBoss EAP 7 via the WildFly Maven Plugin"
author: ama
permalink: /ama/how-to-deploy-an-application-on-wildfly-or-jboss-eap-7-via-the-wildfly-maven-plugin
published: false
categories: [angular]
tags: [forms, reactive forms]
---

So here goes my first Angular post - yeeey. Well, it's not super exciting, but practical and specific, I mean that's still something. Topic - generic: present how to update a field's value in a reactive form field,
once a value in another field is given. Topic - concrete: there is a **personal** bookmarks section on [https://bookmarks.codingpedia.org](https://bookmarks.codingpedia.org); when you add a new bookmark and you fill in the location field
with an URL, the title field is being automatically filled in, by scraping the page for its title. Of course later you have the possibility to change it.

<!--more-->

## Component template

The HTML template is pretty standard. I use reactive forms[^1] to get the data:

[^1]: <https://angular.io/docs/ts/latest/cookbook/form-validation.html#!#reactivel>

```javascript
<div class="container">
  <div class="col-md-8 col-md-offset-2">
    <form [formGroup]="bookmarkForm" novalidate (ngSubmit)="saveBookmark(bookmarkForm.value, bookmarkForm.valid)">
      <div class="form-group">
        <label for="location">Location*</label>
        <input type="text" class="form-control" id="location"
               required
               name="location"
               formControlName="location"
               placeholder="Usually an URL">
        <div [hidden]="bookmarkForm.controls.location.valid || (bookmarkForm.controls.location.pristine && !submitted)"
             class="alert alert-danger">
          Location is required
        </div>
      </div>
      <div class="form-group">
        <label for="name">Title*</label>
        <input type="text" class="form-control" id="name"
               required
               formControlName="name"
               placeholder="Name of the bookmark to recognize later">
        <div [hidden]="bookmarkForm.controls.name.valid || (bookmarkForm.controls.name.pristine && !submitted)"
             class="alert alert-danger">
          Name is required
        </div>
      </div>
      <div class="form-group">
        <label for="tags">Tags* - comma separated</label>
        <input type="text" class="form-control" id="tags"
               formControlName="tagsLine"
               pattern="^[-\w\s]+(?:,[-\w\s]*)*$"
               placeholder='keywoards - comma separated (e.g. "jvm, regex")'>
        <div [hidden]="bookmarkForm.controls.tagsLine.valid || (bookmarkForm.controls.tagsLine.pristine && !submitted)"
             class="alert alert-danger">
          Please provide the tags separated with comma
        </div>
      </div>
      <div class="form-group">
        <label for="description">Notes...</label>
        <textarea class="form-control"
                  id="description"
                  formControlName="description"
                  placeholder="Make some notes to help you later by searching...">
        </textarea>
      </div>
      <div class="form-check">
        <label class="form-check-label">
          <input class="form-check-input" type="checkbox" value="true" formControlName="shared">
          <strong> Make this bookmark public so others can benefit from it </strong>
        </label>
      </div>
      <button type="submit" class="btn btn-default" [disabled]="!bookmarkForm.valid">Submit</button>
    </form>
  </div>
</div>
```

## Component class

```javascript
export class UserBookmarkFormComponent implements OnInit {

  model = new Bookmark('', '', '', [], '');
  bookmarkForm: FormGroup;
  userId = null;

  constructor(
    private userBookmarkStore: UserBookmarkStore,
    private router: Router,
    private formBuilder: FormBuilder,
    private keycloakService: KeycloakService,
    private bookmarkService: BookmarkService
  ){
    const keycloak = keycloakService.getKeycloak();
    if(keycloak) {
      this.userId = keycloak.subject;
    }
  }

  ngOnInit(): void {

    this.bookmarkForm = this.formBuilder.group({
      name: ['', Validators.required],
      location: ['', Validators.required],
      tagsLine:['', Validators.required],
      description:'',
      shared: false
    });

    this.bookmarkForm.valueChanges
      .debounceTime(600)
      .distinctUntilChanged()
      .subscribe(formData => {
        if(formData.location && !formData.name){
          console.log('location changed', formData);
          this.bookmarkService.getBookmarkTitle(formData.location).subscribe(response => {
            if(response){
              this.bookmarkForm.controls['name'].patchValue(response.title, {emitEvent : false});
            }
          });
        }
      });
  }

  saveBookmark(model: Bookmark, isValid: boolean) {
    model.tags = model.tagsLine.split(",");
    var newBookmark = new Bookmark(model.name, model.location, model.category,model.tagsLine.split(","), model.description);

    newBookmark.userId = this.userId;
    newBookmark.shared = model.shared;

    let obs = this.userBookmarkStore.addBookmark(this.userId, newBookmark);

    obs.subscribe(
      res => {
        this.router.navigate(['/personal']);
      });
  }
}

```

In the component class I declare a form builder[^2] with a couple of validators:
[^2]: <https://angular.io/docs/ts/latest/api/forms/index/FormBuilder-class.html>


```javascript
this.bookmarkForm = this.formBuilder.group({
  name: ['', Validators.required],
  location: ['', Validators.required],
  tagsLine:['', Validators.required],
  description:'',
  shared: false
});
```


The more elegant way:
```javascript
this.bookmarkForm.controls['location'].valueChanges
  .debounceTime(800)
  .distinctUntilChanged()
  .subscribe(location => {
    console.log('Location: ', location);
    this.bookmarkService.getBookmarkTitle(location).subscribe(response => {
      if(response){
        this.bookmarkForm.controls['name'].patchValue(response.title, {emitEvent : false});
      }
    });
  });
```

Note:

* **undeploy** - is bound to the `clean` phase, and it won't complain if it's not present
* **deploy** - is bound to the `install` phase
* if you don't want the deployment to happen in the installation phase, consider moving it in an upper phase  in the the maven build cycle[^2] (e.g. `deploy`) or enclose it in a profile; for the latter see the second example below



## References
