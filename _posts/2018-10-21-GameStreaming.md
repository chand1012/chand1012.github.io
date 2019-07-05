---
layout: post
title: How to Stream Any Game, on Any Console, to Anywhere.
---
Ever wanted to bring your library of home console games (PC, PS4, or Xbox One) on the go? Ever wanted to play a round or two of Call Of Duty while sitting in the school library studying for a class you never payed attention to? Well, with some basic knowledge of router settings, a half decent laptop or smartphone, and an internet connection that's not dial-up, you can!

## PlayStation 4

The defacto and only way to stream your PS4 out of home are as follows: own a PS Vita/PS TV (not many people do), own a Sony-made Android Phone made within the last few years, or have a Windows/Mac PC. That last one is where I fall under, though the few of you that fall under the other two categories will follow a similar process. First forward ports UDP 9296, 9297, and 9303 to your PS4 ([this](https://portforward.com/router.htm) site will help you with that if you don't know how). It is STRONGLY recommended that you connect the PS4 up to the internet via Ethernet, as Sony does not, and apparently never will, support 5GHz (edit: this only applies to the first gen PS4, newer ones do support it). If on Windows or Mac, download and install the the Remote Play app from [here](https://remoteplay.dl.playstation.net/remoteplay/lang/en/index.html). If on one of the supported Sony Android phones, download the app from the Play Store. If you are on the Vita/PS TV, you should already have the app. After installing all you should have to do is enable Remote Play on the PS4 (explained [here](https://manuals.playstation.net/document/en/ps4/settings/remote.html)) and sign in on the app. To play any games, you do need a PS4 controller connected (Unless on Vita) either via wireless or through a cable. If you are on PC, you can buy a wireless adapter that allows you to sync your PS4 controllers wirelessly, but I personally just use a micro USB cable. After all said is done, just hit the "Start" button or press "Options" on the PS4 controller and the app will find and connect to the PS4, then enjoy playing Black Ops 4 Blackout while sitting in the back of class.

## Xbox One

I will be honest, I don't own an Xbox One. When and if I help a friend hook up their Xbox One up to their Windows 10 PC, I will update this. Before I promptly disappoint you, I will tell you what I do know. Microsoft only supports streaming to a Windows 10 PC, but there are a few ways around this. There is a paid Mac OS app that allowed streaming, but it seemed shady and will probably be pulled in a DCMA take down or something. My recommended way to play Xbox One on a computer that does not run Windows is to install a legit Windows copy in a Virtual Machine, which you can do for free. The Windows 10 ISO is freely available, doesn't need a license key to run, and you can get VM Software like Virtualbox completely free. Other than that, [here](https://kinkeadtech.com/how-to-stream-xbox-one-to-windows-10-from-anywhere-with-internet/) is a guide that will teach you how to set up Xbox One game streaming.

## PC

PC has many, MANY options available for streaming. Let's go over the ones that I have used, as most of the other ones are not as mainstream and therefore have little to no support.

### Steam In-Home Streaming
 
The name is deceiving, as you don't actually need to be in your home to stream. The advantage of this streaming platform is that it's probably installed on your PC already, unless you're the type to refuse DRM and only buy on GOG. For the most part, you will need a VPN of some sort running either on its own machine (like PiVPN) or LogMeIn Hamachi running on the host computer you wish to stream from and on the client. No port forwarding would be needed, unless you are using your own VPN, and you can use the standard steam in home streaming setup to run it.

### NVIDIA GAMESTREAM

When GAMESTREAM first came out, you needed a new-ish NVIDIA GPU and a NVIDIA GAMESTREAM client, which at the time was just limited to the NVIDIA Shield. Now NVIDIA also has the shield tablet and the Shield TV, but there are better and more versatile options available now (fingers crossed that the NVIDIA-powered Nintendo Switch gets an official GAMESTREAM app). 

[Moonlight Game Stream](https://moonlight-stream.com/) is a free and open source NVIDIA GAMESTREAM client, compatible with Windows, Mac, Linux, Android, iOS, and Raspberry Pi.  The latency is barely noticeable, and on most platforms it take only a few minutes to configure, assuming you have a graphics card listed somewhere on [this](https://shield.nvidia.com/support/shield-portable/faq/2) page. 

First, forward ports TCP 47984, 47989, 48010 and UDP 47998, 47999, 48000, 48002, 48010 to your host computer ([this](https://portforward.com/router.htm) site will help you with that if you don't know how). After, install the Moonlight Client from their official [GitHub repo](https://github.com/moonlight-stream/moonlight-qt/releases), from the Google [Play Store](https://play.google.com/store/apps/details?id=com.limelight), from the Google [Chrome Store](https://chrome.google.com/webstore/detail/moonlight-game-streaming/gemamigbbenahjlfnmlfdjhdnkpbkfjj) (Chromebooks only), or from the iOS [App Store](https://itunes.apple.com/us/app/moonlight-game-streaming/id1000551566). After the app is downloading/installing, find out your public IP from the site of your choice (typing "IP" into Google will do) and take note of it. After, start the Moonlight app on the platform of your client and making sure GAMESTREAM is enabled on your host PC, click "Add PC" on the app. It will prompt you to enter a code found on the client on the host PC, once entered you should be able to access your library of games both on your home network and on the go. When streaming over WiFi, make sure you are on the 5GHz band as it will decrease your latency and allow you to up the quality on your streaming. In the case that you have to run on 2.4GHz, put the settings on the lowest possible and keep upping them until you find a balance between quality and gameplay.

### Rainway.io

Rainway is the new kid on the block. Its made with "device agnostic streaming" in mind, with the host working on any sort of hardware config, and the client being limited to your browser at the moment, which is a plus and a minus. On the plus side, it can be played on literally anything with a standard keyboard (sorry no mobile yet, though it would theoretically work because HTML5 runs on everything) and with a half-decent internet connection. It can be run on literally any hardware, whether you're team red or blue, team red or green, or somewhere in between (rhyme not intended).

The cons are that it really doesn't perform well on any browser but Chrome, which isn't the worst thing in the world considering most people run Chrome anyway, and it has more latency than both Steam In Home streaming or NVIDIA GAMESTREAM (this may partially be due to the fact that I religiously only use Firefox). This was the kicker for me, as I like twitchy shooters like CSGO or Overwatch (Or my current favorite, Insurgency and its up-and-coming sequel). Regardless, here is how to set up Rainway.

Download the server for your host PC [here](https://rainway.io/downloads/). Make an account on Rainway's website [here](https://rainway.io/register). Login to their web client [here](https://play.rainway.io/). You're done.

That is the biggest pro and one of the main reasons to consider Rainway, its simplicity. It takes 10 minutes to set up and you're off to the races. There is some configuration to do based on your host system, but that is fairly simple. To lock the mouse (which is something you have to do if you're playing basically any game with mouse-based camera movement), press Ctrl-Shift-Z. If in the case Rainway cannot connect, you may have to port forward. If that is the case, forward ports TCP 443 and 40136, and UDP 22000–22010. You should also enable hardware acceleration for your browser, but I will leave that up to you as not to start a browser war, just for the love of computers please avoid Edge.

## Conclusion
 
That is my complete knowledge (at the moment anyway) of how to procrastinate anywhere with video games. Since the release of Black Ops 4 and since I got NVIDIA GAMESTREAM up and running I have been wasting a lot of time in my University's library not doing homework. Let's hope my grades don't show it.

Original blog post [here](http://blog.chand1012.net/2018/10/how-to-stream-any-game-on-any-console.html).
