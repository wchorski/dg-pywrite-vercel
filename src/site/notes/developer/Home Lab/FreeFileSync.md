---
{"dg-publish":true,"permalink":"/developer/home-lab/free-file-sync/","dgPassFrontmatter":true}
---

#backup #cross_platform

> [!info] [FreeFileSync](https://freefilesync.org/)
> Open Source, Cross Platform File Synchronization & Backup Software

when I need to make a **One to One** copy of a directory, internally or to another machine via a [[developer/Home Lab/SAMBA\|SAMBA]] share. I originally used this exclusively until I found [[developer/Home Lab/Duplicati\|Duplicati]]

> check out how I sync my iTunes playlists with my [[developer/Home Lab/Jellyfin\|Jellyfin]] server

## setup
- run the sync script manually once you point directories
- you can either fire off the sync script via a **Windows Schedule** or **Bash Script** 
- use their [RealTimeSync](https://freefilesync.org/manual.php?topic=realtimesync) version if you need something that **watches** a directory 

### connections
- iTunes
- [[developer/Home Lab/Jellyfin\|Jellyfin]]

### installation

---
## backlinks
[[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]]