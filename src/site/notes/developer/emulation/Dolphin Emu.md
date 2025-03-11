---
{"dg-publish":true,"permalink":"/developer/emulation/Dolphin Emu/","dgPassFrontmatter":true}
---

Nintendo Gamecube & Wii emulator

## Mayflash gamecube controller adapter on Linux
[Mayflash Gamecube controller adapter + Linux + Dolphin/Steam (seanyeh.com)](https://www.seanyeh.com/pages/mayflash_gamecube_adapter_linux_dolphin_steam/)

Paste the following into `/etc/udev/rules.d/51-gcadapter.rules`:

```
SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ATTRS{idVendor}=="057e", ATTRS{idProduct}=="0337", MODE="0666"
```

Then, install [wii-u-gc-adapter](https://github.com/ToadKing/wii-u-gc-adapter). (There is a package in the AUR)

After installing, run `sudo modprobe uinput`

Now, you can run the adapter with `wii-u-gc-adapter`

### Dolphin
Make sure the Mayflash adapter is set to **"Wii U / Switch"** setting. In Dolphin, go to `Controller Setings`, set the controller to "GameCube Adapter for Wii U". Your controller should now work in Dolphin.

## Mayflash Dolphinbar Wireless Sensor

I have a [USB sensor bar](https://www.mayflash.com/product/6.html) that connects Wii-motes via bluetooth and senses position with IR. I was having trouble with Wii-motes only connecting when running **Dolphin Emu** with `root` access, which ain't right. Here are the steps to allow your normal user permissions to use the sensor bar

1. make a file like `/etc/udev/rules.d/52-dolphinbar.rules` ('52-dolphinbar is an abritrary name')
2. paste `SUBSYSTEM=="hidraw", ATTRS{idVendor}=="057e", ATTRS{idProduct}=="0306", MODE="0666"`
3. reload rules → `sudo udevadm control --reload-rules`

### Regular sensor bar Firmware Update
Update the bar so it can act as a regular IR sensor bar (useful for frontends like [[developer/emulation/Batocera on SteamDeck\|Batocera]])
https://forums.dolphin-emu.org/Thread-dolphinbar-and-bluetooth-passthrough
## Recover backup
If you don't have any new game saves on your new computer you can do this. Go to new computer and rename the folder called "Dolphin Emulator" to something like "Dolphin Emulator BAK"

`C:/Users/YOUR USERNAME/Documents/Dolphin Emulator/`

Then go to old computer and again find the folder

`C:/Users/YOUR USERNAME/Documents/Dolphin Emulator/`

And copy this folder into your new computer. Should be same location. That will bring all your settings and saves
### Steam
Make sure the Mayflash adapter is set to "PC" setting. If you haven't already, run `wii-u-gc-adapter`

## Widescreen and 60fps Hacks Codes
2 packs that include many games and region's AR + Geko codes for Widescreen and 60fps Gamecube games
- [Part 1](https://forums.launchbox-app.com/files/file/4040-the-nintendo-gamecube-widescreen-project-pre-configured-anamorphic-169-for-dolphin-emulator-part-1-native-169-games/)
- [Part 2](https://forums.launchbox-app.com/files/file/4062-the-nintendo-gamecube-widescreen-project-part-2-pre-applied-ar-gecko-widescreen-cheats-hacks-anamorphic-169-for-dolphin-emulator/)

## Credits
- [Mayflash DolphinBar on Linux (dolphin-emu.org)](https://forums.dolphin-emu.org/Thread-mayflash-dolphinbar-on-linux)
- [Help me get Dolphinbar working on Arch Linux? : emulation (reddit.com)](https://www.reddit.com/r/emulation/comments/3gjmy2/comment/ctyz26e/)
- [Any way to get all game save data + miis from one computer running dolphin to another? : r/DolphinEmulator (reddit.com)](https://www.reddit.com/r/DolphinEmulator/comments/sa3ypn/any_way_to_get_all_game_save_data_miis_from_one/)
- [/shared2/menu/FaceLib/RFL_DB.dat - WiiBrew](https://wiibrew.org/wiki//shared2/menu/FaceLib/RFL_DB.dat)
- [[developer/emulation/Game Emulation\|Game Emulation]]