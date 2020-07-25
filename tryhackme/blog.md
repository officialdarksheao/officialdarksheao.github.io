[Back](/tryhackme)

# Blog Writeup
[Blog](https://tryhackme.com/room/blog)

For this instance, let's do something a little different. You never know what system you will be having to deal with or what tools will be available, so instead of relying on the tools and pre-built systems I normally do, I'm going to set up a fresh virtual machine and go through the process until having this room completed.

# Virtual Machine Setup
Just in case we don't have a VM, let's go an get one.

## Download Linux
Since I've never messed with Arch Linux, I'm using Manjaro this time.
[Manjaro Linux](https://manjaro.org/)

## Alternatives
If you are running a windows computer that supports hyper-v, running in hypervisor can be significantly more responsive than using virtualbox.
I will post a guide on setting this up [Here](/guides/manjaro_on_hyperv)

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
(to be continued)


