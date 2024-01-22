---
title: "How to activate WWAN for the Lenovo ThinkPad P50 on Ubuntu"
date: 2024-01-22
description: "Steps I follow to activate the mobile broadband connection on my ThinkPad P50 after a fresh Ubuntu install"
---

This is a personal reminder on the commands required to activate this feature because as explained on [this post](https://askubuntu.com/a/1436705/1023633) it's deactivated by default. 

## Activate the modem

The command to activate the Lenovo shipped EM7455 is:

```bash
sudo ln -sft /etc/ModemManager/fcc-unlock.d /usr/share/ModemManager/fcc-unlock.available.d/1199:9079
```

## Setup the mobile broadband connection

The following step is to create the Mobile Broadband connection using Network Manager GUI or nmcli.

In order to do it with nmcli, first we need to identify the available modems:

```
$ mmcli -L
    /org/freedesktop/ModemManager1/Modem/0 [Sierra Wireless, Incorporated] Sierra Wireless EM7455 Qualcomm Snapdragon X7 LTE-A
```

So, in my case, there just one modem, the 0 indexed one. Now let's identify the device:

```
$ mmcli -m 0 | grep "primary port"
           |         primary port: cdc-wdm1
```

And now we are ready to create the connection. For my actual service provider the required command is:

```
$ nmcli connection add type gsm ifname cdc-wdm1 con-name Cubacel apn nauta
Connection 'Cubacel2' (906f5b4f-b37b-4eff-b8eb-5c13dc9c532b) successfully added.
```

Being the important parts:
- type: the connection type, gsm for mobile broadband
- ifname: the id the of device
- apn: the value defined by the provider

The name is just a friendly identifier.

To activate this connection the command is:

```
$ nmcli connection up Cubacel
    Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/21)
```

And to deactivate the command is:

```
$ nmcli connection down Cubacel
    Connection 'Cubacel' successfully deactivated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/21)
```

If you want to remove the connection:

```
$ nmcli connection delete Cubacel
    Connection 'Cubacel' (7478c2fb-6d85-443d-ab67-2b812b761460) successfully deleted.
```

Consider also the usage of `modem-manager-gui` to work with the modem, but my notes on that are for another post.
