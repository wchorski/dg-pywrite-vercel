---
{"dg-publish":true,"permalink":"/developer/mac-os/mac-os-relaunch-a-crashing-app/","dgPassFrontmatter":true}
---

The #microsoft Outlook app on my #MacOs constantly crashed. I would get no warning that this would happen, meaning minutes would go by while I sit wistfully unaware of the emails hitting my inbox.

I've tried troubleshooting the app itself, so there must be a way on the **OS** side to keep this app alive, or at least bring it back to life after it has a panic attack

## Create a Launch File

Name it `SomeApp.restart.plist` and place it in `~/Library/LaunchAgents/`

```shell
nano ~/Library/LaunchAgents/Outlook.restart.plist
```

Add contents to the new file
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>RunAtLoad</key>  
        <true/>  
        <key>KeepAlive</key>
        <true/>
        <key>Label</key>
        <string>SomeApp.restart</string>
        <key>ProgramArguments</key>
        <array>
                <string>/Applications/Microsoft Outlook.app/Contents/MacOS/Microsoft Outlook</string>
        </array>
</dict>
</plist>
```

Launch the app via

```shell
launchctl load ~/Library/LaunchAgents/Outlook.restart.plist
```

---
## Credit
- [launchd - Automatically relaunch a closed macOS application - Ask Different (stackexchange.com)](https://apple.stackexchange.com/questions/837/automatically-relaunch-a-closed-macos-application)