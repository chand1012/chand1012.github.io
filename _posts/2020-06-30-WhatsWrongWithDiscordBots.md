---
layout: post
title: Why Discord Bot Development is Flawed.
---

Before I complain about my experience with Discord bots, let me preamble with this: I enjoy developing the bots. I enjoy making bots that entertain people and that everyone uses for fun and memes. I like my Discord bots, I don't regret developing them, and I will continue development of them. I do not think Discord's current system for bot development should be replaced, it is too prevalent and there is too many bots currently using it.

## Issues, Summarized.

My biggest complaint, and the one that I have seen done in what I would call a better way, is how the Discord bot connects to their servers and gets data. Currently, Discord uses a persistent connection to their servers, giving the bot a stream of messages, to which it is the programmer's responsibility to correctly sort through and program behaviors. This is all fine when your bot is only on a few channels, but the second you start getting big, you have a problem of an endless stream of data coming into your bot. Every message must be scanned for your command, and there is no way of verifying that two bots may or may not have the same commands. This can cause issues, such as when one of your users is attempting at using another bot, but because your bot and the other bot on the server have the same command, confusion and frustration occur. There is no system implemented to verify which command the user meant, and it is the server owner's responsibility to verify that the installed bots don't conflict with each other. Finally, there is no "official" Discord bot repository or "store". This means that the task to installing bots falls to websites like [top.gg](https://top.gg/) or [discord.bots.gg](https://discord.bots.gg/), which while good websites, are not official. 

## Connection Issues

I get why Discord decided to use a persistent connection over using another system, I do. Its because the bot is essentially emulating a Discord client, getting all messages sent in the channels that the "user", or in this case the bot, and rather than putting them aside for the user to read later, streams it to the bot. The main issue that I do not like is the requirement for the bot to be running 100% of the time on *some* sort of computer. Now I *do* like that this computer can be anything with an internet connection, and that no ports have to be forwarded. Memory and CPU use heavily depend on the library that you use, along with the language. The variance between libs and languages can be **orders of magnitude**. I originally wrote my [DiscordQuickMeme](https://github.com/chand1012/Discord-Quick-Meme) bot in Python using [discord.py](https://discordpy.readthedocs.io/en/latest/), and at peak hours with around 40k users RAM use was well over 1GB. The same Discord bot, with RAM caching added, i.e. post titles, links, and scores from Reddit were all cached in RAM, peaked at 100MB of use when coded in Golang with Discordgo. Literally all of my programmer friends attributed to Python's lack of efficient RAM use and because Golang has a great garbage collection system. 

## Command Overlap

Another issue that I only actually came across once was command overlap. It seems that most server admins are good about making sure that command overlap is minimal, but it has happened before. Now, the command in question was called `!search` by both my bot and the other one in question, which I just changed my command to `!revsearch` as it was a reverse image search command. I think I have a solution to both this and the previous issue, but I will talk about that later.

## Bot Sources

Discord does not have an official store for bots or extensions. While this isn't an immediate problem, I think that they would benefit greatly, as there are users who wouldn't know what to trust or not to trust. Simple as that. 

## The Solution

My solution already exists in another popular instant messaging service, and one that is more oriented at businesses: Slack. Slack's method for creating a bot is very different in its structure. Rather than having the programmer scan each message for the commands, Slack sends a REST request to a web app with a matching API key. This means two things: 1) The app doesn't have a persistent connection to the server at any given time and 2) You can develop the app to be serverless. 

Serverless? Why? Because Functions-as-a-Service exist! For the most part, the average Slack bot can host themselves for free using AWS Lambda or Cloudflare Workers using webhooks to trigger commands. If this was used for Discord, bots could be designed using a simple web framework such as [Flask](https://flask.palletsprojects.com/en/1.1.x/) or [Sinatra](sinatrarb.com), both of which are supported by AWS Lambda. Cloudflare Workers only supports Javascript and WASM, but that may be all you need depending on the complexity of the Discord Bot. 

Now, as stated before, I don't think Discord should just throw away their current method for bot development, that would upset their bot devs and their users too much. Rather, I think that they should offer this development cycle as an option that can be used in place of the current one. 

What about conflicting commands? Well this would be handled by the Discord Client on the user's side, allowing the client to differentiate which bot they meant by some sort of context bot or similar. That way the users will always use what they mean. Another possible solution would be to disallow bots with the same command all together, either making each one on a server have a unique prefix (kind of like how my Discord bot has a `quickmeme` command for system commands) that is unique to each bot, and that can be aliased if they are conflicting on a specific server.

## Conclusion

This was my rant about Discord, the platform that I use literally every day to communicate with my friends, classmates, and coworkers. I love the platform, and I love developing new ways for users to use Discord to interact with their friends and the internet. However, I think that their bot development cycle could use some work, and I just felt like complaining today. That is all, hope you enjoyed my complaining.
