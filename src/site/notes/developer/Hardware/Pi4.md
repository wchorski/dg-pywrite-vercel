---
{"codename":"rpi4","dg-publish":true,"permalink":"/developer/Hardware/Pi4/","dgPassFrontmatter":true}
---


## Raspberry Pi 4b 4gb

A great start for anyone who wants to start running digital services

### install
```shell
       _,met$$$gg.          pi4@rpi
    ,g$$$$$$$$P.       -------
  ,g$P"     """Y$.".        OS: Debian GNU/Linux 12 (bookworm) aarch64
 ,$P'              `$$.     Host: Raspberry Pi 4 Model B Rev 1.5
',$P       ,ggs.     `$b:   Kernel: 6.1.21-v8+
`d$'     ,$P"'   .    $$    Uptime: 2 mins
 $P      d

Surprised that it isn't **Raspberry Pi OS**? Instead I run the Debian 64 bit version as recommended by [[developer/Home Lab/Home Assistant\|Home Assistant]] **Supervised** install. I'm a little bummed I don't get the cool Raspberry ascii art from `neofetch`, but honestly, the developer experience is pretty much identical 


## system wide backup

`full-img-backup.sh`
```shell
#!/bin/bash

#TODO clean out or ignore other folders and files before the copy

FILE_NAME="pi4--$(date +%y_%B_%d).img"
DEST="/ntwmnt/octo8/backups/pi4/img"
LOG="./backup-status.log"

echo "[[ creating system wide backup \| creating system wide backup ]]"
echo $FILE_NAME

#backup img of SD card
if sudo dd bs=4M if=/dev/mmcblk1p2 of=$DEST/$FILE_NAME\
&& sudo zip -jm "$DEST/$FILE_NAME.zip" $DEST/$FILE_NAME\
; then
        sudo echo $FILE_NAME > $LOG
        sudo echo "SUCCESS" >> $LOG
else
        sudo echo $FILE_NAME > $LOG
        sudo echo "FAILURE" >> $LOG
fi
```

---
## Programs & Apps
1. [[developer/Home Lab/Pi-hole\|Pi-hole]]
2. [[developer/Home Lab/PiVPN\|PiVPN]]
3. [[developer/Home Lab/Home Assistant\|Home Assistant]]
4. [[developer/Home Lab/DuckDNS\|DuckDNS]]
5. [[developer/Home Lab/Hyperion\|Hyperion]]

[[developer/Hardware/Homelab Hardware PC Builds\|Homelab Hardware PC Builds]]     ,    $P    Packages: 1265 (dpkg)
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

Surprised that it isn't **Raspberry Pi OS**? Instead I run the Debian 64 bit version as recommended by [[developer/Home Lab/Home Assistant|Home Assistant]] **Supervised** install. I'm a little bummed I don't get the cool Raspberry ascii art from `neofetch`, but honestly, the developer experience is pretty much identical 


## system wide backup

`full-img-backup.sh`
{{CODE_BLOCK_1}}

---
## Programs & Apps
1. [[developer/Home Lab/Pi-hole|Pi-hole]]
2. [[developer/Home Lab/PiVPN|PiVPN]]
3. [[developer/Home Lab/Home Assistant|Home Assistant]]
4. [[developer/Home Lab/DuckDNS|DuckDNS]]
5. [[developer/Home Lab/Hyperion|Hyperion]]

[[developer/Hardware/Homelab Hardware PC Builds|Homelab Hardware PC Builds]]