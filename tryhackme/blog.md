[Back](/tryhackme)

# Blog Writeup
[Blog](https://tryhackme.com/room/blog)

For this instance, let's do something a little different. You never know what system you will be having to deal with or what tools will be available, so instead of relying on the tools and pre-built systems I normally do, I'm going to set up a fresh virtual machine and go through the process until having this room completed.

# Virtual Machine Setup
Just in case we don't have a VM, let's go an get one.

## Download Linux
Since I've never messed with Arch Linux, I'm using Manjaro this time.
[Manjaro Linux](https://manjaro.org/)

## Virtual Machine Setup
If you are running a windows computer that supports hyper-v, running in hypervisor can be significantly more responsive than using virtualbox.
If you support running hyper-v, use that. Otherwise you can use virtualbox.
Gides on setting these up:
* [Manjaro on Hyper-V](/guides/manjaro_on_hyperv)
* [Manjaro on VirtualBox](/guides/manjaro_on_virtualbox)

## Initial Tool Setup
There are a few tools that I always need, so let's set those up.

Once Manjaro is booted up, time to jump into a terminal.

Having a sources directory is useful for all the things you will download through github.
```
mkdir ~/sources
cd /sources
```

We need to have access to git, so time to set it up:
```
sudo pacman -S git
```

I like a nice editor, and VSCode is my favorite flavor.
```
sudo pacman -S code
```

Plain terminals are boring, tmux just adds that little polish that makes things more fun.
```
sudo pacman -S tmux
```

We need nmap for most pentesting and scanning operations, let's get that now.
```
sudo pacman -S nmap
```

My manjaro already has python 3 and pip3 (the python package manager). Verified with entering:
```
which python
which pip
```

For the blog room, I see it's a wordpress instance, so I need wpscan to make things easy.
```
sudo pacman -S wpscan
```
Note - I had to run this command twice to get it to install, my network is a bit unstable.

We need openvpn to connect to TryHackMe's network, thankfully we already have it.
```
which openvpn
```

we might as well pull in hydra and john the ripper.
```
sudo pacman -S hydra
sudo pacman -S john
```
