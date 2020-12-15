---
layout: post
title: Why GitHub Actions are Really Cool.
---
## Action!

GitHub Actions are a simple and easy way to build and distribute your projects that need compiled or tested. Actions has a "store", unlike the other cloud-hosted competitors but like Jenkins, where you can install different modules into your build scripts and workflows to speed up the process of either compiling your programs or just writing your build scripts. GitHub refers to the machines that run your build scripts as "runners" and there are two types: hosted and self-hosted. Hosted are the runners that GitHub provides. There is a good summary on them [here](https://help.github.com/en/actions/reference/virtual-environments-for-github-hosted-runners), but the main things you need to know are these: each hosted runner gets two vCores and 7GB of RAM and you get 14GB of SSD storage. You can pick between your different operating systems, but the biggest thing you should know is this: Its free if your project is open source. Free CI/CD on any project I'm willing to share? Count me in!

To define an action, you use a YAML file. This file contains all external actions you call, all of the systems you wish to build on (and yes, you can build on more than one at a time), and any installation of dependencies you need for your project (and yes, you do get `sudo` when building on macOS and Ubuntu). Unless you are hosting your own runner, you will have to install dependencies every time, as the VMs are destroyed at the end of the run. This is great if you do not want to risk anything getting changed in the OS that may affect your build, but is annoying as installed dependencies will eat your limited private minutes if you are one to hide your projects from the world. Here is an example of the file I use on my Discord Bot, also available in its latest stable iteration [here](https://github.com/chand1012/Discord-Quick-Meme/blob/master/.github/workflows/go.yml):

```YAML
name: Test Bot
on: 
  [push]
jobs:

  build:
    name: Build
    runs-on: ubuntu-latest
    steps:

    - name: Set up Go 1.13
      uses: actions/setup-go@v1
      with:
        go-version: 1.13
      id: go

    - name: Check out code into the Go module directory
      uses: actions/checkout@v1

    - name: Get Dependencies
      run: |
        go get -v -t -d ./...

    - name: Runs Unit Tests
      run: |
       go test -run ''
    
    - name: Runs Benchmark Tests
      run: |
       go test -bench=.

```

Now I have a lot of repeats that realistically are not required, but I wanted to know what was going on in the web interface. Anyone can go on your Action and view what was executed (assuming your project is open source), so don't put a password in the file (there are ways to safely input those found [here](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)). You can look at my executed actions for this project [here](https://github.com/chand1012/Discord-Quick-Meme/actions).

Now this Action is relatively simple. All it does is download and install the Go toolchain to the VM, imports my code from the repo, installs all dependencies, and runs unit tests and unit benchmarks. If you had a lot of platforms and/or or compiling took more than a few second, say a few minutes or even an hour, you could make multiple similar YAML files with your build scripts in them. If you are under your maximum concurrent jobs (for a free user its 20 VMs), it will run all of them at once, otherwise they will be queued and executed when a slot is available.

## Okay, but how can I use this?

If you are a fan of compiled languages, you see a use in this right away. If you like unit testing, you see a use right away. If you are a novice programmer, or one who strictly uses interpreted languages, you are probably scratching your head, thinking "how is this useful for me?". 

