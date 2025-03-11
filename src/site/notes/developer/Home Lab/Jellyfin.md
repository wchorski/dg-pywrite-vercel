---
{"dg-publish":true,"permalink":"/developer/Home Lab/Jellyfin/","dgPassFrontmatter":true}
---

#music #media #player #itunes #bash #powershell

![banner-dark (jellyfin.org)](https://jellyfin.org/images/logo.svg) 
> [!info] [Jellyfin](https://jellyfin.org/) The Free Software Media System.
> Jellyfin is the volunteer-built media solution that puts _you_ in control of your media. Stream to any device from your own server, with no strings attached. Your media, your server, your way.

I use this exclusively for music hosting because [[developer/Home Lab/Plex.tv\|Plex.tv]] has more restrictions on client music playback. Plus, with a little shell magic -> [[developer/Home Lab/Jellyfin#iTunes playlist Sync\|iTunes playlist Sync]]

---

### Connections
- [[developer/Home Lab/Home Assistant\|Home Assistant]]
- Android Phone (Galaxy s9)

## installation
1. `./compose.yml` via [[developer/Home Lab/Docker\|Docker]]
```yaml
version: "3.7"

services:

  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    restart: unless-stopped
    ports:
      - 8096:8096
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
    volumes:
      - ./config:/config
      - ./cache:/cache
      - /mnt/<externalDrive>/jellyfin/media:/media:ro #:ro = read-only

# networks:
#   default:
#     external:
#       name: nginx-prox-mgmt-3_default
```

## Phone Players
Gotta be honest, the native [Jellyfin | F-Droid](https://f-droid.org/en/packages/org.jellyfin.mobile/) app is very lack luster. 🫰that this will improve in the future. Here is 2 alternatives

1. [Symfonium](https://www.symfonium.app/): It cost **$5**, and that's fine because it is very feature rich (Android Auto, auto sync, Offline downloads, etc). 
2. [FinAmp](https://github.com/jmshrv/finamp)If you're looking for something light weight (and [[developer/FOSS\|FOSS]])

## iTunes playlist Sync
- copy files to server: [[developer/Home Lab/FreeFileSync\|FreeFileSync]] or [[developer/Linux/rsync\|rsync]]
- [[developer/Media Software/iTunes XML Playlist to m3u Converter\|iTunes XML Playlist to m3u Converter]]
- [[developer/MacOs/Apple Music Batch Playlist Export\|Apple Music Batch Playlist Export]]


---
- [[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]]
- [[music/music box\|music box]]
