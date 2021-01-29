---
title: "About tiling windows managers"
date: 2019-11-28
description: "This is a quick introduction to three of the more popular tiling windows managers: i3, bspwm and my favorite dwm. Just to know my vision about each one of these. I hope this helps you..."
image: https://glpzzz.is-a.dev/assets/about-tiling-windows-managers.png
---
I have been using tiling windows managers for a couple of years now and I love it!

## what?

Tiling windows managers (just) focus on the layout of windows on the screen making sure of two things:

- every window will use all the available space on screen
- if several windows open, all of them will be shown according to certain rules.

## why?

Because is simple, is less, is better (for me). Most of the implementations dont even decorate the windows. So, closing it depends on app specifics or a keyboard binding. 

## how?

There are many options. I have tried extensively:

- i3: the nicer one. Good defaults and also provides an status bar and desktop changer. Configuration is done by editing a file. It also have a locker and status monitor.

- bspwm: the lightest. No bar at all. Is up to you. Configuration is just a script that calls a program that handles the window manager. This event don't handle keyboard bindings. Normally you will use sxhkd for this.

- dwm: my current choice. It's a suckless.org project. So, you will be compiling C code to configure stuff. But it's not hard. It provides a good bar and the chance to have current active window title on top. In reminds me Unity experience. 

## important

- the layouts: most tiling provides different ways to organize the windows. The tiles, columns, rows, master and slave, master on the center, monocle, floating (sometimes is good)

- don't need to start from scratch: you can just setup the window manager for your current desktop environment. I'm currently using lxqt desktop environment with dwm as windows managers instead of openbox, This way I can use a regular graphical session with all the components wired in and also enjoy my favorite window manager in less than 5 minutes. Especially with lxqt that allows to change the window manager in the configuration as a very normal thing. 

You can also replace kwin in a KDE session of xfwm on XFCE and rely on  that DE setup instead of configuring everything from scratch. The methodnis different in each one, but is in general about this 2 steps:

- avoid the start or kill the default window manager
- start the tiling one.

## conclusion

I have learned a lot while playing with windows managers because they give you the chance to create your desktop environment from scratch and decide which pieces do you really need.

I'm open to questions!
