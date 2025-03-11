---
{"dg-publish":true,"tags":["linux","debian","FOSS"],"permalink":"/developer/linux/debian-linux-in-place-os-upgrade/","dgPassFrontmatter":true}
---

I wanted to upgrade my [[developer/Hardware/Pi4\|Pi4]] Debian OS from `bullseye 11` to `bookworm 12` without having to reimage and migrate any configurations or redownload apps

```shell
# First, ensure your system is up-to-date in it's current release.
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get full-upgrade

# If you haven't already, ensure all backups are up-to-date.  

# In a text editor, replace the codename of your release with that of the next release in APT's package sources
# For instance, the line
#    deb https://deb.debian.org/debian/ buster main
# should be replaced with
#    deb https://deb.debian.org/debian/ bullseye main
$ sudo nano /etc/apt/sources.list /etc/apt/sources.list.d/*

# If you are migrating to Bookworm or later, then a new repo for non-free firmware is available.
# If you wish, you can add non-free and non-free-firmware, depending on your specific needs.
# For instance, the line
#    deb https://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware
# or
#    deb https://deb.debian.org/debian/ stable main contrib non-free non-free-firmware

# Clean and update package lists
$ sudo apt-get clean
$ sudo apt-get update
```

> [!error] Mono repo was not signed
> This was a slightly concerning error as I didn't really know what this `mono` repo did. I believe it was installed when I ssh remoted in with [[developer/VS Code\|VS Code]]. I just moved that file `/etc/apt/sources.list.d/MONOREPO` to a temporary folder for now and then ran the update again and it worked

```
# Perform the major release upgrade, removing packages if required
# Interrupting this step after downloading has completed is an excellent way to stress-test your backups
$ sudo apt-get upgrade
$ sudo apt-get full-upgrade
```

> [!note] Lots of prompts
> During the upgrade I was asked a lot of `[Y]` prompts every so often, so don't think this will be a set and walk away. Expect to be running the terminal and glancing at it over the course of an hour or 2 (depending on computer's speed)

```
# Remove packages that are not required anymore
# Be sure to review this list: you may want to keep some of them
$ sudo apt-get autoremove

# Reboot to make changes effective (optional, but recommended)
$ sudo shutdown -r now
```

You can fire off `uname -a ; lsb_release -a` to tell you the current OS version. For me, I use trusty `neofetch`

```shell
       _,met$$$gg.          pi4@rpi
    ,g$$$$$$$$P.       -------
  ,g$P"     """Y$.".        OS: Debian GNU/Linux 12 (bookworm) aarch64
 ,$P'              `$$.     Host: Raspberry Pi 4 Model B Rev 1.5
',$P       ,ggs.     `$b:   Kernel: 6.1.21-v8+
`d$'     ,$P"'   .    $$    Uptime: 2 mins
 $P      d

---
## Credits
- [DebianUpgrade - Debian Wiki](https://wiki.debian.org/DebianUpgrade)
- [In-Place Upgrade Debian 11 Bullseye to 12 Bookworm (youtube.com)](https://www.youtube.com/watch?v=nMFtCDRz0YA)
- [Upgrade Raspberry Pi OS to the Latest Version (2024) – RaspberryTips](https://raspberrytips.com/update-raspberry-pi-latest-version/)     ,    $P    Packages: 1265 (dpkg)
 $:      $.   -    ,d$'    Shell: bash 5.2.15
 $;      Y$b._   _,d$P'      Terminal: /dev/pts/0
 Y$.    `.`"Y$$P"'         CPU: BCM2835 (4) @ 1.800GHz
 `$b      "-.__              Memory: 202MiB / 1849MiB
  `Y$
   `Y$.
     `$b.
       `Y$b.
          `"Y$b._
              `"""
```

---
## Credits
- [DebianUpgrade - Debian Wiki](https://wiki.debian.org/DebianUpgrade)
- [In-Place Upgrade Debian 11 Bullseye to 12 Bookworm (youtube.com)](https://www.youtube.com/watch?v=nMFtCDRz0YA)
- [Upgrade Raspberry Pi OS to the Latest Version (2024) – RaspberryTips](https://raspberrytips.com/update-raspberry-pi-latest-version/)