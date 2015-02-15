---
layout: post
title: "Nordic Game Jam 2015"
date: 2015-02-15 14:16
published: true
category: development
tags: development, games, gaming
---

[![Workflow example]({{ 'superlaserrobots.png' | asset_path }})](http://robo.cat/superlaserrobots/)

> Life is more fun if you play games
>
> -- Roald Dahl

Games are fun to play but also fun to make. This past weekend I attended my second game jam, [Nordic Game Jam 2015](http://nordicgamejam.org/). If you're not familiar with Game Jams it is an event in which you invent, design and build a game in a short time (usually 48 hours). I last attended in 2013 where we made a pretty awesome local two-player arena style game with doctor gone mad called [Buddy Builder](http://robo.cat/buddybuilder). This year we created local four-player fighting game with robots shooting lasers called [Super Laser Robots](http://robo.cat/superlaserrobots).

## The Tech

For this years game we decided to go with a familiar language that we really like. What better choice than Swift. This choice also gave us a ready make excuse to try out "new" and interesting SpriteKit.

Swift has over the the last ~8 months since its introduction become more and more stable. It is already to the point where it is the primary language in all our new ventures at [Robocat](http://robo.cat). Even though the language is pretty stable and has cool shiny new features its toolchain still suffer. We decided before starting JAM to count crashes and issues with Xcode and Swift. 

The results was as follows:

- SourceKit crashes: 58
- Xcode crashes/beachballs: 7
- Swift compiler seg. faults: 2

Had the JAM taken place just a few months earlier prior to Xcode 6.1, the numbers would have significantly worse. The Swift compiler still has some pretty rough edges. One of the segfaults stems we got arises when trying to compare a value agains an enum case:

{% gist ksmandersen/4af33879ed16c04d7d98 %}

This is some pretty basic stuff that pretty easily chokes up the compiler. Rumors are that the Swift 1.2 beta solves most of these obnoxious errors though there are many left.

For all the bad things said about Swift here I could say at least twice as many lovely things. I'll focus on one positive. One of our team members had never written any Swift code, and is primarily concerned with backend development in languages like Python and Rust. He was able to pick up swift and start building code in only a few short hours and was very profficient before the weekend ended. Say what you will, but I think the learning could would have been significantly steeper if it had been Objective-C.

## The Game

The actual resulting game was not overall great to play but we had so much fun building. [The music and sounds](https://soundcloud.com/soundbrandt/sets/super-laser-robots-nordic-game) were beyound awesome, the graphics were great and our idea was interesting. In the end we just didn't have enough time to get the game mechanics to work just right. The game came down to who smashes the key fastest to kill the other opponents.

We had fun building our game and we learned a lot about building games with SpriteKit. You can [find the game here](http://robo.cat/superlaserrobots) or find the [code on GitHub](http://github.com/robocat/superlaserrobots).
