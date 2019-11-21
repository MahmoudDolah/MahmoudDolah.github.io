---
layout: post
title:  "MongoDB Upgrade Notes"
date:   2019-11-05 06:39:32 -0400

categories: DevOps MongoDB Databases
---

Recently, we upgraded our MongoDB replica set two major versions (from version 3.4 to 4.0). This was my first time performing such an upgrade, so I was understandably nervous. The database architecture set up consisted of three total clusters. One cluster consisted of a primary instance, and two replicas, while the with two other clusters consisted of a primary, a replica, and an arbiter (for businessinsider.com, insider.com, and businessinsider.de respectively).

While there were some speedbumps, fortunately, the upgrades were successful!

![Image of slack after successful upgrade](/assets/woot.png)

Here's what I learned during the process:

## Practice Makes Perfect

This may seem self-explanatory, but it's interesting how many engineers will skip this step. To prep, I set up a MongoDB cluster in docker containers on my laptop that was nearly identical to our production environment and performed practice upgrades.
It's imperative to practice on an environment as close to production as possible. To practice, I configured a [MongoDB replica][mongo-docker] set using Docker

## Automate

While you may not be able to automate everything, do as much as you can
Personally, I automated the database backups and package installations
I also recommended to creating a [runbook][runbook-doc]

## Remember Useful Commands

As I said, this is my first time working this closely with MongoDB, so here are a list commands that I found useful

Install specific MongoDB version via apt (ensure the list files are [added][mongo-install])
```
sudo apt-get install -y mongodb-org=3.6.14 mongodb-org-server=3.6.14
mongodb-org-shell=3.6.14 mongodb-org-mongos=3.6.14 mongodb-org-tools=3.6.14

sudo apt-get install -y mongodb-org=4.0.12 mongodb-org-server=4.0.12
mongodb-org-shell=4.0.12 mongodb-org-mongos=4.0.12 mongodb-org-tools=4.0.12
```

Shutdown MongoDB Instance
```
 > use admin
 > db.shutdownServer()
```

Show replica set [status information][mongo-replica-status-info]
```
 > rs.status()
```

[mongo-docker]: https://www.sohamkamani.com/blog/2016/06/30/docker-mongo-replica-set/
[runbook-doc]: https://www.process.st/create-a-runbook/
[mongo-install]: https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-ubuntu/#using-deb-packages-recommended
[mongo-replica-status-info]: https://docs.mongodb.com/manual/reference/method/rs.status/#rs.status

