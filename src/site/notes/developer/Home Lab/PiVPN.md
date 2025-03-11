---
{"dg-publish":true,"permalink":"/developer/home-lab/pi-vpn/","dgPassFrontmatter":true}
---

>[!info] [PIVPN: Simplest way to setup a VPN](https://pivpn.io/)
>The simplest way to setup and manage a VPN,  
designed for Raspberry Pi™.
>

## install
It is recommended to install [[developer/Home Lab/Pi-hole\|Pi-hole]] first as it automates **DNS** routing upon installing **PiVPN**

>[!important ]
>don't forget to set boot option to wait for network (i.e. `raspi-config`)

## updating
[link](https://docs.pivpn.io/install/#update)

```shell
run pivpn update
```

VPN protocols are updated via system package manager

> [!Note] 
> Read [link](https://docs.pivpn.io/openvpn/#updating-openvpn) or [link](https://docs.pivpn.io/wireguard/#updating-wireguard) for information on how to update the VPN protocol.

## backup
also a backup of [[developer/Home Lab/Pi-hole\|Pi-hole]]

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

## Migrating PiVPN & Wireguard

Backup your server with `pivpn -bk` copy the tar archive to your computer. example using scp on Linux:

`scp <user>@<server>:~/pivpnbackup/<archivename> <path/on/local>`

1.  Backup the current (new instance) install: `sudo cp -r /etc/wireguard /etc/new_wireguard_backup`
2.  Extract the backup archive: `tar xzpfv <archive name>`
3.  Copy the extracted content: `sudo cp -r etc/wireguard /etc`
4.  Restart the wireguard service: `sudo systemctl restart wg-quick@wg0`

## tunnels connecting but no internet after reinstall
`sudo cat /etc/wireguard/wg0.conf`
`sudo iptables -t nat -A POSTROUTING -s <FIRST_IP_IN_CONF>/<#> -o eth0 -j MASQUERADE`
then turn off and on the tunnel

## Port UDP (and TCP too maybe idk)

### connections 
- Running on [[developer/Hardware/Pi4\|Pi4]]
- paired with [[developer/Home Lab/Pi-hole\|Pi-hole]] for ad blocking on the go

## No internet browsing when connected
```bash
# debug help
pivpn -d 
```

```bash
::::		Self check		 ::::
:: [OK] IP forwarding is enabled
:: [OK] Ufw is enabled
:: [ERR] Iptables MASQUERADE rule is not set, attempt fix now? [Y/n] Y
Firewall reloaded
Done
:: [OK] Ufw input rule set
:: [ERR] Ufw forwarding rule is not set, attempt fix now? [Y/n] Y
```

## Install PiVPN on non supported platforms like Linux Mint
- https://github.com/pivpn/pivpn/pull/1040#issuecomment-634077570
```shell
cat /etc/os-release 

UBUNTU_CODENAME=jammy
```

```
curl https://raw.githubusercontent.com/pivpn/pivpn/master/auto_install/install.sh > pivpn-installer.sh

nano pivpn-installer.sh
```

`pivpn-installer.sh`
```sh
...
distroCheck(){
	# if lsb_release command is on their system
	if command -v lsb_release > /dev/null; then

		PLAT=$(lsb_release -si)
		OSCN=$(lsb_release -sc)

	else # else get info from os-release

		# shellcheck disable=SC1091
		source /etc/os-release
		PLAT=$(awk '{print $1}' <<< "$NAME")
		VER="$VERSION_ID"
		declare -A VER_MAP=(["9"]="stretch" ["10"]="buster" ["16.04"]="xenial" ["18.04"]="bionic")
		OSCN=${VER_MAP["${VER}"]}
		
	fi
## !👇 HERE
	OSCN="jammy"
	PLAT="Ubuntu"
	
	if [ "$PLAT" = "Debian" ] || [ "$PLAT" = "Ubuntu" ]; then
		DPKG_ARCH="$(dpkg --print-architecture)"
		if [ "$DPKG_ARCH" = "amd64" ] || [ "$DPKG_ARCH" = "i386" ]; then
			X86_SYSTEM=1
		else
			X86_SYSTEM=0
		fi
	fi
...
```

```sh
sudo chmod +x pivpn-installer.sh
```

---
## Citations 
- [Wireguard - docs.pivpn.io](https://docs.pivpn.io/wireguard/)
- [PiVPN not working properly : raspberry_pi (reddit.com)](https://www.reddit.com/r/raspberry_pi/comments/9i6rmm/pivpn_not_working_properly/)
- [Make local devices accessible - Pi-hole documentation](https://docs.pi-hole.net/guides/vpn/wireguard/internal/)