---
layout: post
title:  "A Linux Users Guide For Configuring Their Macbook"
date:   2019-04-09 06:39:32 -0400

categories: Linux macOS
---

I'm starting a new job as a DevOps Engineer and with it comes a shiny, new Macbook! For those of you that don't know me, I'm a hardcore Linux user that's been using various Linux distros on my personal laptop for several years now.    
By far and away, the most annoying part about using macOS is that it's *UNIX-like*. Meaning that, just when you think that everything is, more or less the same, you go ahead and make some mistake like [this][max-gsort-post]

***Note: Not all of these are the fault of macOS. The above example is actually because macOS is based on BSD, not Linux***

With this guide, we will be looking at ways to make our shiny macbooks more like my Ubuntu installation on my personal computer. 

## Package Management
First, we need to install a package manager. Unlike nearly all Linux distros, macOS does not come with a package manager by default. 

The most popular one is [homebrew][brew] which we can install with the following command: 
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
This will be useful for installing tools in later sections. 

I also recommend install [Nix][nix] and [MacPorts][macports]. While macOS does not have something as unified as good as the `apt` package manager, the combination of these three make up for it. 

## CLI Tools
Earlier, I brought up the difference between macOS `sort` and the GNU/Linux `sort`. This next step will involve installing the GNU Coreutils with a shell script called [Linuxify][linuxify].

This bash script will use homebrew to [install and replace][cli-linuxify] the macOS/BSD command line tools with the GNU/Linux variants.

## Window / Application Management
Now that we have the command line out of the way, let's focus on the desktop itself. One of the biggest annoyances of moving from Linux (as well as Windows) to macOS is switching between applications and windows. Pressing the `alt+tab` keys will switch between the open applications instead of the open windows. For this, I recommend [Hyperswitch][hyperswitch] (which is what I personally use) or [Witch][witch]. 

***Note: See [this][stack-overflow] Stack Overflow post for additional macOS window managment tools***

Next, let's mimic the way Linux allows us to move windows with powerful keyboard shortcuts (specifically, I'm referring to the `Super` + direction). The only free one that I've come across is an application called [Spectacle][spectacle]. 

If you're more a fan of linux window managers like xmonad or i3, I recommend [Amethyst][amethyst]

Finally, we want a powerful tool to search across our machine. For this purpose, I use [Alfred][alfred].
You can directly download Alfred via the above link, or you can install it via homebrew
```
brew cask install alfred
```

## Conclusion
To conclude, these are the steps that I personally take to make my Macbooks feel more like a Linux distro. For me, macbooks are UNIX-like enough for most usecases and for getting work done.   
God forbid I'm given a laptop with . . . Windows *shudder* :p


[max-gsort-post]: https://maxchadwick.xyz/blog/sort-h-on-a-mac
[linuxify]: https://github.com/fabiomaia/linuxify
[cli-linuxify]: https://github.com/fabiomaia/linuxify/blob/master/linuxify#L22-L65
[brew]: https://brew.sh/
[hyperswitch]: https://bahoom.com/hyperswitch
[witch]: https://manytricks.com/witch/
[stack-overflow]: https://apple.stackexchange.com/questions/293790/make-macos-look-like-a-linux-desktop
[spectacle]: https://www.spectacleapp.com/
[amethyst]: https://github.com/ianyh/Amethyst
[alfred]: https://www.alfredapp.com/
[nix]: https://nixos.org/nix/
[macports]: https://www.macports.org/
