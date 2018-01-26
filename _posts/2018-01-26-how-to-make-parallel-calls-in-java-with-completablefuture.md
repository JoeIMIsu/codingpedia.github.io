---
layout: post
title: How to make parallel calls in Java with CompletableFuture
description: "This blog post presents how to make parallel calls in Java with CompletableFuture. 
It makes asynchronous calls, sort of like the async await and promise.all pattern..."
author: ama
permalink: /ama/how-to-make-parallel-calls-in-java-with-completablefuture
published: false
categories: [java]
tags: [java, async]
---

```java
import org.codingpedia.example;

import javax.inject.Inject;
import java.util.List;
import java.util.concurrent.CompletableFuture;
import java.util.function.Supplier;
import java.util.stream.Collectors;

public class ParallelCallsDemoService {

    @Inject
    RestApiClient restApiClient;

    public List<ToDo> getToDos(List<String> ids){

        List<CompletableFuture<ToDo>> futures =
                ids.stream()
                          .map(id -> getToDoAsync(id))
                          .collect(Collectors.toList());

        List<ToDo> result =
                futures.stream()
                        .map(CompletableFuture::join)
                        .collect(Collectors.toList());

        return result;
    }


    CompletableFuture<ToDo> getToDoAsync(String id){

        CompletableFuture<ToDo> future = CompletableFuture.supplyAsync(new Supplier<ToDo>() {
            @Override
            public ToDo get() {
                final ToDo timeSeriesForRegister = restApiClient.getTimeSeries(id);

                return timeSeriesForRegister;
            }
        });

        return future;
    }

}
```



