---
{"dg-publish":true,"permalink":"/developer/Android/Removing Samsung and Google Bloatware from Android 12/","dgPassFrontmatter":true}
---


I tried installing [LineageOS](https://lineageos.org/) on my Samsung Galaxy s9 and s10 phones. Unfortunately my [model wasn't supported](https://wiki.lineageos.org/devices/beyond1lte/) (just one letter off 😑). 

I still wanted to use this phone, but learn how to remove all the annoying Samsung and Google bloatware that they make impossible to remove in the phone's UI
## List of apks to unintall

Following this list [ADB list to safely disable Samsung Bloatware](https://www.reddit.com/r/GalaxyS9/comments/iv4p3n/adb_list_to_safely_disable_samsung_bloatware/) and [this list too](https://technastic.com/remove-samsung-bloatware-safe-to-remove-apps/)here is my non exhaustive list of `apks` to remove

```bash
#!/bin/bash

## Remove these
adb shell pm uninstall -k --user 0 net.supertreat.solitaire
adb shell pm uninstall -k --user 0 com.sec.android.app.sbrowser
adb shell pm uninstall -k --user 0 com.samsung.android.app.taskedge
adb shell pm uninstall -k --user 0 com.samsung.android.app.clipboardedge
adb shell pm uninstall -k --user 0 com.samsung.android.game.gametools
adb shell pm uninstall -k --user 0 com.samsung.android.game.gamehome
adb shell pm uninstall -k --user 0 com.samsung.android.scloud
adb shell pm uninstall -k --user 0 com.sec.android.app.samsungapps
adb shell pm uninstall -k --user 0 com.sec.android.widgetapp.samsungapps
adb shell pm uninstall -k --user 0 com.samsung.android.drivelink.stub
adb shell pm uninstall -k --user 0 com.samsung.android.service.peoplestripesamsungpassautofill
adb shell pm uninstall -k --user 0 com.samsung.android.email.provider
adb shell pm uninstall -k --user 0 com.samsung.andorid.fmm
adb shell pm uninstall -k --user 0 com.samsung.android.visionarapps
adb shell pm uninstall -k --user 0 com.mygalaxy
adb shell pm uninstall -k --user 0 com.samsung.android.arzone
adb shell pm uninstall -k --user 0 com.samsung.faceservice
adb shell pm uninstall -k --user 0 com.samsung.android.calendar
adb shell pm uninstall -k --user 0 com.samsung.android.app.reminder
adb shell pm uninstall -k --user 0 com.samsung.android.app.simplesharing
adb shell pm uninstall -k --user 0 com.samsung.android.app.tips
adb shell pm uninstall -k --user 0 com.samsung.android.svoiceime
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.service
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.agent
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.wakeup
adb shell pm uninstall -k --user 0 com.samsung.android.app.spage
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.agent.dummy
adb shell pm uninstall -k --user 0 com.samsung.android.visionintelligence
adb shell pm uninstall -k --user 0 flipboard.boxer.app
adb shell pm uninstall -k --user 0 com.sec.android.app.chromecustomizations
adb shell pm uninstall -k --user 0 com.samsung.android.mdx
adb shell pm uninstall -k --user 0 com.microsoft.appmanager
adb shell pm uninstall -k --user 0 com.microsoft.skydrive
adb shell pm uninstall -k --user 0 com.google.android.apps.googleassistant
adb shell pm uninstall -k --user 0 com.google.android.apps.docs
adb shell pm uninstall -k --user 0 com.google.android.gm
adb shell pm uninstall -k --user 0 com.google.android.googlequicksearchbox
adb shell pm uninstall -k --user 0 com.google.android.youtube
```

## adb commands

```bash
adb shell pm uninstall -k --user 0 <APPFILE.apk>
adb install <APPFILE.apk>
adb shell cmd package list packages

**#1 Get the list of all apps**  
`adb shell pm list packages`  
**#2 Get the list of system apps only**  
`adb shell pm list packages -s`  
**#3 Get the list of all Samsung apps**  
`adb shell pm list packages | grep 'samsung'`
```


## Force allow apps to SD Card

it's a setting in the Debug menu [How to Move Apps to SD Card on Android (How to Force Them) - YouTube](https://www.youtube.com/watch?v=znKGIHRa2SQ&t=246s)

---

## Credits
- 
## index
- [[developer/developer_box📦\|developer_box📦]]