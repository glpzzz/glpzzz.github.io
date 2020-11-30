---
title: "How to identify my public IP from Linux CLI"
date: 2020-11-30
description: "Which is my public IP? Linux command to know it on CLI, as I like the things"
image: /assets/2020-11-30-my-public-ip-linux-cli.png
---
So easy!
```
dig +short myip.opendns.com @resolver1.opendns.com
```
