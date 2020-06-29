---
layout: post
title: Why Discord Bot Development is Flawed.
---

Before I complain about my experience with Discord bots, let me preamble with this: I enjoy developing the bots. I enjoy making bots that entertain people and that everyone uses for fun and memes. I like my Discord bots, I don't regret developing them, and I will continue development of them. The thing I am going to be complaining about is mostly pertaining to one issue, but there may be others.

# Issues, Summarized.

My biggest complaint, and the one that I have seen done in what I would call a better way, is how the Discord bot connects to their servers and gets data. Currently, Discord uses a persistent connection to their servers, giving the bot a stream of messages, to which it is the programmer's responsibility to correctly sort through and program behaviors. This is all fine when your bot is only on a few channels, but the second you start getting big, you have a problem of an endless stream of data coming into your bot. Every message must be scanned for your command, and there is no way of verifying that two bots may or may not have the same commands. This can cause issues, such as when one of your users is attempting at using another bot, but because your bot and the other bot on the server have the same command, confusion and frustration occur. There is no system implemented to verify which command the user meant, and it is the server owner's responsibility to verify that the installed bots don't conflict with each other. Finally, there is no "official" Discord bot repository or "store". This means that the task to installing bots falls to websites like [top.gg](https://top.gg/) or [discord.bots.gg](https://discord.bots.gg/), which while good websites, are not official.