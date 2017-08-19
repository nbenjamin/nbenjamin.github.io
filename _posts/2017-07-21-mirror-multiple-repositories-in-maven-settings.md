---
layout: post
title: Mirror multiple repositories in maven settings.xml
categories:
  - Maven
tags: [Maven]
---
 
Here is the configuration to enable multiple [mirrors](https://maven.apache.org/guides/mini/guide-mirror-settings.html) in maven (`settings.xml`). 
Mirrors block
```xml
<mirrors>
    <mirror>
        <id>nexus</id>
        <mirrorOf>*</mirrorOf>
        <url>https://nexus.organization.com/nexus/content/groups/CLM</url>
    </mirror>
    <mirror>
        <id>art</id>
        <mirrorOf>artifactory</mirrorOf>
        <url>https://artifactory.organization/artifactory/maven-internalfacing</url>
    </mirror>
</mirrors>
```

update the profiles block
```xml
<profiles>
    <profile>
        <id>nexus</id>
        <repositories>
            <repository>
                <id>central</id>
                <name>central</name>
                <url>https://nexus.organization.com/nexus/maven2</url>
            </repository>

            <repository>
                <id>art</id>
                <name>art</name>
                <url>https://artifactory.organization/artifactory/maven-internalfacing</url>
            </repository>
        </repositories>
    </profile>
</profiles>
```
