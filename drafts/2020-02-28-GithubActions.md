---
layout: post
title: Why GitHub Actions are Really Cool.
---

## Preamble

#### If you want to get to the nitty-gritty and learn about GitHub Actions, skip this.
 
When I started developing in Golang, I wanted to have a way to automatically compile my project. When it was first concieved, and even now, it only takes a few second to compile on even something as simple as a Raspberry Pi. Being one to future proof both code and workflow, and being a big fan of automation, I wanted to find a continious intergration and deployment solution. I am in college, therefore pretty broke, so I needed a free solution.

The first thing I turned to was solutions akin to [CircleCI](https://circleci.com/) and [TravisCI](https://travis-ci.org/). I didn't really care for them that much, more being aimed at teams of people who are developing an app that needs to be deployed quickly and effciently. I am one guy, developing a small Discord Bot. I wanted to be able to get my build executable out of the builds and nothing more. Setting up TravisCI or CircleCI is not hard, I just didn't care for them.

I knew about [Jenkins](https://jenkins.io/) for a while. The first time I heard about it was about eight years ago when I was just a middle schooler modding Minecraft, and my favorite mod at the time (I have no clue which) had binaries that were downloadable from the developer's Jenkins server. My first actual use of Jenkins was at my first programming job, where we used an entire array of Jenkins servers to build and test all of our code, which sped up a lot of our workflow. Jenkins is a web-based host-your-own CI/CD server. It will run on basically anything, as its build upon Java, and will even run on a Raspberry Pi if you are a pretty patient person.

Around August of 2019, I made a Jenkins server. Jenkins is great and all, but you have to use either your own machine or have to purchase some sort of cloud hosting service, and unless you plan on running short and/or non-CPU intensive jobs, you can't just use a VPS. The builds I was doing varied wildly, going from complilation of my Discord Bot to building the Marlin 3D printer firmware, and finally build Yocto Images for my Raspberry Pi collection. For the first two, a VPS may do, or a low-power server like a Raspberry Pi 4 (4GB of RAM preferred) or the Atomic Pi (Which is just a re-branded robot control board, see [here](https://fccid.io/2AN44-AHR-M8T/Internal-Photos/Internal-photos-1-of-2-3909532)). For my Yocto builds, these wouldn't cut it. I was able to use my friend James' 12-core 24-thread server as a build server for a while, but he and I agreed that it would be better to virtualize the whole thing so we could divy up the power, as I was being a bit of a CPU-hog (though I was able to build the entire image in under an hour). 

## Ok, you talk a lot. What are GitHub Actions?

GitHub Actions are a simple and easy way to build and distribute your projects that need compiled or tested. Actions has a "store", unlike the other cloud-hosted competetors but like Jenkins, where you can install different modules into your build scripts and workflows to speed up the process of either compiling your programs or just writing your build scripts. GitHub refers to the machines that run your build scripts as "runners" and there are two types: hosted and self-hosted. Hosted are the runners that GitHub provides. There is a good summary on them [here](https://help.github.com/en/actions/reference/virtual-environments-for-github-hosted-runners), but the main things you need to know are these: each runner gets two vcores and 7GB of RAM and you get 14GB of SSD storage. You can pick between your different operating systems, but the biggest thing you should know is this: Its free if your project is open source. Free CI/CD on any project I'm willing to share? Count me in!

To define an action, you use a YAML file. This file contains all external actions you call, all of the systems you wish to build on (and yes, you can build on more than one at a time), and any installation of dependencies you need for your project (and yes, you do get `sudo` when building on macOS and Ubuntu). Unless you are hosting your own runner, you will have to install dependencies every time, as the VMs are destroyed at the end of the run. This is great if you do not want to risk anything getting changed in the OS that may affect your build, but is annoying as installed dependencies will eat your limited private minutes if you are one to hide your projects from the world. Here is an example of the file I use on my Discord Bot, also available in its latest stable iteration [here](https://github.com/chand1012/Discord-Quick-Meme/blob/master/.github/workflows/go.yml):

```YAML
name: Build Bot
on: [push]
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

    - name: Get dependencies
      run: |
        go get -v -t -d ./...
        if [ -f Gopkg.toml ]; then
            curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
            dep ensure
        fi

    - name: Build Linux x86_64
      run: GOOS=linux GOARCH=amd64 go build -v -o GoDiscordBot
    - name: Build Linux ARMv7
      run: GOOS=linux GOARCH=arm GOARM=7 go build -v -o GoDiscordBotArm
    - name: Build Windows x64
      run: GOOS=windows GOARCH=amd64 go build -v -o GoDiscordBot.exe
    - name: Archive Linux x86_64
      uses: actions/upload-artifact@v1
      with:
          name: GoDiscordBotLinux
          path: GoDiscordBot 
    - name: Archive Linux ARMv7
      uses: actions/upload-artifact@v1
      with:
          name: GoDiscordBotArm
          path: GoDiscordBotArm
    - name: Archive Windows x64
      uses: actions/upload-artifact@v1
      with:
        name: GoDiscordBotWindows
        path: GoDiscordBot.exe

```

Now I have a lot of repeats that realistically are not required, but I wanted to know what was going on in the web interface. Anyone can go on your Action and view what was executed, so don't put a password in the file (there are ways to safely input those found [here](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)). You can look at my executed actions for this project [here](https://github.com/chand1012/Discord-Quick-Meme/actions).