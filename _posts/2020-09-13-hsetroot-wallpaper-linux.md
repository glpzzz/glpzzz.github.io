---
title: "Simple way to set wallpaper on Linux"
date: 2020-09-13
description: "The simplest way to set a wallpaper on Linux: use hsetroot."
image: https://glpzzz.is-a.dev/assets/hsetroot-wallpaper-linux.png
---
I don't use any major desktop environment on Linux. Most of these takes care of managing the wallpaper of your session by some functionality in the specific file manager. So, the desktop is treated as  another directory by Nautius, Dolphin, Thunar, and others. 

This is not the way I do things. I'm more into [window managers](https://dev.to/glpzzz/about-tiling-window-managers-4ggb) that regularly don't know anything about wallpaper.

In my case, I use [hsetroot](https://github.com/himdel/hsetroot), a very small utility, that can be installed on my Ubuntu setup with:

```bash
sudo apt install hsetroot
```

And I just use `hsetroot` for two very specific actions

Setup a solid color as background
--------------------------------

```bash
hsetroot -solid "#000000"
```

Will set black as the solid color background. 

Setup an image as wallpaper covering all the screen
---------------------------------------------------

```bash
hsetroot -cover /path/to/image.jpg
```
Will expand and/or crop the image to fully cover the screen by respecting its aspect ratio.

Remember between sessions...
----------------------------

`hsetroot` doesn't store any settings. So, then session is restarted it will not remember the last color/image set. How to fix this? Call `hsetroot` on session start. I personally do it in my `~/.xsessionrc` and with an specific path. 

```bash
hsetroot -cover ~/.wallpaper.jpg
```

When I want to set a new wallpaper I replace this file, in the next session that will be the default. 

Conclusion
----------

There are more comfortable ways to change your wallpaper, specially for those who do it very frequent. But, for me, who changes it once in months (maybe), ``hsetroot`` is perfect:

* it does just one thing
* it doesn't have a GUI
* it's very simple to call in scripts.

And finally, yes, there are other utilities to do the same. `feh` and `xsetroot` for example. But, for some reason I have been using `hsetroot` for a couple of years. It just works!
