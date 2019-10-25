---
layout: post
title:  "Prep and Perform a Major Database Upgrade"
date:   2019-06-01 06:39:32 -0400

categories: DevOps MongoDB Databases
---

Not unlike many other organizations, the database of choice at Insider Inc. is MongoDB. While I am familiar with NoSQL databases from previous jobs, I still considered myself a novice. 

## Perform as many manual operations as you can beforehand

There were two setbacks during the first upgrade

- The
- Mongov4 and above requires the

## Practice Practice Practice

This may seem self-explanatory, but it's interesting how many engineers will skip this step. To prep, I set up a MongoDB cluster in docker containers on my laptop that was nearly identical to our production environment (one primary and two replicas) and performed practice runs.

## Ensure Compatibility

[atlantis]: https://www.runatlantis.io/