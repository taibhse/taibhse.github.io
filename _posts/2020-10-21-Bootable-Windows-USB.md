---
layout: post
title: Creating bootable Windows usb from iso in macos
---

## Introduction

Every now and then I need to look this up myself. Collecting and maintaining notes here.
Let's start with Windows, as it is tricker than Linux:

## Credits
I don't know this full process off the top of my head. I would like to credit sites that helped me

[unixtutorial](https://www.unixtutorial.org/creating-bootable-usb-from-iso-in-macos/)
Decent guide, but there is an issue with Step 5, `sources/install.wim` is too big for the FAT32 file system.

[freecodecamp](https://www.freecodecamp.org/news/how-make-a-windows-10-usb-using-your-mac-build-a-bootable-iso-from-your-macs-terminal/)
This has a process for using wimlib via Homebrew which worked well for me.


## Plug in the USB and find out what Disk number it is assigned with diskutil

You want a USB at least 8GB in size for this.

- Open your command prompt and run `diskutil list`
- Plug in your USB Device
- run `diskutil list` again

While you can expect it to be the highest number list, don't take that for granted. 
Double check it has the name and is about the right size.
You do not want to make a mistake and clear the wrong device.


## Format the USB 
if you found disk9 from the previous command, we use it here:
`diskutil eraseDisk MS-DOS "WIN10" MBR disk9`

That should format the USB with FAT and mount it at /Volumes/WIN10

## Mount the Windows ISO
This will make it possible to move the contents onto the USB.

go to the directoy and open it:

`open ./Win10_1607_English_x64.iso`
You can close the finder windows, this command will mount it somewhere like `/Volumes/CCSA_X64FRE_EN-US_DV5` 
open that location for the nest step

## Copy the files onto the USB Stick
Use rsync with the option to skip the problem file. (otherwise you will have to delete it later on a 8GB USB)
`rsync -vha --exclude=sources/install.wim /Volumes/CCCOMA_X64FRE_EN-US_DV9/ /Volumes/WIN10`

If you really don;t want to use rsync, `sudo cp -r . /Volumes/WIN10/`
Just remember to run this if you see an error from install.wim being too big `rm /Volumes/WIN10/sources/install.wim`

## Dealing with sources/install.wim

This file is too big, we need to use winlib to split it.
If you don't have Homebrew install it:  (It is a very useful thing to have in OSX anyway)
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Here are the commands:
```
brew install wimlib
mkdir /Volumes/WIN10/sources
wimlib-imagex split /Volumes/CCCOMA_X64FRE_EN-US_DV9/sources/install.wim /Volumes/WIN10/sources/install.swm 4000
```

## Finished

That should be it, eject the drive safely, put it into the target PC and restart it.
It will either pick it up automatically if configured to do so, or you will need to go into the BIOS and boot directly into it from there.



