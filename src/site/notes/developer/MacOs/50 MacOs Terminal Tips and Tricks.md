---
{"dg-publish":true,"permalink":"/developer/MacOs/50 MacOs Terminal Tips and Tricks/","dgPassFrontmatter":true}
---

## find password of previously connected wifi
```shell
security find-generic-password -wa "coffee-shop-wifi"
```

## keep mac awake
```
caffeinate
```

## save screenshot to clipboard
```bash
cmd ctrl shift 4 
```

## use finger print id for sudo password
```
sudo nano /etc/pam.d/sudo

# add
auth sufficient pam_tid.so
```

---
## Credits
- [the last one is CRAZY!) - YouTube](https://www.youtube.com/watch?v=qOrlYzqXPa8)
- [[developer/MacOs/index\|index]]