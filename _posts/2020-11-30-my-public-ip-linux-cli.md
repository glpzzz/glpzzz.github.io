---
title: "How to identify my public IP from Linux CLI"
date: 2020-11-30
description: "Which is my public IP? Linux command to know it on CLI, as I like the things"
image: https://glpzzz.is-a.dev/assets/2020-11-30-my-public-ip-linux-cli.png
---
So easy!

```bash
dig +short myip.opendns.com @resolver1.opendns.com
```

And of course I created an alias for it:

```bash
alias myip="dig +short myip.opendns.com @resolver1.opendns.com"
```

So, now I just need to type

```bash
myip
```

## Update

The coleagues @codeshard and @luilver on Twitter suggested this other way:

```bash
curl ifconfig.io
```

;-) Useful for me, hope it's useful for you!
