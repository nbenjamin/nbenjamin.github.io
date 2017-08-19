---
layout: post
title: Load files from folders as a resources using spring
categories: [Spring]
tags: [Spring]
description: Load files from folders as a resources using spring.
---

Here is the code snippet to load files from classpath
```java
@Autowired
private ResourceLoader resourceLoader;
Resource[] resources  = ResourcePatternUtils.getResourcePatternResolver(resourceLoader).getResources
    ("classpath*:data/*.json");
```
and make sure you import spring libraries
```java
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.core.io.support.ResourcePatternUtils;
```
