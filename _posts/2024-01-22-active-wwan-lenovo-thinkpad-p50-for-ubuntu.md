---
title: "Step-by-Step Guide: Enabling WWAN on ThinkPad P50 Ubuntu"
date: 2024-01-22
description: "Discover the steps to set up mobile broadband on Lenovo ThinkPad P50 using Ubuntu. Our guide simplifies the WWAN activation process, ensuring a hassle-free experience for users."
image: https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/Lenovo_thinkpad_p50_%2829237281256%29.jpg/640px-Lenovo_thinkpad_p50_%2829237281256%29.jpg
---

If you've encountered issues with the default deactivation of WWAN on your Lenovo ThinkPad P50 after a fresh Ubuntu install, this guide will walk you through the steps to activate it. This is a personal reminder on the commands required to activate this feature based on discussions in [this Ask Ubuntu post](https://askubuntu.com/a/1436705/1023633) where it's explained that WWAN is deactivated by default.

## Activate the Modem

To enable the Lenovo shipped EM7455, use the following command:

```bash
sudo ln -sft /etc/ModemManager/fcc-unlock.d /usr/share/ModemManager/fcc-unlock.available.d/1199:9079
```

## Setup the Mobile Broadband Connection

The next step is to create the Mobile Broadband connection using Network Manager GUI or nmcli. In order to do it with nmcli, first we need to identify the available modems:

```bash
$ mmcli -L
    /org/freedesktop/ModemManager1/Modem/0 [Sierra Wireless, Incorporated] Sierra Wireless EM7455 Qualcomm Snapdragon X7 LTE-A
```

In this case, there is only one modem (indexed as 0). Now, identify the device:

```bash
$ mmcli -m 0 | grep "primary port"
           |         primary port: cdc-wdm1
```

### Creating the Connection with nmcli

And now we are ready to create the connection. For my actual service provider the required command is:

```bash
$ nmcli connection add type gsm ifname cdc-wdm1 con-name Cubacel apn nauta
Connection 'Cubacel2' (906f5b4f-b37b-4eff-b8eb-5c13dc9c532b) successfully added.
```

Values of the command:
- type **gsm**: the connection type (gsm for mobile broadband)
- ifname **cdc-wdm1**: the device ID
- apn **nauta**: the APN value defined by the provider

The name is just a friendly identifier.


### Activate/deactivate the connection 
To activate this connection the command is:

```bash
$ nmcli connection up Cubacel
    Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/21)
```

And to deactivate the command is:

```bash
$ nmcli connection down Cubacel
    Connection 'Cubacel' successfully deactivated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/21)
```

### Remove the connection 

If you want to remove the connection:

```bash
$ nmcli connection delete Cubacel
    Connection 'Cubacel' (7478c2fb-6d85-443d-ab67-2b812b761460) successfully deleted.
```

## Conclusion 

- We have addressed the issue of deactivated WWAN on Lenovo ThinkPad P50 running Ubuntu, providing clear steps to activate the modem, set up the mobile broadband connection, and achieve seamless connectivity. Following these steps ensures a successful configuration for utilizing mobile broadband on your ThinkPad P50.
- Consider also the usage of `modem-manager-gui` to work with the modem, but my notes on that are for another post.

___
Using image from https://commons.wikimedia.org/wiki/File:Lenovo_thinkpad_p50_%2829237281256%29.jpg licensed with CC BY-SA 2.0.
