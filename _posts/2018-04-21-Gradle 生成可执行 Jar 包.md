---
title: Gradle 生成可执行 Jar 包
date: 2018-04-21 16:58:55
categories:
- Gradle
tags:
- Gradle
---
# 1. ShadowJar 打包例子

```groovy
group 'top.xiexiaodong'
version '1.0-SNAPSHOT'

apply plugin: 'java'
apply plugin: 'application'
apply plugin: 'com.github.johnrengelman.shadow'

sourceCompatibility = 1.8

repositories {
    mavenCentral()
}

buildscript {
    repositories {
        mavenCentral()
        maven {
            url "https://cdn.lfrs.sl/repository.liferay.com/nexus/content/groups/public"
        }
    }
    dependencies {
        classpath 'com.github.jengelman.gradle.plugins:shadow:2.0.2'
    }
}

dependencies {
    testCompile group: 'junit', name: 'junit', version: '4.12'
}

mainClassName = "top.xiexiaodong.example.Demo"

shadowJar {
    zip64 true
    baseName = mainClassName
    classifier = null
    version = null
}
```

详细参考文档：[http://imperceptiblethoughts.com/shadow/](http://imperceptiblethoughts.com/shadow/)
