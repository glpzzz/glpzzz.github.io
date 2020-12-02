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

And of course I created an alias for it:

```
alias myip="dig +short myip.opendns.com @resolver1.opendns.com"
```

So, now I just need to type

```
myip
```

## Update

The coleagues @codeshard and @luilver on Twitter suggested this other way:

```
curl ifconfig.io
```

;-) Useful for me, hope it's useful for you!