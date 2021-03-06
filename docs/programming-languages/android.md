# Android

This guide will help you get started with an Android project on Semaphore.
If you’re new to Semaphore please read our
[guided tour](https://docs.semaphoreci.com/article/77-getting-started) first.

## Overview

Semaphore provides a set of tailored container images that Android developers
can use as an environment for their projects on Semaphore. These pre-built
images provide Android SDK and other common tools needed to run Android projects
out of the box. Your task is only to configure CI/CD steps that are specific for
your project.

Semaphore supports both Android and React Native projects.  

You can run an Android emulator in Semaphore CI/CD jobs.

## Example configuration

Here's an example Android configuration:

```yaml
version: v1.0
name: Android example
agent:
  machine:
    type: e1-standard-2
  containers:
    - name: main
      image: semaphoreci/android:29
blocks:
  - name: Build
    task:
      jobs:
        - name: Hello Android
          commands:
            - checkout
            - echo "hello world"
```

Here's an example configuration for React Native projects:

```yaml
version: v1.0
name: React Native example
agent:
  machine:
    type: e1-standard-2
  containers:
    - name: main
      image: semaphoreci/android:29-node
blocks:
  - name: Install dependencies
    task:
      jobs:
        - name: npm install and cache
          commands:
            # Get the latest version of our source code from GitHub:
            - checkout
            # Restore dependencies from cache. This command will not fail in
            # case of a cache miss. In case of a cache hit, npm install will
            # run very fast.
            # For more info on caching, see https://docs.semaphoreci.com/article/68-caching-dependencies
            - cache restore
            - npm install

            # Store the latest version of node modules in cache to reuse in
            # further blocks:
            - cache store
  - name: Run Android tests
    task:
      prologue:
        commands:
          - checkout
          # Install Android emulator
          - sdkmanager "platform-tools" "platforms;android-24" "emulator"
          - sdkmanager "system-images;android-24;default;armeabi-v7a"
          - echo no | avdmanager create avd -n Nexus_S_API_24 -k "system-images;android-24;default;armeabi-v7a" --device "Nexus S"
      jobs:
        - name: Run tests
          commands:
            - cache restore
            - npm test
```

## Android image variants

The pre-built Android images are available on [SemaphoreCI Docker
Hub](https://hub.docker.com/repository/docker/semaphoreci/android).

The source code and Dockerfiles for these images are available in [an open
source repository
semaphoreci/docker-images](https://github.com/semaphoreci/docker-images/tree/master/dockerfiles/android).

The Semaphore Android images are based on the `ubuntu:16.04` official Docker
image. Jobs commands are executed by root user.

There are Docker images for each recent version of Android. There are also
variants with Node.js preinstalled for React Native development.

 - `android:version` is the basic image that comes with development utilities
   and is an optimal first choice.
 - `android:version-node` is an extension of the basic image and comes with
   Node.js (10.x) pre-installed.

For more information on using Docker images to define your Semaphore CI/CD
environment, see [Custom CI/CD environment with
Docker](https://docs.semaphoreci.com/article/127-custom-ci-cd-environment-with-docker)
documentation page.

## Example projects

Semaphore provides a demo React Native application with a working
CI pipeline for Android and iOS that you can use to get started quickly:

- [Demo project on GitHub][react-native-demo-project]

## Supported Android SDK versions

- build-tools;25.0.3
- build-tools;26.0.1
- build-tools;26.0.2
- build-tools;27.0.0
- build-tools;27.0.1
- build-tools;27.0.2
- build-tools;27.0.3
- build-tools;28.0.0
- build-tools;28.0.1
- build-tools;28.0.2
- build-tools;28.0.3
- build-tools;29.0.0
- build-tools;29.0.1
- build-tools;29.0.2
- platforms;android-25
- platforms;android-26
- platforms;android-27
- platforms;android-28
- platforms;android-29

### Installed software

- ARM simmulator (libqt5widgets5)
- Google Cloud SDK
- apt
- apt-transport-https
- build-essential
- bzip2
- ca-certificates
- curl
- docker
- docker-compose
- git
- gnupg
- gradle
- gzip
- jq
- lftp
- locales
- lsb-release
- maven
- mercurial
- net-tools
- netcat
- openssh-client
- parallel
- ruby 2.6.1
- software-properties-common
- sudo
- tar
- tree
- unzip
- vim
- wget
- xvfb
- zip

## See Also

- [iOS Continuous Integration guide](https://docs.semaphoreci.com/article/124-ios-continuous-integration-xcode)
- [sem command line tool reference](https://docs.semaphoreci.com/article/53-sem-reference)
- [Toolbox reference page](https://docs.semaphoreci.com/article/54-toolbox-reference)
- [Pipeline YAML reference](https://docs.semaphoreci.com/article/50-pipeline-yaml)

[react-native-demo-project]: https://github.com/semaphoreci-demos/semaphore-demo-react-native
