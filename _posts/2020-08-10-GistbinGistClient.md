---
layout: post
title: GistBin - The GitHub Gist command line client.
---

So I have always like the idea of being able to share simple snippets of code, but my first real foray into this was using [PasteBin](https://pastebin.com/). The reason I was using PasteBin was because that is what the Minecraft mod, ComputerCraft, allowed you to download code off of the internet easily. I wrote a few programs that are still on [my PasteBin](https://pastebin.com/u/chand1012). Since I found out that GitHub had a competitor in the form of GitHub's Gists, I switched to that. I like the GitHub platform and its easy to switch between the gists and Git, and even clone the gists with `git clone`. I recently found [HasteBin](https://hastebin.com/about.md), an open source alternative to PasteBin that has a command-line tool. The command-line tool, called [haste-client](https://github.com/seejohnrun/haste-client) is extremely easy to use, only requiring you to pipe in the information you want to upload to HasteBin. You can even host your own [HasteBin Server](https://github.com/seejohnrun/haste-server) and point the haste-client to it. While I liked this, I also liked the ability to have other information about the code that I was sharing, along with being able to look at revisions like git, but haste-client is *so easy* to use, so I decided to create my own client for Gists.

# GistBin

[GistBin](https://gistbin.chand1012.dev/) is my alternative. This allows you to upload files to GitHub's Gist platform from the command line by piping the text into the program. Here is an example:

```
octocat@octoserv:~$ cat hello_world.rb | gistbin
https://gist.github.com/octocat/6cad326836d38bd3a7ae
```

You can also pipe any text from the terminal into it, for example if you wanted to list all files in a directory, you could run this.

```
chandler@BlueServer:~$ ls -1 | gistbin -n "home_dir_example" -d "Example of listing your home directory"
https://gist.github.com/chand1012/c9cfe757c1577b347eae6f3dd130e134
chandler@BlueServer:~$
```

The `-n`/`--name` and the `-d`/`--desc` flags can be used to name the file and add a description to your gists, respectively. If these are omitted, like in the former example, a random name will be generated with the extension of `.txt` and the description will be left blank. You can also get the raw text URL of the gist with the `-r` flag.

```
chandler@BlueServer:~$ ls -1 | gistbin -n "home_dir_example" -d "Example of listing your home directory" -r
https://gist.githubusercontent.com/chand1012/c9cfe757c1577b347eae6f3dd130e134/raw/d8203f2b7026fb27bef0d08cff1765cc2cb2088d/home_dir_example
chandler@BlueServer:~$
```

This is useful for when you are copying and pasting the URL to be downloaded on another system. 

While I think my tool is useful, and I hope others find it so too, there are a sacrifices I had to make to get it working, the biggest being that it does require that you have a GitHub user authenticated. You can log in with the following:

```
octocat@octoserv:~$ gistbin --login
Enter GitHub username: octocat
Enter GitHub access token: ****************
Keyfile saved.
octocat@octoserv:~$ 
```

On the plus side, Gistbin uses `pip` for installation, which can easily be installed on most Linux and Unix systems. To install, simply run `pip3 install gistbin` and you can then run it!

Currently I only *know* it works on Linux, but it should work on most Unix systems, OS X included. Windows is not supported at the current moment.

Hope you all find this useful!