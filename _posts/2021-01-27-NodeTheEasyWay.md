---
layout: post
title: How to Install Node on Linux the Easy Way.
---

I recently started working on a few major NodeJS project, and found that installing the latest LTS release could be rather cumbersome on Linux. The application we're developing is being hosted on Heroku, and will not be using a Docker container, and for a few reasons we would rather develop locally rather than in a container. Here is how I installed NodeJS and NPM on my Linux installations, and I found it quite easy. While this tutorial will be using Ubuntu's `apt` and Arch Linux's `pacman`, the process should be similar for most distributions. See [here](https://nodejs.org/en/download/package-manager/) for more information about installing NodeJS and NPM via your package manager.

First, install whatever versions of NodeJS and NPM are available in your distro's package manager. For example, this is how you would install on Ubuntu 20.04.

```Bash
sudo apt update
sudo apt install nodejs npm
```

This would be the command for Arch Linux.

```Bash
sudo pacman -Syu nodejs npm
```

```Bash
```

This will install a (probably) outdated version of NodeJS and NPM. 

Next, you need to update NPM. To update NPM, simply run the following.

```Bash
sudo npm i -g npm
```

Then, install the package [`n`](https://www.npmjs.com/package/n). This is a nice NodeJS installation manager similar to Ruby's [RVM](https://rvm.io/).

```Bash
sudo npm i -g n
```

To use `n` to install the latest or latest long-term support version of Node.

```Bash
sudo n latest # for the newest version
sudo n lts # for the latest LTS
```

If you are just getting started, or you don't know which to install, I *highly* recommend you install the LTS version, as it is the most stable and has the best package support.

To finish the install, you either have to log out and log back into your terminal, or you can execute `PATH=$PATH`. 

That's it! You now have the latest version (or LTS version) of NodeJS and NPM installed on your Linux machine, ready to start your latest big project!