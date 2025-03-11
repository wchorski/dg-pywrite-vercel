---
{"dg-publish":true,"permalink":"/developer/home-lab/pi-hole/","dgPassFrontmatter":true}
---

> [!info] [Pi-hole – Network-wide protection](https://pi-hole.net/)
> Network-wide Ad Blocking

## install
thanks to [Craft Computing](https://www.patreon.com/CraftComputing) for putting together a easy to follow tutorial for setting up Pi-hole with Unbound DNS for extra privacy 
[You're running Pi-Hole wrong! Setting up your own Recursive DNS Server! - YouTube](https://www.youtube.com/watch?v=FnFtWsZ8IP0&t=551s)

>[!important ]
>don't forget to set boot option to wait for network (i.e. `raspi-config`)

## Update script with Logging

```bash
#! /bin/bash

DATE=`date +%Y-%m-%d`
LOG="./update.log"
echo $DATE > $LOG

echo "[ pihole -up ]" >> $LOG
pihole -up         >> $LOG
echo "=============" >> $LOG

echo "[ apt update ]" >> $LOG
apt-get update      >> $LOG
echo "============" >> $LOG

echo "[ apt upgrade ]" >> $LOG
apt-get upgrade -y  >> $LOG
echo "============" >> $LOG
```

## backup
also a backup of [[developer/Home Lab/PiVPN\|PiVPN]]

```bash
#! /bin/bash

DATE=`date +%Y-%m-%d`

echo '======================' > backup.log
echo $DATE >> backup.log


if pihole -a -t\
&& mv /home/pi4/scripts/backup/pi-hole-rpi-teleporter* /home/pi4/scripts/backup/pihole\
; then
        echo 'pihole SUCCESS' >> backup.log
else
        echo 'pihole failed' >> backup.log
fi


if pivpn -bk\
&& sudo mv /home/pi4/pivpnbackup/* /home/pi4/scripts/backup/pivpn\
; then
        echo 'pivpn SUCCESS' >> backup.log
else
        echo 'pivpn failed' >> backup.log
fi
```

## Resolve DNS for the Raspberry PI

Try to ssh into Raspberry Pi (or whatever you host **Pi-Hole** on) and try to ping any local DNS entries (that you would have set up in [[developer/Pretty URLS for Local DNS Records\|PiVPN]]). These are not reachable because the Pi itself is not resolving to the local domain name service because it is higher up the stream. We can manually tell the pi to resolve itself (assuming Pi-hole and PiVPN are running on the same machine).

```shell
sudo nano /etc/dhcpcd.conf
```

find the line and add `127.0.0.1` and save.

```shell
static domain_name_servers=127.0.0.1
```

```shell
sudo service dhcpcd restart
```

## Credits
- [Raspberry Pi DNS Settings: How to Change the DNS - Pi My Life Up](https://pimylifeup.com/raspberry-pi-dns-settings/)
- [Pi-hole's host machine can't resolve local domains - Help / Community Help - Pi-hole Userspace](https://discourse.pi-hole.net/t/pi-holes-host-machine-cant-resolve-local-domains/48008)