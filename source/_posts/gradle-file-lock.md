title: gradle file lock
date: 2015-07-29 10:30:41
tags:
---
运行gradle idea 下载依赖的时候报下面的错误
```
Timeout waiting to lock buildscript class cache for build file '/Users/twer/works/javaee/core/build.gradle' (/Users/twer/.gradle/caches/2.2.1/scripts/build_dqoptxiza8a734uwm8mpmbw5h/ProjectScript/buildscript). It is currently in use by another Gradle instance.
     Owner PID: unknown
     Our PID: 4202
     Owner Operation: unknown
     Our operation: Initialize cache
     Lock file: /Users/twer/.gradle/caches/2.2.
     1/scripts/build_dqoptxiza8a734uwm8mpmbw5h/ProjectScript/buildscript/cache.properties.lock
```
这是因为我还开着其他的gradle 服务（jetty），关闭后gradle就可以正常下载依赖了。