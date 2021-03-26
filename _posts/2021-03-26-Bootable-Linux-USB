---
layout: post
title: Create bootable Linux usb from iso in macos
---

## Introduction

Every now and then I need to look this up myself. Collecting and maintaining notes here.
This version is for Linux

## Credits
[freecodecamp](https://www.freecodecamp.org/news/how-make-a-windows-10-usb-using-your-mac-build-a-bootable-iso-from-your-macs-terminal/)



## Plug in the USB and find out what Disk number it is assigned with diskutil

You want a USB at least 8GB in size for this.

- Open your command prompt and run `diskutil list`
- Plug in your USB Device
- run `diskutil list` again

```
diskutil list
#Plug in USB
diskutil list
```

While you can expect it to be the highest number, don't take that for granted. 
Double check it has the name and is about the right size.
You do not want to make a mistake and clear the wrong device.

Once you are sure, make a not of the name and unmount it
```
diskutil unmountDisk /dev/disk4
```

## Convert ISO into bootable DMG

```
hdiutil convert -format UDRW -o fedora34b ./Fedora-Workstation-Live-x86_64-34_Beta-1.3.iso
```

##  Write DMG image onto USB stick with dd

This might take a while, you may want to increase the value for bs or just let it run for a while.

```
sudo dd if=./fedora34b.dmg of=/dev/disk4 bs=1m
```

