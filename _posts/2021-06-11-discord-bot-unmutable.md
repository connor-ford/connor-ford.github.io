---
title:  "Making Myself Unmutable in Discord"
date:   2021-06-11 11:41:00 -0500
categories: [Computers, Bots]
tags: [python, random-discord-bot]
---

## Intro

My friends call it a "misuse of my Discord bot-creating powers". I like to think of it as a fun learning experience.

If you aren't familiar with Discord, it's a voice and text chat platform designed for creating communities. My friends and I use it for messaging each other and hanging out while playing games. One of the core features of Discord is its community-created bots. There's a bot for practically anything you can think of: bots that play music in a voice chat, bots for getting info from sites like Wikipedia, bots that'll translate messages into another language, even bots that'll play text-based games!

The Discord bot API has been packaged up for use in a slew of langauges, including Java, Javascript, and my personal favorite, Python. For a while now, I've been working on a *[kitchen-sink Discord bot](https://github.com/connor-ford/random-discord-bot)*, chock full of random features. Currently, it can:

- Generate an information page on a specific breed of cat,
- Get an image of a specific breed of cat or dog,
- Get a joke, with options to specify a category and the type of joke,
- Respond to keywords with responses, with commands to add, list, and delete keyword/response pairs,
- Generate an information page on a specified Minecraft user, or get their Minecraft skin, with an option to supply either their username or UUID for identification,
- Create and return a hue-shifted worm-on-a-string, because why not, and
- Flip a specified amount of coins, roll a specified amount of specified-sided dice, or just generate a bunch of random numbers.

Like I said, kitchen-sink. Most of these features were added as a proof of concept, to demonstrate functionality, and today's feature is no exception. When my friends and I hang out in a Discord server, we mess with each other every now and then: moving someone to another voice channel, server deafening them so nobody else can hear them, or my personal favorite: server muting them so they can't talk. It's always fun to see how long it takes someone to notice before they're muted. The problem is, *I* get messed with too, which is completely unfair. Only *I'm* allowed to mess with other people, after all. So I set out to give my bot a little extra functionality to make sure that I stay unmuted, undeafened, and in the correct channel.

## Staying Unmuted

I had already built a similar system for handling keywords, so I didn't have to start from scratch. I created two commands to start out with: `/lock mute`, to add a mute lock to a specified user (whether it be locked as muted or unmuted), and `/lock remove`, to remove all locks from a user. I had a bit of trouble with the data structure, as I needed to track each user *in* each guild, as opposed to each user *or* each guild. In the end, I decided to use a three-dimensional dictionary, with the `server ID`, `user ID`, and `lock type` serving as keys. I also used this structure when saving to and loading from server data files, which took a bit to implement, but once I got the ball rolling it was easy to tack on more features.

For the muting part itself, I had to create a new listener for a change in voice state. When this listener is triggered, if any changes in the voice state are detected that conflict with a lock (someone trying to mute me, for example), the bot edits the locked user's voice status to make sure that the lock stays in place. This feature took a long time to get right, but once it did, it worked like a charm.

## Staying Undeafened

Implementing this easy, now that I had the foundation in place; all I needed to do was create a new command, `/lock deafen`, that would lock a user as deafened or undeafened, and add the deafened flag to the listener. **Bob's your uncle**.

## Staying in the Correct Channel

This one was a little more complex. The other two stored boolean values to determine what state they should be locked in, but I needed a way to pass and store a voice channel from the `/lock channel` command. Fortunately, every channel on Discord has an associated `ID`, so I decided to use that. However, this resulted in a bit of hair pulling: I had to convert the `ID` (an integer) into string format in order to pass it to the function that changes a user's channel, which is easy enough to implement, but I forgot to convert it in a couple of places, resulting in the listener logic getting screwed up, and determining that the channel needs to be changed *every single time it gets updated*. After getting yelled at by the Discord API for exceeding the rate limit by a couple hundred requests, I found the datatype error and fixed it, and all was right with the world.

## Showtime

Once I was happy with my result, I decided to test it in action. I locked myself in the main channel as unmuted and undeafened. It didn't take long for my friends to notice my "immunity" whenever I got muted or moved. After a few minutes of this, I had an epiphany: if I could lock myself as unmuted, I could also lock my friends as muted. This resulted in pure chaos, as you could imagine.

## Flaws

Unfortunately, my implementation of this came with a few flaws.

While I can prevent myself from getting muted, deafened, and moved to another channel, I can't prevent myself from getting disconnected from the call. I could potentially create a client interface that automatically connects me when I get disconnected, but that's *kind of* against Discord's Terms of Service. **Darn**.

Another flaw resides in the fact that every time my bot changes a user's status, it shows up in the Audit Log. This isn't good for when someone is trying to be sneaky, but again, not much I can do about that without breaking Discord's ToS. **Double darn**.

My friends found a loophole; instead of switching me to another channel, they switched themselves, meaning that I would have to unlock myself in order to go to the new channel. This got me thinking about user-specific user locking, where if somebody else tried to move me to another channel it'd be reverted, but if I moved myself the bot would allow it. This would most likely have to rely on the Audit Log, which is a layer of complexity that I think I should save for another time.

I felt like some work could be done on removing locks. I added some functionality to the `/lock remove` command, which gave the option of removing the mute lock, deafen lock, channel lock, or all locks on a user. I also added a new command, `/lock list`, that gives a list of all locked users in the server, as well as which locks they have.

There's bound to be some bugs with this implementation, such as problems with listing a user's locks if they have since left the server. While addressing issues such as this are important, I think I will leave it for another time.

Finally, there is the issue of security; currently, any user can lock and unlock any other user. I plan to address this down the road, when I implement permission scopes across all of the bot's commands. For now, I suppose there'll just have to be some chaos.

## Conclusion

Well, this was a fun adventure! I've been meaning to tinker with the voice side of bot creation, and while this isn't that big of a first step into the voice side, it's still a first step. I also got to mess with my friends! For my next Discord bot feature, I think I'll implement something more ambitious, like a TTS command that joins the channel you're in and says what you entered back to you. That sounds interesting to me.

Until next time,  
\- CF
