# docker-gradle

## Supported tags and respective Dockerfile links

* [latest, jdk8, jdk](https://github.com/keeganwitt/docker-gradle/blob/master/jdk8/Dockerfile)
* [slim, jdk8-slim, jdk-slim](https://github.com/keeganwitt/docker-gradle/blob/master/jdk8-slim/Dockerfile)
* [alpine, jdk8-alpine, jdk-alpine](https://github.com/keeganwitt/docker-gradle/blob/master/jdk8-alpine/Dockerfile)

* [jre8, jre](https://github.com/keeganwitt/docker-gradle/blob/master/jre8/Dockerfile)
* [jre8-slim, jre-slim](https://github.com/keeganwitt/docker-gradle/blob/master/jre8-slim/Dockerfile)
* [jre8-alpine, jre-alpine](https://github.com/keeganwitt/docker-gradle/blob/master/jre8-alpine/Dockerfile)

* [jdk11](https://github.com/keeganwitt/docker-gradle/blob/master/jdk11/Dockerfile)
* [jdk11-slim](https://github.com/keeganwitt/docker-gradle/blob/master/jdk11-slim/Dockerfile)

* [jre11](https://github.com/keeganwitt/docker-gradle/blob/master/jre11/Dockerfile)
* [jre11-slim](https://github.com/keeganwitt/docker-gradle/blob/master/jre11-slim/Dockerfile)

## What is Gradle?

[Gradle](https://gradle.org/) is a build tool with a focus on build automation and support for multi-language development. If you are building, testing, publishing, and deploying software on any platform, Gradle offers a flexible model that can support the entire development lifecycle from compiling and packaging code to publishing web sites. Gradle has been designed to support build automation across multiple languages and platforms including Java, Scala, Android, C/C++, and Groovy, and is closely integrated with development tools and continuous integration servers including Eclipse, IntelliJ, and Jenkins.

## How to use this image

Note that if you are mounting a volume and the uid running Docker is not _1000_, you should run as user _root_ (`-u root`).

### Building a Gradle project

Run this from the directory of the Gradle project you want to build.

`docker run --rm -v "$PWD":/home/gradle/project -w /home/gradle/project gradle:latest gradle <gradle-task>`

### Reusing the Gradle cache

The local Gradle cache can be reused across containers by creating a volume and mounting it to _/home/gradle/.gradle_.
Note that sharing between concurrently running containers doesn't work currently
(see [#851](https://github.com/gradle/gradle/issues/851)).

Also, currently it's [not possible](https://github.com/moby/moby/issues/3465) to override the volume declaration of the parent.
So if you are using this image as a base image and want the Gradle cache to be written into the next layer, you will need to use a new user (or use the `--gradle-user-home`/`-g` argument) so that a new cache is created that isn't mounted to a volume.

```
docker volume create --name gradle-cache
docker run --rm -v gradle-cache:/home/gradle/.gradle -v "$PWD":/home/gradle/project -w /home/gradle/project gradle:latest gradle <gradle-task>
```

## Instructions for a new Gradle release

1. Run `update.sh <new Gradle version>` or `update.ps1 <new Gradle version>`.
1. Commit and push the changes.
1. Update [official-images](https://github.com/docker-library/official-images) (and [docs](https://github.com/docker-library/docs) if appropriate).

---
![Travis Build Status](https://travis-ci.org/keeganwitt/docker-gradle.svg?branch=master)
