---
{"dg-publish":true,"tags":["windows","batch","powershell","terminal"],"permalink":"/developer/windows-10/windows-close-programs-on-loggout-or-sleep/","dgPassFrontmatter":true}
---

The day is done and my work (or gaming) at my [[developer/Windows 10/Microsoft Windows\|Microsoft Windows]] PC has come to an end. I usually send my PC to sleep so I can quick boot back into my desktop upon next session. Problem is that I don't like to close out of every single app one click at a time. 

> [!tip] Background Apps
> This is especially for apps that minimize to the tool tray even when I hit the `x` to *close* the app

When I wake my PC back up I want a fresh start. I have no problem launching the apps manually as I need them, but I would like to start from wake without apps such as **Discord** m **Steam**, or **EpicGamesLauncher** always running.
## Bat File
```bat
wmic process where name="Discord.exe" call terminate
wmic process where name="OpenRGB.exe" call terminate
wmic process where name="steamwebhelper.exe" call terminate
wmic process where name="steam.exe" call terminate
wmic process where name="EpicGamesLauncher.exe" call terminate
```

This `wmic ... terminate` also takes care of any sibling services running and gracefully shuts down the whole app. 

## Task Scheduler
Now to launch this script every time the computer goes to sleep (or in this case logs off)

> [!warning] non locking desktop
> If your desktop doesn't lock when going to sleep. I'm not sure if this will work for you. You can still poke around in the **Event Viewer** to see what you can use to trigger your script.

![attachments/Pasted image 20250123120707.png](/img/user/attachments/Pasted%20image%2020250123120707.png)
### Trigger
![attachments/Pasted image 20250123120911.png](/img/user/attachments/Pasted%20image%2020250123120911.png)

## Finding Event
If you want the script to trigger from a different event you can browse the **Event Viewer**.

![attachments/Pasted image 20250123121159.png](/img/user/attachments/Pasted%20image%2020250123121159.png)

Here I've fount Event ID `42` might even work better for what I need. But I'm sticking to `7002` for now because it already just works
## 2nd Option Command
If the program isn't closing you can try `taskill /IM` to forcefully close the program. This is not a graceful solution but it can work on those stubborn apps.

```bat
taskkill /IM Discord.exe /F
taskkill /IM OpenRGB.exe /F
taskkill /IM steam.exe /F
taskkill /IM steamwebhelper.exe /F
taskkill /IM EpicGamesLauncher.exe /F
```

---
## Credit
- https://stackoverflow.com/questions/53308169/terminating-a-process-from-cmd-softest-to-hardest