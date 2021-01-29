---
title: Personal directory distributed over several physical disks
date: 2020-03-12
description: "How to split your home directory in several disks instead of having a bunch o units visibly mounted on your file manager."
image: https://glpzzz.is-a.dev/assets/linux-home-several-disks.png
---
Someone may say that it is a symptom of OCD, but the fact is that I do not like having disk drives visible in the file browser. Point! I also do not like having a separate disk to store other things and that remain outside my home directory.

What I like is being able to use the directories that are pre-configured for each user: Documents, Downloads, Images, Public, Music, Videos.

When you have a single disk on your computer, it is very simple:
* Disc 1:
    * Partition for the system: `/`
    * Partition for user: `/home`

But when you have multiple albums that's when things start to get a little disorganized. It is possible to use a whole disk for the system (the small one) and the rest for the home (the big one). But if the system disk is big, having all that space dedicated to / would be a waste, then we also put home there:

* Disc 1:
    * Partition for the system: `/`
    * Partition for user: `/home`
* Disc 2:
    * (n partitions that are mounted as independent drives)

But in this case, the partition (s) of Disk 2 appear in the file browser, as something apart. I do not like it, it seems out of place, I will also store my files there, they should be below `/home/<user>`. 

In that Disc 2, the extra, maybe we put what is "not essential": Images, Music and Videos. With this situation I started to think about alternatives and I came to:

* Disc 1:
    * Partition for the system: `/`
    * Partition for user: `/home`
* Disc 2:
    * Partition mounted to: `/home/<user>/Media`

![Settings in /etc/fstab to mount in /home//Media](https://gutl.jovenclub.cu/wp-content/uploads/2020/03/2020-03-12_00-06.png)

In Media then I put the directories Images, Music and Videos. But it seemed unnatural to me to have to go to another directory to get to music, videos, etc. Something better should be able to do.

![Settings for personal directories on LxQt](https://gutl.jovenclub.cu/wp-content/uploads/2020/03/con-afuera.png)

After thinking a bit, I came up with the schema I'm currently using:

* Disc 1:
    * Partition for the system: `/`
    * Partition for user: `/home`
* Disc 2:
    * Partition mounted to: `/home/<user>/.media`

![New settings por personal directories](https://gutl.jovenclub.cu/wp-content/uploads/2020/03/con-oculto.png)

In this way, and using symlinks, the traditional file structure of a home remains, distributed between 2 physical disks.

![Result](https://gutl.jovenclub.cu/wp-content/uploads/2020/03/final.png)
