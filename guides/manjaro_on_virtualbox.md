# Setting up Manjaro with VirtualBox

Make sure to download an iso of Manjaro.
[Manjaro Linux](https://manjaro.org/)

## Download VirtualBox
Virtualbox is a decent option, so I'm going with that for this exercise.
[Oracle VirtualBox](https://www.virtualbox.org/)

Also grabbing the extension pack while I'm at it.
After installing, don't forget to install the extension pack.

## Build a VM

Launch VirtualBox and select "New" from the top menu.

![First Time Launching VirtualBox](/images/tryhackme/blog/1.jpg)

We know we are building for Manjaro, which is Arch linux.

![Initial Settings for Manjaro Box](/images/tryhackme/blog/2.jpg)

The hardware you are running will make an impact on how you set up the RAM allocation, personally I like to have a minimum of 4g

![4g of ram](/images/tryhackme/blog/3.jpg)

We need a hard drive, so create a virtual disk now.

![create-a-disk](/images/tryhackme/blog/4.jpg)

VTI format is fine for this, and to keep your system clean, dynamically allocated is a good choice.
I'll run with 64g for my drive so we can have some room to play.

![disk settings](/images/tryhackme/blog/5.jpg)

We do need to put our manjaro iso into our virtual disk drive, so let's go into our Manjaro machine's settings

![open settings](/images/tryhackme/blog/6.jpg)

From here go into the storage tab, then for the disk drive select to "choose a disk file"

![insert your install disk](/images/tryhackme/blog/7.jpg)

Now we can launch our machine and begin the install process.

( TO BE CONTINUED )
( I have to disable hyper-v to continue with the tutorial and I just don't want to right now )