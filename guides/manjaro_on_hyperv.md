# Setting up Manjaro Linux using Hyper-V

Ensure you have hyper-v and the windows hypervisor platform enabled. If you are running windows 10 home edition, you will need to upgrade to pro for this to be available.

> Done with help from forum article [Installing Manjaro in Hyper-V with Enhanced Session Support](https://forum.manjaro.org/t/installing-manjaro-in-hyper-v-with-enhanced-session-support/79394)

Go to the "Turn Windows features on or off" menu.

![Windows Features Start Menu Search](/images/guides/manjaro_on_hyperv/1.jpg)

Ensure the "Hyper-v" and the "Windows Hypervisor Platform" are enabled. If this is the first time you are enabling this, you will need to restart your computer.

![Enable Hyper-v](/images/guides/manjaro_on_hyperv/2.jpg)

You will now be able to load the Hyper-V Manager.

![Load Hyper-V Manager](/images/guides/manjaro_on_hyperv/3.jpg)

We will need to make a tweak to the Hyper-V settings

![Hyper-v settings](/images/guides/manjaro_on_hyperv/4.jpg)

Ensure the "Enhanced Session Mode" is enabled. This will greatly increase the performance and responsiveness of our VM.

![Enhanced session mode](/images/guides/manjaro_on_hyperv/5.jpg)

Now we need to go into the virtual switch settings.

![Virtual Switch Settings](/images/guides/manjaro_on_hyperv/6.jpg)

Create a new virtual switch, of the type "external". Then click "Create Virtual Switch".

![New External Switch](/images/guides/manjaro_on_hyperv/7.jpg)

In the newly create switch's settings, let's name it "Bridge". (This can be anything, just a way to identify the connection type). Make sure the connection interface is a valid one, and that it allows the host system to share the interface. Click "Apply". It will warn you that it may disrupt connections, proceed.

![Making the Bridge](/images/guides/manjaro_on_hyperv/8.jpg)

Now that we have the settings ready and a virtual network switch created, Let's create the VM. Select "New" then "Virtual Machine".

![New Machine](/images/guides/manjaro_on_hyperv/9.jpg)

In the dialog that opens, we want to create custom settings, so make sure to click "Next" instead of "Finish".

To make things simple, I'm naming the machine "Manjaro", and I am specifying where I want it's data to live. Make sure you pick a hard drive with a decent amount of free space.

![Machine name and location](/images/guides/manjaro_on_hyperv/10.jpg)

For the sake of this exercise, we are making a Generation 2 Machine.

![Gen 2](/images/guides/manjaro_on_hyperv/11.jpg)

For Memory, since this is a graphical desktop environment, I like having a minimum of 4g to play with. Leaving dynamic memory on so it can borrow more if it needs to.

![4g LTE](/images/guides/manjaro_on_hyperv/12.jpg)

Networking is where we make use of that "Bridge" connection we made earlier. Select it. (or whatever you named it)

![Bridge'd](/images/guides/manjaro_on_hyperv/13.jpg)

For hard drive, I like a nice round 64g.

![2 to the power of 6](/images/guides/manjaro_on_hyperv/14.jpg)

I already have the < INSERT MANJARO EDITION I GOT WORKING HERE > iso ready to go, so I select that for the installation media. This will insert the iso (virtual dvd) into the VM's virtual dvd drive.

![Less is more](/images/guides/manjaro_on_hyperv/15.jpg)

Accept the summary, and watch it build!
Now we can't launch yet, we have a little more machine settings to adjust. Right click your new Manjaro machine and select settings.

![MOAR SETTINGS](/images/guides/manjaro_on_hyperv/16.jpg)

The Manjaro ISO will only run if you disable the VM's "Secure Boot" Option. So, turn it off and apply.

![Unsecure it](/images/guides/manjaro_on_hyperv/17.jpg)

Now we can actually start our machine! Double clicking and the selecting "start" will get this booted up.

The First screen we will see will be the bootloader. You can either push enter, or arrow around to change the timezone if you feel like it.

This is the process of booting into "Live" mode for Manjaro. This does not actually install it to our virtual hard drive yet.

![it lives](/images/guides/manjaro_on_hyperv/18.jpg)

You will see it run through pages of text and console logs, but eventually "hang", usually around messages like "Light Display Manager" and "TLP System Startup/Shutdown". This is because the display drivers for this version of Manjaro don't play nicely with hyper-v. We have a workaround though!

![it dies?](/images/guides/manjaro_on_hyperv/19.jpg)

Push Control + Alt + F2. This switches us to a tty console.

![login plz](/images/guides/manjaro_on_hyperv/20.jpg)

Login with the default "manjaro" user, passowrd "manjaro".

You will be presented with a good old console prompt.

We need to run these commands to install and setup the graphics drivers:

```
su
pacman –Sy
pacman –S xf86-video-fbdev
systemctl restart lightdm
```
> Explaination:
> `su` -- Change to root user (the text will go red as a warning)
> `pacman -Sy` -- Sync the PACkage MANager with the remote repositories
> `pacman -S xf86-video-fbdev` -- Search for and attempt installing the xf86 video drivers
> You will need to press enter when it asks you if you want to install things.
> `systemctl restart lightdm` -- Let's reboot just the display system!

Now we can actually see the desktop! Let's Go ahead and install manjaro.

( TO BE CONTINUED )