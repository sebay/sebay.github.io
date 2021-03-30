---
title: Create Live Persistent Ubuntu 20.04 USB thumbdrive on Mac OS with Mac Linux USB Loader
date: 2021-03-30 13:47:00 +0800
categories: [Blogging]
tags: [blogging, ubuntu, macos]
toc: false
---
I have had this mania recently to want to have my linux running on just any single computer I touch and without putting a mess on that computer.

Ubuntu USB Live just lets you do that. And with persistence attached to the USB thumbdrive you can persist any data you want: files, config, software installed.

<!--more-->
A lot of the UI tools available will fail to make USB stick bootable on Intel Macs. That is because a lot of distribution do not have EFI bootable support.

The easiest solution to create a live persistent USB on Mac OS seems to be by using [Mac Linux USB Loader](https://www.sevenbits.io/mlul/), or the free version that you will have to build from [github](https://github.com/SevenBits/Mac-Linux-USB-Loader).

1. On your Mac start Volume Disk Utility and Create 3 FAT32 partition on your thumbdrive.
* The first partition labelled MACUBUNTU1 can be fairly small, ie 10GB. Do not use space in the label.
* The second partition labelled MACUBUNTU2 and MACUBUNTU3 can use the rest of your thumbdrive (ie 55GB and 55GB if you have a 120GB thumbdrive).
2. Launch Mac Linux USB Loader and click *Persistence Manager* to Create a persistence file on the first partition MACUBUNTU1. The persistence file should be 4GB and should be named `casper-rw` (or `writable` if you are fine with a 4GB only persistence) 
3. In Mac Linux USB Loader, create the bootable in *Create Live USB*
* Choose [Ubuntu latest iso](https://ubuntu.com/download/desktop)
* Make sure to untick *Skip the boot selection menu*
* Tick *This ISO lacks an EFI-enabled kernel*
4. At this stage you have a perfectly working Ubuntu USB Live stick. But it only has 4GB persistence.
5. In order to fix this, we will resize `casper-rw`.
* From a Linux session, with gparted, reformat MACUBUNTU2 into an ext4 partition.
* Copy the existing casper-rw into the new partition (replace below `/dev/sdXY` with the correct partition):
`sudo dd if=/media/ubuntu/MACUBUNTU1/casper-rw of=/dev/sdXY status=progress`
* Extend the partition to its full size by using either `resize2fs` or do a *Check* with gparted.
6. Here you go, you now have a bootable Ubuntu stick that you can use on MAC.
To boot it, Press option and start your Mac. Then when prompted choose option 2 (advanced) 5 (persistence) and 0 to start! 
