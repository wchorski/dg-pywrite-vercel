---
{"dg-publish":true,"tags":["cloud","nextcloud","owncloud","media","assets","management"],"permalink":"/developer/home-lab/nextcloud/","dgPassFrontmatter":true}
---

#cloud #gmail #opensource 

## [Homepage - Nextcloud](https://nextcloud.com/)

> Regain control over your data. Remote collaboration made easy
>-   On-premises or cloud
>-   Customizable and scaleable
>-   No data leaks to third parties

> I dropped **Google Drive** harder than a DJ with a fresh new crate of tracks. I even brought on some friends and family  
> - me

---
### connections
- publicly face app with [[developer/Home Lab/Nginx Proxy Manager#Publicly Facing App\|Nginx Proxy Manager]]
- syncs pictures for [[developer/Home Lab/PhotoPrism\|PhotoPrism]] to read my every growing photo gallery
- used as my calendar server
- backup phone contacts

### installation
1. note that I use an external drive. You must set up your other drive first.
2. [[developer/Home Lab/Docker\|Docker]] `compose.yml`
```yaml
volumes:
  nextcloud:
  db:

services:
  db:
    image: mariadb
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW --innodb-read-only-compressed=OFF
    volumes:
      - /home/<username>/docker/nextcloud/db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=<password09>
      - MYSQL_PASSWORD=<password09>
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud

  app:
    image: nextcloud:25 # USE current version number, increment by one when upgrading
    restart: always
    ports:
      - 8080:80
    links:
      - db
    volumes:
      - /home/<username>/docker/nextcloud/html:/var/www/html
      - /home/<username>/docker/nextcloud/html/apps:/var/www/html/custom_apps 
      - /home/<username>/docker/nextcloud/html/config:/var/www/html/config 
      - /mnt/<externalDrive>/nextcloud/data:/var/www/html/data
    environment:
      - MYSQL_PASSWORD=<password09>
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db
```

## How to Update
> [!error]
> Don't use `nextcloud:latest` as the image.
> UPDATE ONE MAJOR VERSION AT A TIME
> 23 -> 24 👍
> 23 -> 25 🚫

---
## Gmail SMTP server
Nextcloud needs a way to send password resets / notifications / alerts. So I've hooked it up with gmail's SMTP server using an google app password.  

---
## Troubleshooting
### Upgraded 2 versions up on accident 
Turn off Maintenance mode
> via `config/config.php` [source](https://help.nextcloud.com/t/cant-start-nextcloud-because-the-version-of-the-data-is-higher-than-the-docker-image-version-and-downgrading-is-not-supported/109438/4)
> -   go to your nextcloud folder and then open config/config.php
> -   search for `'maintenance' => true,`
> -   change `true` to `false`
> -   save your changes
> -   reload your web page


recover old ./version.php
> 1. restore /version.php from backup into the container  
> 2. run a manual update via 
> 	1. `docker-compose exec --user www-data nextcloud php occ -vvv upgrade` 
> 	2. `docker-compose exec --user www-data nextcloud php occ maintenance:mode --off`.  (or with method above)
> 	   
>  That got me back to a running v19.  
From there I could then pull the v20 docker image and run the upgrade to v20 using the same two commands.
> [source](https://help.nextcloud.com/t/cant-start-nextcloud-because-the-version-of-the-data-is-higher-than-the-docker-image-version-and-downgrading-is-not-supported/109438/4?u=wchorski)

If you don't make backups then you could possibly edit the container's `html/version.php`. Gotta thank [[developer/Home Lab/Duplicati\|Duplicati]] for saving my behind

## Upgrading to AIO (All In One)

Will get to this eventually, but for now I'm sticking with the regular `nextcloud:latest` docker container deployment 
- [Install and Setup Next... | OSiA (opensourceisawesome.com)](https://wiki.opensourceisawesome.com/books/nextcloud-all-in-one/page/install-and-setup-nextcloud-all-in-one-aio)
- [all-in-one/migration.md at main · nextcloud/all-in-one (github.com)](https://github.com/nextcloud/all-in-one/blob/main/migration.md)
- [link](https://github.com/nextcloud/all-in-one/blob/main/reverse-proxy.md#4-open-the-aio-interface)
- [all-in-one/compose.yaml at main · nextcloud/all-in-one (github.com)](https://github.com/nextcloud/all-in-one/blob/main/compose.yaml)

## Fix Security & Setup Warnings
If you've been upgrading Nextcloud from an older version (in my case 23), I'm sure you'll find a list of warnings in the admin dashboard found at `https://YOUR_NEXTCLOUD_DOMAIN/settings/admin/overview`

### config updates
> [!warning] Warning from Nextcloud
> Server has no maintenance window start time configured. This means resource intensive daily background jobs will also be executed during your main usage time. We recommend to set it to a time of low usage, so users are less impacted by the load caused from these heavy tasks. For more details see the [documentation ↗](https://docs.nextcloud.com/server/29/go.php?to=admin-background-jobs).
> 
> Your installation has no default phone region set. This is required to validate phone numbers in the profile settings without a country code. To allow numbers without a country code, please add "default_phone_region" with the respective ISO 3166-1 code of the region to your config file. For more details see the [documentation ↗](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements).

```bash
sudo nano ./nextcloud/html/config/config.php

## Add this to top inside the first `array(`
'maintenance_window_start' => 1, # period between 1am 5am UTC
'default_phone_region' => 1974, # United States code
```

What it will look like
```php
<?php
$CONFIG = array (
  'maintenance_window_start' => 1,
  'default_phone_region' => 1974,
  'htaccess.RewriteBase' => '/',
  'memcache.local' => '\\OC\\Memcache\\APCu',
  'apps_paths' => 
  array (
    0 => 
    array (
...
```
### Mimetype Repair
> [!warning] Warning from Nextcloud
> One or more mimetype migrations are available. Occasionally new mimetypes are added to better handle certain file types. Migrating the mimetypes take a long time on larger instances so this is not done automatically during upgrades. Use the command `occ maintenance:repair --include-expensive` to perform the migrations.

This is how I ran it with `nextcloud-app-1` being the exact name of my container
```bash
docker exec -it -u 33 nextcloud-app-1 /var/www/html/occ  maintenance:repair --include-expensive
```
### fix missing db indexes
> [!warning] Warning from Nextcloud
> The database is missing some indexes. Due to the fact that adding indexes on big tables could take some time they were not added automatically. By running "occ db:add-missing-indices" those missing indexes could be added manually while the instance keeps running. Once the indexes are added queries to those tables are usually much faster. (long list of missing stuff)

```bash
docker exec -it -u 33 nextcloud-app-1 /var/www/html/occ db:add-missing-indices
```
### enhance memcache file locking
> [!warning] Nextcloud
> The database is used for transactional file locking. To enhance performance, please configure memcache, if available. For more details see the [documentation ↗](https://docs.nextcloud.com/server/29/go.php?to=admin-transactional-locking).

This looks like this is for servers with heavy multi user activity. I'll get back to this later as I could see some benefit to setting this up anyway for myself

To use a memcache with Transactional File Locking, you must **install the Redis server** and corresponding PHP module. 

If you want to configure **Redis** to listen on an Unix socket (which is recommended if Redis is running on the same system as Nextcloud) use this example `config.php` configuration:
```php
'memcache.locking' => '\OC\Memcache\Redis',
'redis' => array(
     'host' => '/var/run/redis/redis.sock',
     'port' => 0,
     'timeout' => 0.0,
     'password' => '', // recommended!, if not defined no password will be used.
	),
```
## Rescan files
If you edited any files outside of Nextcloud UI (possibly in the terminal), you will need to rescan files to update Nextcloud's database
```bash
docker exec -it -u 33 nextcloud-app-1 /var/www/html/occ  files:scan --all
docker exec -it -u 33 nextcloud-app-1 /var/www/html/occ  files:cleanup
```

```bash
## Go to the nextcloud folder (Ex. /var/www/nextcloud) where the **occ** exists.

docker exec -it -u 33 nextcloud-app-1 /var/www/html/occ maintenance:mode --on

docker exec -it -u 33 nextcloud-app-1 redis-cli --askpass -s /var/run/redis/redis.sock flushall
Password can be found in nextcloud_folder/config/config.php

docker exec -it -u 33 nextcloud-app-1 /var/www/html/occ maintenance:mode --off

docker exec -it -u 33 nextcloud-app-1 /var/www/html/occ files:scan --all

## This scans for all files within the specific user
docker exec -it -u 33 nextcloud-app-1 /var/www/html/occ files:scan-app-data

## This scans for all the shared folder or files that has been shared
```

### Flush File Lock Cache in SQL
```bash
docker exec nextcloud-db-1 mysql nextcloud -p$MYSQL_ROOT_PASSWORD -e "SELECT * FROM oc_file_locks WHERE \`lock\`<>0;"
```
### Fix Lock Error
If you've been fiddling with files in the terminal, permissions may get jacked up. In the below example we are making sure all files inside `nextcloud` data repo are owned by `www-data`. You can narrow this scope to any folders or files inside this directory to save time especially if you have a lot of files. 

```shell
sudo chown -R www-data:www-data nextcloud
```
## Other Troubleshooting
- [[developer/Nextcloud/Nextcloud Block Issues and Large Log Files\|Nextcloud Block Issues and Large Log Files]]

#todo 
- [x] man description
- [x] my desc
- [x] working on contact syncing
- [ ] try out nextcloud-aio https://github.com/nextcloud/all-in-one/blob/main/migration.md

---
## Credits
- [File is locked - how to unlock - 📑 How to - Nextcloud community](https://help.nextcloud.com/t/file-is-locked-how-to-unlock/1883/94)