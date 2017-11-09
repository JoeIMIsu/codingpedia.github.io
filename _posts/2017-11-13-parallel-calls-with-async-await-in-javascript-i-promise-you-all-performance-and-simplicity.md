---
layout: post
title: Parallel calls with async-await in javascript - I promise you all performance and simplicity
description: "I was blown away about the peformance gain and simplicity of making parallel calls with the new async-await feature in javascript
. See the blog post for details"
author: ama
permalink: /ama/parallel-calls-with-async-await-in-javascript-i-promise-you-all-performance-and-simplicity
published: false
categories: [javascript, nodejs]
tags: [javascript, nodejs, async-await, typescript]
---

Some days ago, I presented how the code got much cleaner when replaced Mongoose callbacks with the async-await feature
 from ES7, which NodeJs already support. I mentioned then I was gonna do a post to demonstrate how async-await can be 
  used to make parallel calls. I was actually blown away this week by its simplicity and performance gain when doing so.
  Let me show you what I mean.
  
  <!--more-->

## Usecase
Use a notification service (might be a REST service) to send emails to all employees of a company - `sendEmailNotification(user)`

### Before
```typescript
    async notifyEmployees(organisationId: string): Promise<string> {

        const employees:User[] = await this.getEmployeesOfOrganisation(organisationId);

        for(let i = 0; i < employees.length; i++) {
            this.sendEmailNotification(employees[i]);
        }
        return "Sent email to employees of Company with id " + organisationId;
    }
```

### After
```typescript
    async notifyEmployees(organisationId: string): Promise<string> {

        const employees:User[] = await this.getEmployeesOfOrganisation(organisationId);

        let emailNotificationPromises:any = [];
        for(let i = 0; i < employees.length; i++) {
                emailNotificationPromises.push(this.sendEmailNotification(employees[i]));
        }

        await Promise.all(emailNotificationPromises);

        return "Sent email to employees of Company with id " + organisationId;
    }
```

Notice how simple is with the help of the [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
we can aggregate the result of multiple promises. The performance gain was fantastic from around 40s for sending email to
30 users to around 1s...
   
Stay tuned for other cool async-await features I might find along the way...
