---
{"dg-publish":true,"tags":["game","gaming","linux","windows","MacOs","playstation","nintendo"],"permalink":"/developer/emulation/controller-gamepad-joypad-management/","dgPassFrontmatter":true}
---

I've taken on the foolish challenge of gaming on Linux. For the most part I've had good success weaving through the small inconveniences with many modern games, emulation, and playing with 

## The Goal
I'm aiming to have a setup similar to how the Nintendo Switch handles controllers (GUI not necessary but highly wanted). My list of needs are
1. Some kind of virtual 1-8 gamepad slots that can be mapped into any game, emulator, etc
2. Remappable controllers (i.e. swapping player 1 to player 2, or re order player 2 to player 1 if controller 1 disconnects)
3. No need to re-configure Controller mapping if switching type (player 1 goes from **Gamecube** to **PS3** controller, but mapping is same , i.e. button `x` and button `a` stay as **accept/ok** function  ) 
4. Handle disconnect re-connect if switching ports, batteries die, etc.
5. BONUS: show assigned port to controller's LED
6. BONUS: controller that launches game becomes player 1
## Controllers, Gamepads, Joy sticks
### PS3 Sixaxis Controller
So far the [PS3 Sixaxis](https://en.wikipedia.org/wiki/Sixaxis) controller has been the best gamepad with the least amount of headaches
#### Linux Mint
**Pros:**
- Easy Wired or Bluetooth Pairing
- Persistent Bluetooth re-paring
- Has modern inputs for most console/platforms games
#### Windows 10
Does not work out of the box. try this? [XInput Wrapper for DS3(pcsx2.net)](https://forums.pcsx2.net/Thread-XInput-Wrapper-for-DS3-and-Play-com-USB-Dual-DS2-Controller)
### Nintendo Pro Controller (3rd Party)
Here is when I started having problems. I have two 3rd party Pro controllers I've had vary degrees of success when connecting to different machines.
- [PowerA Enhanced Wireless Controller for Nintendo Switch - Link Silhouette](https://www.amazon.com/PowerA-Enhanced-Wireless-Controller-Nintendo-Switch/dp/B07GXJHRVK/ref=sr_1_2?crid=VXSSPMP7TJ8W&dib=eyJ2IjoiMSJ9.NWqzeQknrQtnCYD7csWB-nGj87YKx-UticKanPY_8GaRK2rE2zjobE6QrGkP1frhgdg986YRk0stqW2rZwrhlwKq0TW4WLFIA3DIhDNVaV4.DIWDi3sPo5gsM__yN9Cdmb3IcRXa_vmKI4Hvg35D_C8&dib_tag=se&keywords=lic+pro+controller+power+a&qid=1728673215&sprefix=lic+pro+controller+power+a%2Caps%2C75&sr=8-2)
- [PowerA Wireless GameCube Style Controller for Nintendo Switch - Purple](https://www.amazon.com/PowerA-Wireless-Controller-Nintendo-Switch-GameCube/dp/B07GXLBCC3/ref=sr_1_6?crid=3OCM9EO4T4RQN&dib=eyJ2IjoiMSJ9.gnQt7WH-UkN_1ZrbF-tXigQK6qssNN3u0b3n_NmsmNI_x4Nqnsa3o7xDS-YZTfWaFIpCPOmb23_6e-3D4SCeyA.fsFM7OGjfabkAyeZWQYcY_GPyyfiJrLCGbcm9p4AgH0&dib_tag=se&keywords=lic+pro+controller+power+a+gamecube&qid=1728673227&sprefix=lic+pro+controller+power+a+gamecube%2Caps%2C68&sr=8-6)

The game I used to test these controllers is a cross platform desktop game
- https://cravecraw.itch.io/hammer-dongers
- [Snow Flow by Jon Topielski (itch.io)](https://jontopielski.itch.io/snow-flow)
#### Windows 10
Nintendo controllers do not work out of the box. 

> [!tip] BetterJoy
> Use this app to make compatability with **Windows 10** easy [Davidobot/BetterJoy(github.com)](https://github.com/Davidobot/BetterJoy) 

https://askubuntu.com/questions/1331009/bluetooth-on-ubuntu-20-04-constantly-disconnecting

---
## Credit
- [Using Nintendo Switch controllers on Linux : r/linux_gaming (reddit.com)](https://www.reddit.com/r/linux_gaming/comments/fxwh54/using_nintendo_switch_controllers_on_linux/?sort=new)
- [Setting Up Multiple Controllers In EmuDeck : r/SteamDeck (reddit.com)](https://www.reddit.com/r/SteamDeck/comments/xm1vew/setting_up_multiple_controllers_in_emudeck/)