---
{"dg-publish":true,"tags":["emulation","game","gaming","retro"],"permalink":"/developer/emulation/Batocera on SteamDeck/","dgPassFrontmatter":true}
---

I stumbled across my own SteamDeck, so of course the natural first thing to do is see how it fares with emulating other gaming consoles. 

> [!note] I refrain from saying the word *retro* as I was born in the 90's and want to believe the **3DS** isn't a retro console (or else my bones will wither into dust)

### EmuDeck
Through all the rave reviews, I tried [EmuDeck](https://www.emudeck.com/) first as it seemed like the most *one click install* option. I'm all about automation, so let's let all the hard work of these nerds set my system up in perfect configuration
### Batocera
https://batocera.org/

> [!tip] Multiplayer
> The main reason why I chose Batocera was the way it handles multiple gamepads. 

Being a portable device, I need it to be easy pairing and auto port assigning gamepads across emulators. Something that **EmuDeck** can't offer. I don't want to be fiddling around assigning controllers per emulator, especially when I don't have a keyboard and mouse on the ready. 

>[!warning] Older emulators
>Note that Batocera uses older versions of some emulators like [[developer/emulation/Dolphin Emu\|Dolphin Emu]]. If you are in need of the bleeding edge features, you may want to try something else. But I like consistency, so this works for me. 
#### Samba  
With your **Batocera** machine on and connect to a private network, from another computer access the device. With Linux Mint I can drop `smb://steam-deck.local/share/` into my file browser address bar and get straight there  
  
you may need to replace `steam-deck.local` with your machine's hostname or IP address.  

##### Windows Network

On Windows you can check under your Networks tab and find it.

> [!error] Password
> You may want to enable a password for your deck's samba share if you ever plan on connecting it to a *public netowrk*

![attachments/Pasted image 20241125221410.png](/img/user/attachments/Pasted%20image%2020241125221410.png)
#### AR Geko Code How to  
add codes in Dolphin-emu through text files remotely  
#### Find Game file  
`smb://steam-deck.local/share/saves/dolphin-emu/GameSettings/*.ini`  
  
example **Mario Kart Double Dash** is `GM4E01.ini`  
#### Add Code  
Edit the `.ini` file. Here I'm adding 2 widescreen codes 

```ini  
[Gecko]  
$widescreen-ralph  
04003F20 809F4EB4  
04003F24 28030003  
04003F28 40800008  
...  
  
$widescreen-menu-ralph  
06003860 0000007C  
807B00DC 28030000  
4182006C 3CA0C2CC  
3CC04432 3C000953  
...  
  
[Gecko_Enabled]  
$widescreen-ralph  
$widescreen-menu-ralph  
```  
  
This adds the code and enables without having to touch the UI

> [!tip] Battery Life
> I won't say I did any extensive testing, but I believe the battery life is much more efficient when running games or idling Batocera.
> 
## cloud saves

## Scraper
Make An account https://www.screenscraper.fr/index.php. this is the best scraper by far.
### rclone
-https://github.com/peterbozso/batocera-backup-service
### syncthing
- https://wiki.batocera.org/syncthing
### Troubleshooting
- [no sound after wake](https://github.com/batocera-linux/batocera.linux/issues/12342)
- change brightness ingame [not planned](https://github.com/batocera-linux/batocera.linux/issues/8833)
- install pkg files and add to frontend [tutorial](https://wiki.batocera.org/systems:ps3)