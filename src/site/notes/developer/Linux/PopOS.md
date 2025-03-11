---
{"dg-publish":true,"tags":["linux"],"permalink":"/developer/Linux/PopOS/","dgPassFrontmatter":true}
---

## Sluggish UI and or App Loading
```shell
system76-power graphics nvidia
```

## Wake On LAN
`sudo nano /etc/systemd/system/wol.service`

```shell
[Unit]  
Description=Configure Wake On LAN

[Service]  
Type=oneshot  
ExecStart=/sbin/ethtool -s INTERFACE wol g

[Install]  
WantedBy=basic.target
```

```shell
sudo systemctl daemon-reload  
sudo systemctl enable wol.service  
sudo systemctl start wol.service
```

---
## Credits
- [Wake-on-LAN (WoL) doesn't work · Issue #866 · pop-os/pop (github.com)](https://github.com/pop-os/pop/issues/866)
- [Pop OS 22.04 - extremely slow, laggy and unresponsive, what happened? : r/pop_os (reddit.com)](https://www.reddit.com/r/pop_os/comments/v1th8w/pop_os_2204_extremely_slow_laggy_and_unresponsive/)