---
{"dg-publish":true,"tags":["nextcloud","logs","data","recovery"],"permalink":"/developer/Nextcloud/Nextcloud Block Issues and Large Log Files/","dgPassFrontmatter":true}
---

I had the external drive (a externally powered external HDD. I know... but it's cheap and it works) unmount itself a few times in one week. This worried me that the drive may be reaching end of life, but I had another suspicion something was going on.

I immediately started to move files over to a new drive via `rsync` and noticed it was getting hung up one one file `nextcloud.log` 
## Find Large Files
By checking Nextcloud's mounted data drive via `ls -lh /mnt/MYDRIVE/nextcloud/data` I could see the offending files

```
total 84K  
drwxr-xr-x 3 www-data www-data 4.0K Nov 30 2021 admin  
drwxr-xr-x 16 www-data www-data 4.0K Feb 6 2024 appdata_ocfj7vpipn7t  
drwxr-xr-x 10 www-data www-data 4.0K Nov 20 2021 appdata_ocg8qrefwjdq  
drwxr-xr-x 6 www-data www-data 4.0K Nov 30 2021 appdata_ocxyq0emigvu  
drwxr-xr-x 9 www-data www-data 4.0K Aug 8 2023 myuser  
drwxr-xr-x 2 www-data www-data 4.0K Jul 8 2024 files_external  
-rw-r----- 1 www-data www-data 56GB Jan 22 17:42 flow.log  
drwxr-xr-x 5 www-data www-data 4.0K Nov 21 2021 __groupfolders  
-rw-r--r-- 1 www-data www-data 0 Jan 15 10:45 index.html  
-rw-r----- 1 www-data www-data 511GB Jan 22 18:51 nextcloud.log  
```

`nextcloud.log` file was over 500gb. `flow.log` was over 50gb. [Nextcloud Logging](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/logging_configuration.html)

> [!note] Worry Not
> If your worried about deleting things willy nilly it's [ok](https://help.nextcloud.com/t/delet-log-file-in-nextcloud-13-0-5/36458)

## Clearing the File
If your using bash here is a cool script to set the whole file to a blank string

```bash
echo "" | sudo tee /mnt/MYDRIVE/nextcloud/data/nextcloud.log > /dev/null
```
## Logging Settings
The reason why these files ballooned in size was because of my Logging levels. I had `Debug` & `Info` ticked for some reason. you can configuring them at this path on your server

https://NEXTCLOUD_DOMAIN/settings/admin/logging


---
## Credit
- https://stackoverflow.com/a/550808
- https://www.reddit.com/r/NextCloud/comments/k7fx4m/am_i_safe_to_remove_my_nextcloudlog/