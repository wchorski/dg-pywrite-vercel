---
{"dg-publish":true,"permalink":"/developer/linux/crontab/","dgPassFrontmatter":true}
---

run scripts on a regular schedule

![note] if you edit the 

```shell
# current user
crontab -e

# root user
sudo crontab 
```


```bash
# 3am on 4th day of month every 2nd (other) month

0 3 4 */2 * 

```
## tools
- [Crontab.guru - The cron schedule expression editor](https://crontab.guru/)

---
## Credits 
- 

## Backlinks
- [[developer/Linux/Linux\|Linux]]