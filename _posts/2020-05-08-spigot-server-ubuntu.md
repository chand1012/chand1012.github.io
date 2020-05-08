---
layout: post
title: How to Host a Spigot Server on Ubuntu 18.04 LTS
---

In this article we will be setting up a Spigot Minecraft server on Ubuntu 18.04 LTS. Spigot is a Minecraft server that supports Vanilla (as in, not modded) clients while allowing plugins to make the user experience different. It also is more resource efficient than the default server, and allows more granular control over your users, allowing you to give them the best experience possible. 

## Requirements

- An Ubuntu 18.04 LTS server with at least 1GB of RAM
- An SSH Client

## Updates and Installing Java

After connecting to your server via SSH, we first must update all of our packages to ensure the best security:

`sudo apt-get update && sudo apt-get upgrade -y`

This will update the package index and download and install all the latest updates for your VPS. After the update is done, install the latest version of OpenJDK Java on your server:

`sudo apt-get openjdk-8-jre -y`

This will download and install the latest version of OpenJDK, which we will use to execute all of the Java executable on our machine. After the installation finishes, make a new folder. Let's call it `build`.

    mkdir build
    cd build

Next, download the latest Spigot Build Tool:

    wget https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar

After that's downloaded, execute it with Java:

    java -Xmx512M -Xms512M -jar BuildTools.jar

This will download and compile the latest Spigot Minecraft server. If you wish to specify a version, for example 1.14.4, you would execute the following: 

    java -Xmx512M -Xms512M -jar BuildTools.jar --rev 1.14.4

This will take about 15 minutes to execute. After this executes, change directory to the home directory and make a new folder for the server:

    cd ~
    mkdir server
    cd server

Then copy the executable to the directory. In this example, we will assume you made a 1.15.2 server. To copy the server, execute the following command

    cp ~/build/spigot-1.15.2.jar ./spigot-1.15.2.jar

This will copy the files to your server folder. Now we can make a startup script for your server. This will prevent you from having to enter the command over and over again to start your server. You can create this file with `nano`:

    nano start.sh

Paste the following into the start up file:

    #!/bin/bash
    echo Starting Minecraft Vultr Server...
    java -Xms512M -Xmx768M -jar spigot-1.15.2.jar

This will start the Minecraft server with 768MB of RAM. I recommend allocating 75% of the VPS memory to your Minecraft server, this should allow enough RAM to the OS to keep running, and also allow some room for players on your server.

After that is saved, and closed, allow the file to be executable with the following command: 

```
chmod +x start.sh
```

After this is complete, you can start up the server with `./start.sh`. This will run the server but only until the SSH connection is disconnected. To run this indefinitely, we can run it as a service.

## Creating a Service File

