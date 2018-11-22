---
layout: post
title:  "Comparing Different Linux Package Types"
date:   2018-11-22 06:39:32 -0400

categories: Linux
---

Whether you use Linux on the desktop or as your Operating System of choice for your servers, we've all come across the concept of packages. For the most part, the methodology for installing them is the same regardless of distribution. On Debian based distros, you would use something like apt-get, whereas Centos and Fedora use yum

However, if you try to install a Debian package (a `.deb` file) on a computer running Fedora, you're going to have a bad time. Even both are flavors of Linux, you can't install the same packages. Even if you try to install the same package on each via the package manager, you'll find that they can even have different names[1]

In an effort to standardize the package management space, there are different package management systems that allow the different flavors of linux to install the same packages[2]. In this post, we'll be discussing a few of them - [Snap][snap], [AppImage][appimage], and [Nix][nix].

## Snap
Snap is a packaging format build by Canonical (the organization behind Ubuntu) that bundles the software, including all of its dependencies, into a single package. Unlike `deb` and `rpm` formats, it installs the software in a separate directory, away from other system directories. The project brands itself as the "Universal App Store for linux"

The method for installing a snap package would look something like this:

`$ snap install spotify`

One of the main reasons to use Snap is for security purposes. The installed snap packages work in an isolated system on Linux which limits the harm that it can do post-installation. Another reason to use Snap is that it is complemented by the use of [Snapcraft][snapcraft], which is a tool to easily allow developers to build software for personal computers, IoT, servers, and even mobile.

The main downside to Snap is storage. Due to the fact that Snap packages bundle the application with all of the dependencies, they can get quite large. For example, a LibreOffice snap is 280MB, while the .deb package is only 75MB[3]


You can install snap from your distribution's [package manager][snap-install]


## AppImage
AppImage is similar to Snap, in that it bundles the application in with all of its dependencies. However, the difference is that you don't need to actually 'install' AppImage on your computer, as you would with Snap. From a technical standpoint, an AppImage is basically a self-mounting disk image that contains an application and everything it needs to run on the targeted system.

The method for installing a `.AppImage` file would be like this
```
$ chmod +x <file-name>.AppImage
$ ./<file-name>.AppImage
```
And then you're done!

All you do is make the `.AppImage` file executable and run it.

Generally, upon execution, a popup will appear asking if you'd like to create a `.Desktop` file entry for the new AppImage application, which will make it searchable, allow it to appear in you docks / widgets, etc. An AppImage file an fundamentally run on any Linux distribution and it does not require any additional steps

It also has the distinction of being endorsed by [Linus Torvalds][appimage-endorse]

***Note: When you execute the AppImage file and it creates the .Desktop entry, etc, the resting place of the executable is wherever you left it.***

***Personally, I've gotten into the habit of moving my `.AppImage` into a hidden directory in home folder in order to consolidate them in one place - you don't want to get into the habit of leaving them in your Downloads folder***


## Nix Package Management System
Last, but not least, we have Nix. Similar to Snap, Nix requires you to install the Nix package manager onto your system. Where Nix differs from the rest is that it works, not only for Linux, but for other Unix-like OSes, as well (such as macOS).

Unlike Snap and AppImage, Nix works more like a traditional package management system. As such, the user / distributer compiles the source code into a package, but the difference is that it is capable of keeping different library versions apart and will automatically share libraries if they are the same.

## Conclusion
To conclude, here are the three different universal package systems that I have used. Personally, I find AppImage to be the easiest (simply due to how easy it is to use), followed by Nix.

Other similar projects are [Flatpak][flatpak] by Redhat and [Limba][limba].

As for which of these ends up being the "winner", I'm actually going to say Flatpak (even though I didn't talk about it) and AppImage. The reasoning for Flatpak is that the tech from Redhat, like systemd and pulseaudio, generally "wins". The reasoning for AppImage is because how simple it is to use (and getting an endorsement from Linux Torvalds doesn't hurt either)



[1] The most notable of this would be `apache2` vs `httpd`

[2] Yes, I am aware of the irony that there are multiple standards for something that's supposed to unify  the current state of Linux package management

[3] The apt-get package manager will say that LibreOffice is 200MB, but this is because it is including all of the dependencies that will be installed by the package manager (and will be shared by other applications on the system), the actual deb file provided by Debian is only 75MB


[snapcraft]: https://developer.ubuntu.com/snapcraft
[snap-install]: https://docs.snapcraft.io/installing-snapd/6735
[snap]: https://snapcraft.io/
[appimage]: https://appimage.org/
[appimage-endorse]: https://plus.google.com/+LinusTorvalds/posts/WyrATKUnmrS
[nix]: https://nixos.org/nix/
[flatpak]: https://www.flatpak.org/
[limba]: https://github.com/ximion/limba