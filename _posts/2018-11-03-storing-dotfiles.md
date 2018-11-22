---
layout: post
title:  "How I Store Dotfiles"
date:   2018-11-03 06:39:32 -0400

categories: DevOps
---

I've finally got down to it - I'm going to start properly managing my dotfiles!
Previously, I had kept them scattered in random Github gists and repositories. I've experimented with tools like [yadm][yadm], [Ansible][ansible] with several Python scripts, and even [GNU Stow][stow]

In the end, I just found all the other methods too cumbersome to use.

***Note: There's a great [Hacker News thread][hn] about this with dozens of alternatives***

## Goals

I had some requirements for how such a system should function. It should be:

#### 1) Straightforward to set up    
I don't want to spend several hours trying to fight my tools just to get this working the first time

#### 2) Simple to Remember    
It should be something that is second nature to me and relies on knowledge that I have already internalized

#### 3) Have Minimal Dependencies    
Assuming I'm setting up my dotfiles on a new machine (or am trying out a new Linux distribution), I don't want to have to install and fiddle around with other tools.
Ideally, it would leverage only what is available via my package manager

## The Decision

After spending some time looking, I found a blog post for a dotfile management system that covers all the bases. Here's the [blog post][atlassian] that covers the system that I have set up.

***Note: Just in case, I've included an archive link [here][archive]***

So, does this cover all the bases?    
1) This is really easy to set up    
2) It uses git and a shell alias, which is easy to remember    
3) The only dependency is git    

You may now bask in the glory of my [dotfiles repository][dotfiles] :p

To conclude, this same set up might not work for you, I know some people that prefer something like [this][crap] and it works for them ¯\\\_(ツ)_/¯


## To Do
There's still a bit more I'd like to do with this, such as, limit the disconnect between the dotfiles on my personal and work computer and find better ways to account for the different Operating Systems and desktop environments that I use


[yadm]: https://thelocehiliosan.github.io/yadm/
[ansible]: http://brandon.invergo.net/news/2012-05-26-using-gnu-stow-to-manage-your-dotfiles.html
[stow]: http://brandon.invergo.net/news/2012-05-26-using-gnu-stow-to-manage-your-dotfiles.html
[hn]: https://news.ycombinator.com/item?id=11070797
[atlassian]: https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/
[crap]:https://news.ycombinator.com/item?id=11072115
[archive]: https://web.archive.org/web/20181012222049/https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/
[dotfiles]: https://github.com/MahmoudDolah/dotfiles/tree/master