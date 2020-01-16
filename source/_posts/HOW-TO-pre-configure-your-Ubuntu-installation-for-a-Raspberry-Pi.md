---
title: HOW TO pre-configure your Ubuntu installation for a Raspberry Pi
date: 2020-01-14 20:23:54
updated: 2020-01-16 18:58:09
tags:
- Rapsberry Pi
- Ubuntu
- Cloud-init
---
{% img /blog/images/raspberrypi.jpg '"Public Domain Image / CC0"' %}
Whether you need to install your Raspberry Pi headless,
Or whether you plan to mass-produce SD card for a big IoT project,
In both case, Ubuntu offer a very standard way to pre-configure your image, based on Cloud-init.
<!-- more -->

## Preamble
Flash your SD card with an Ubuntu image: [Install Ubuntu Server on a Raspberry Pi 2, 3 or 4](https://ubuntu.com/download/raspberry-pi). You can use [balenaEtcher](https://www.balena.io/etcher/) to install the image on your media in the blink of an eye!

## Cloud-init configuration
(Re)mount your SD card volume, then you have acces to the two files we will update: `user-data` and `network-config`. Cloud-init is a de facto standard for Cloud VM configuration. You can do pretty everything with it.

### System configuration
`user-data` file contains some examples to help you with your configuration. For more details, you can refer to the Cloud-init [documentation](https://cloudinit.readthedocs.io/en/latest/index.html); or you can jump directly to [examples](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).

You can find below my own configuration file, which:
- set a hostname
- set a (default) user `pi` with an authorized public ssh key and sudo rights

{% codeblock lang:YAML %}
#cloud-config

## Set hostname
hostname: raspberry

## Configure default user
system_info:
  default_user:
    name: pi
    ssh_authorized_keys:
      - ssh-rsa ... user@comment
    sudo: ALL=(ALL) NOPASSWD:ALL

## Reboot to enable Wifi configuration (more details in network-config file)
power_state:
 mode: reboot
{% endcodeblock %}

### Network configuration
`network-config` relies on Netplan format ([reference](https://netplan.io/reference)).
The default configuration of the Ubuntu pre-installed image enables network on the ethernet port, using DHCP:
{% codeblock lang:YAML %}
version: 2
ethernets:
  eth0:
    dhcp4: true
    optional: true
{% endcodeblock %}

For example, if you want to enable both ethernet and wifi, update the file accordingly:
{% codeblock lang:YAML %}
version: 2
ethernets:
  eth0:
    dhcp4: true
    optional: true

# Wifi interface remains down during first boot. You can't rely on
# sole Wifi for network during cloud-init (forget about packages
# upgrade and so on...).
# You might like to reboot your system at the end of cloud-init.
# This can be achieved by adding those lines to your user-data file:
# power_state:
#   mode: reboot
wifis:
  wlan0:
    dhcp4: true
    optional: true
    access-points:
      "ChangeWithYourSSID":
        # mode defaults to "infrastructure" (client)
        password: "ChangeWithYourPassword"
{% endcodeblock %}
