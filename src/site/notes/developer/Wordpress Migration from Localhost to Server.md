---
{"dg-publish":true,"permalink":"/developer/wordpress-migration-from-localhost-to-server/","dgPassFrontmatter":true}
---


I was looking to build out a [[developer/Home Lab/Wordpress\|Wordpress]] sight locally and deploy the SQL + Wordpress files onto a remote server. There is a few steps involved to make sure login, links, and db all play nicely together after the transfer

## Local Prep
1. bundle wordpress files into a single zip folder. contents will look something like this `wp-mynewsight-local.zip`
```shell
wp-admin
wp-content
wp-includes
.htaccess
index.php
...
```
2. Delete `wp-config.php`
4. visit the local build's **PHPMyAdmin** app 
5. `export` the database
	1. method = `quick`
	2. format = `SQL`
	3. file will download as `mynewsight-local_db.sql`


## Transfer to Server

I'm deploying this sight onto [Bluehost.com](https://www.bluehost.com/) so I had to create a fresh sight first, then transfer data onto it 

### upload Wordpress files
8. Using **CPanel**, access the **File Manager** 
9. navigate to the server's new wordpress files. Example: `public_html/mynewsight-com`
10. upload the `wp-mynewsight-local.zip`, unzip, and replace any matching file names
~~11. edit `wp-config.php`
	1. `define('DB_NAME', 'mynewsight_db');`
	1. `define('DB_NAME', 'mynewsight_user');`
	1. `define('DB_NAME', 'super_strong_password');`~~
12. edit `wp-config.php`
13. take note of the `Table Prefix` → example: `S4B_`
13. change the table prefix to match your locally developed SQL file `Table Prefix` → example: `wp_`

### edit database
1. Login to Server's **CPanel**
2. Launch **MySQL® Databases**
~~	1. create new database → `mynewsight_db`
	2. create new user → `mynewsight_user` (with strong password)
	2. add new user to new database with all previleges~~
1. Look inside to find the database with the matching table prefixes →  `S4B_`
2. ~~add new user with same password from the *local build's db*. With all permissions enabled (my case `admin_mynewsight`)~~
3. click the database and import the local db file `db_mynewsight-local.sql`
4. edit `wp_options`
	1. siteurl = `https://mynewsight.com`
	1. home = `https://mynewsight.com`
5. you can delete the old tables prefixed with `S4B_`. I just let them be
6. visit your sight and login `https://mynewsight.com/wp-admin`
7. fix all internal permalinks links via `https://mynewsight.com/wp-admin/options-permalink.php` and hit the <button>Save Changes </button> button
8. confirm `Search Engine Visibility` is set to off via `https://mynewsight.com/wp-admin/options-reading.php`


---
## Credits
- [How To Move Your WordPress Site from Localhost to Live Server Manually 2023 | Step by Step - YouTube](https://www.youtube.com/watch?v=Bn7fRQ87C-8)
- [How Do I Transfer My WordPress Website To Bluehost?](https://www.bluehost.com/blog/faq-how-do-i-move-my-wordpress-website-to-bluehost/)