---
{"dg-publish":true,"permalink":"/developer/home-lab/docker/","dgPassFrontmatter":true}
---

## [Home - Docker](https://www.docker.com/)
fast, easy and portable application development through containerization 

If I haven't figured out a way to [containerize](https://www.docker.com/resources/what-container/)) an app, I will find a way. Pretty much the backbone of how I deploy and maintain many services.    

---

### connections
- [[developer/Home Lab/portainer\|portainer]]
- [[developer/Home Lab/Nginx Proxy Manager\|Nginx Proxy Manager]]
- [[developer/Home Lab/Nextcloud\|Nextcloud]]
- [[developer/Home Lab/PhotoPrism\|PhotoPrism]]
- [[developer/Home Lab/Wordpress\|Wordpress]]
- [[developer/NextJS/NextJS\|NextJS]]
- [[developer/ReactJS/ReactJS\|ReactJS]]
- [[developer/Home Lab/vaultwarden\|vaultwarden]]
- [[developer/Home Lab/Jellyfin\|Jellyfin]]
- [[developer/Home Lab/Plex.tv\|Plex.tv]]
- pretty much any of my microservices. Especially if it's public facing 

### installation
1. [Install Docker Desktop on Linux](https://docs.docker.com/desktop/install/linux-install/)

---
## Tips & Tricks
specifiy a file other than `docker-compose.yml` or `compose.yml`
```bash
docker-compose -f docker-compose.test.yml up
```

### execute a command
```shell
sudo docker exec -it dev-garden-app-1 ls /public # '/public = whatever directory you want to look into'

sudo docker exec -it dev-garden-app-1 /bin/bash # 'heard you like shell inside your shell'
```

### Map Volume on Windows 10
- [link](https://stackoverflow.com/questions/50959475/docker-compose-on-windows-volume-not-working#:~:text=To do so%3A 1 Run the command "set,container rm -f ) 5 Re-run the containers)
```shell
1.  Launch Docker from your windows taskbar
2.  Click on Settings icon on top
3.  Click Resources
4.  Click File Sharing
5.  Click on (+) sign and add path of local folder in which you want to map the container volume.

It worked for me.
```

````yaml
"C:\\ProgramData\\Docker\\volumes\\sqldata:c:\\mssql"
````

## macOS app not responding upon launch
1.  Go to finder and open `Applications > Utilities > Activity Monitor.app`
    
2.  Find `Docker` or `Docker Desktop` (it was in Not responding mode in my case) and Force Kill the app.
    
3.  Run the following command in Terminal:
    
    `sudo rm -rf ~/Library/Containers/com.docker.*`
    
4.  Open the setting file in the terminal:
    
    `sudo nano ~/Library/Group\ Containers/group.com.docker/settings.json`
    

then find `"filesharingDirectories":`

5.  Remove all entries inside this key and leave it as `"filesharingDirectories": [],` save and close the file.
6.  Finally go to the Application folder, find `Docker` and run it (wait sometime, it takes a minute or so to run)
7.  It will run and work like a charm!

## Run Docker Compose in Crontab
running `docker compose up -d` is the norm when you're already in the same directory as the `compose.yml` file, but what if you want to call that same file from a `crontab`

```bash
# spin up container from selected file
docker compose -f /home/mypath/compose.yml up -d 
docker compose -f /mnt/uasis5/node/filesystem-livesync-pywriter4/compose.yml up -d

# restart container 'server' named in compose.yml file
docker compose -f /mnt/uasis5/node/filesystem-livesync-pywriter4/compose.yml restart server
```

#todo 
- [link](https://www.jamescoyle.net/how-to/docker-compose-files/3323-docker-compose-file-for-watchtower#:~:text=Create a new directory and save the above,from the moment you start the Watchtower container.)

---
## Credits
- [cannot launch docker desktop for mac - Stack Overflow](https://stackoverflow.com/questions/69552636/cannot-launch-docker-desktop-for-mac)