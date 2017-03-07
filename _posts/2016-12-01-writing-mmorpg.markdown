---
layout: post
title:  "Writing a MMORPG video game part 1: Motivation"
date:   2016-12-01 12:30:00
categories: mmorpg
---

This is the first part of a series of posts about writing a MMORPG game.

* [Motivation]({{site.baseurl}}/mmorpg/2016/12/01/writing-mmorpg.html) - you are here.

## Motivation

Writing a video game is hard. Before I started I had to ask myself why should I write a video game instead of just playing one.
And a couple of answers came into my mind:

* Learning new development stacks.
* Improve my development and management skills.
* This is a good challenge.
* And most important, just for the lolz.

But there was something missing, and for that matter, something quite important. What's the motivation behind writing a game?

And for me was uniqueness of items, consequences after every dead or decision and commerce
between players using blockchain technologies like ethereum as a backend for this.

### Uniqueness

In every major video game the mechanics are very simple: kill a monster, collect the drop, use it or sell it. But then you realise that you've been playing
this game for a long time and your stats and gear are no different than someone elses because the game has a fix set of items than can be used at a given level, so,
there is no distinction at all.

What this game proposes is a different way to build your gear, every drop is no longer a weapon or gear (unless is specified by a quest), instead, you get items to make
your weapons or magical armors and for that you get to name that piece. 1) because is yours, 2) because when you create an item it depends on the your level.
So, in theory is a unique item that you can only make. And of course you can sell this items in the market or to individuals and gain some ethereum in the process :)

### Consequences

When you die there a couple of actions that can happen according to the difficulty of the level:

* A random item you are wearing gets dropped
* Half of your money gets dropped
* And according to specific zones, if you die, you die.

So please don't die, instead try to play with others as parties and make sure to balance the team.

### Commerce

Because every one has its own NPC's, there are items that you can't make because you don't have the level or your NPC does not have the skill nor the items
to make them, but if someone is willing to sell you can definetly buy them. Every player has access to some slots in an online market where you can sell/buy items in gold
(game currency) or in ethereum and I hope this motivate people to create a healthy market where your time can be compensate IRL.


Let's dive into it.

## 2D over 3D

This decision affects heavily the looks of the game, take for instance Super Mario 64 and its 3D graphics:

![Super Mario 64](/images/mario.jpg)

and compare it with a 2D game like Pokemon for game boy.

![Pokemon](/images/pokemon.jpg)

The difference is clear, but then why 2D?

I'm not lazy, well yes, but creating a 2D game is difficult as it is when you are solo developer, that's why I chose 2D for this game.
It's simple and we can acomplish excelent game play using this approach, there are a lot of games doing (or were doing) this like Tibia online.

![Tibia](/images/tibia.jpg)


## Fat client and Thin server

The client should do the heavy lifting of the game while the server should only save the status of the game and provide additional information as well as security.

### Client stack

The client should focus on:

* Rendering the graphics
* AI
* I/O
* Multiplatform

### Server stack

The key word here is concurrency, your server should be able to handle massive ammount of concurrent requests without sweating
and this is because in theory your server is only doing CRUD operations most of the times. Other operations should be:

* Experience multiplier given at log in.
* Drop probability given after each kill.
* Hash validation (on every request) to avoid hickjacking of the service.
