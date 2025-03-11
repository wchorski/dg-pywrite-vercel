---
{"dg-publish":true,"tags":["linux","network","DNS"],"permalink":"/developer/Linux/Raspberry hostname mismatch/","dgPassFrontmatter":true}
---


```shell
cat /etc/hostname
cat /etc/hosts
```

Look for any differences. In my case the last line in `/etc/hosts` was `127.0.1.1       mypc-name` and I needed to change it to match the output from `/etc/hostname`

---
## Credit
- https://forums.raspberrypi.com/viewtopic.php?t=276036