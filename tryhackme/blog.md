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

## Setting up Blackarch

There are a few tools that I always need, so let's set those up.

Once Manjaro is booted up, time to jump into a terminal.

Having a sources directory is useful for all the things you will download through github.
```
mkdir ~/sources
cd /sources
```

BlackArch is a huge collection of tools that we can download, so we can switch our package manager to use BlackArch instead.
```
cd ~/sources
mkdir blackarch && cd blackarch
curl -O https://blackarch.org/strap.sh
```
verify the sum matches.
```
sha1sum strap.sh
```
If that returns anything other than `9c15f5d3d6f3f8ad63a6927ba78ed54f1a52176b`, stop and go to [BlackArch](https://blackarch.org/downloads.html#install-repo) and check if something has been updated. We need to make sure the repo is accurate.

We need to make this script executable, then run it as root.
```
chmod +x strap.sh
sudo ./strap.sh
```

## Tool Setup

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

There is a blackarch package for metasploit
```
sudo pacman -S metasploit
```

Dirbuster and gobuster are excelent tools to scan webpages with, and enum4linux is a great tool to find easy vulneribilities.
```
sudo pacman -S dirbuster
sudo pacman -S gobuster
sudo pacman -S enum4linux
```

for any SQL injection vulneribility found, sqlmap can do some scary things.
```
sudo pacman -S sqlmap
```

for anything to do with data hidden (or to be hidden) in images, steghide
```
sudo pacman -S steghide
```

Netcat for interacting with communication over networks, and for listening for reverse shells
```
sudo pacman -S openbsd-netcat
```

exploitdb is a great command-line version of [exploit-db](https://www.exploit-db.com/)
```
sudo pacman -S exploitdb
```

## To TryHackMe!

Off to the challenge!
[Blog](https://tryhackme.com/room/blog)

If this is your first time on TryHackMe, make sure you have downloaded your VPN connection file.

Deploy the challenge box and copy the ip, to make things easier, in your console set the IP to a variable for easy use.
(Your IP will likely be different)

![Deployed Box](/images/tryhackme/blog/1.png)

```
export ip=10.10.14.235
```

Now that the IP is ready, let's spin up tmux. it will let us multitask better, as well as monitor as we go to see accesses to local servers and progress of scans.
```
tmux
```

We need to connect to the VPN now. My vpn connection file has been stored in my home directory /vpn, you will need to replace with your own.
```
openvpn ~/vpn/darksheao.ovpn
```