---
layout: post
title:  "MongoDB Upgrade Notes"
date:   2019-11-05 06:39:32 -0400

categories: DevOps MongoDB Databases
---

Recently, we upgraded our MongoDB replica set two major versions (from version 3.4 to 4.0). This was my first time performing such an upgrade, so I was understandably nervous. The database architecture set up consisted of three total clusters. One cluster consisted of a primary instance, and two replicas, while the with two other clusters consisted of a primary, a replica, and an arbiter (for businessinsider.com, insider.com, and businessinsider.de respectively).

While there were some speedbumps, fortunately, the upgrades were very successful!

![Image of slack after successful upgrade](/assets/woot.png)

Here's what I learned during the process:

## Practice Makes Perfect

This may seem self-explanatory, but it's interesting how many engineers will skip this step. To prep, I set up a MongoDB cluster in docker containers on my laptop that was nearly identical to our production environment and performed practice upgrades.
It's imperative to practice on an environment as close to production as possible

## Automate

While you may not be able to automate everything, do as much as you can
Personally, I automated the database backups and package installations

## Understand and prep the manual portions

INSERT IMPORTANT COMMANDS USED
