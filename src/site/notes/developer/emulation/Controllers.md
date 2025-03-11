---
{"dg-publish":true,"tags":["emulation","gamer","gaming","3rd-party","linux"],"permalink":"/developer/emulation/Controllers/","dgPassFrontmatter":true}
---

## Controller manager
try this on deb - https://github.com/jgeumlek/MoltenGamepad

## PS3 Dualshock Controller Bluetooth
When connecting my controller to my **Linux Mint** emulation machine, it would ask for a pin. This was strange as I never had to do this before, and it would worked wired just fine. Here is the work around

```bash
sudo nano /etc/bluetooth/input.conf

# find and change line
ClassicBondedOnly=false

# after saving file, restart service
sudo systemctl restart bluetooth
```

## Linux Tweaking
```shell
nano /etc/udev/rules.d/
udevadm control --reload 
udevadm info -a /dev/input/js1
```

https://selfmadepenguin.wordpress.com/2024/02/14/how-i-solved-my-gamecontroller-problems/

https://wiki.archlinux.org/title/Gamepad

https://blog.rubenwardy.com/2023/01/24/using_sdl_gamecontroller/

https://www.softpedia.com/get/Gaming-Related/SDL2-Gamepad-Tool.shtml

https://generalarcade.com/gamepadtool/

https://github.com/mdqinc/SDL_GameControllerDB

https://www.reddit.com/r/SteamDeck/comments/t8al0i/install_arch_packages_on_your_steam_deck/
```shell
sudo steamos-readonly disable
sudo pacman-key --init
sudo pacman-key --populate
```

## Credits
- [updates - Why does official PS3 bluetooth controller no longer work and pin code suddenly required? - Ask Ubuntu](https://askubuntu.com/questions/1497783/why-does-official-ps3-bluetooth-controller-no-longer-work-and-pin-code-suddenly)