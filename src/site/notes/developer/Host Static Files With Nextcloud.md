---
{"dg-publish":true,"tags":["media","assets","images","files","public","selfhosted","docker"],"permalink":"/developer/host-static-files-with-nextcloud/","dgPassFrontmatter":true}
---

Between self-hosting a [[developer/Home Lab/Nextcloud\|Nextcloud]] instance and creating website, I wanted there to be a way for my **Nextcloud** users to upload and manage public files to share and or add to their site. 
## Built in Apps
First I tried the **Nextcloud** app [Pico CMS](https://apps.nextcloud.com/apps/cms_pico). Essentially it's a static site host directly inside your cloud. At first it worked as I needed it to. Setting up each client with their own site, and loading there files into an `/assets` folder nested inside their account. 

After a few updates from **Nextcloud** it seemed that the Pico app was left in the dust. UI seemed to bug out, configurations wouldn't load, sites would go down. Setting up a site was also a bit of a chore. Also the urls were long and ugly.
## Nginx Container
This method assumes a bit more from your IT skills. Here I already have set up 
- [[developer/Home Lab/Nginx Proxy Manager\|Nginx Proxy Manager]]
- DNS provider domain `assets.tawtaw.site`

Setting up a Nginx server seems like the easiest in terms of setup, simplicity, and longevity.  

```yml
services:

  nginx:
    image: nginx:latest
    restart: unless-stopped
    volumes:
	  # where we will drop the `index.html` 
      - ./html:/usr/share/nginx/html
      # path to your client's Nexcloud files
      - /mnt/STORAGE_DRIVE/nextcloud/data/NEXTCLOUD_USERNAME/files/PUBLIC_SITE_FILES:/usr/share/nginx/html/CUSTOMER_A:ro
    networks:
      # - this
      - proxy
    # ports:
    #  - "8888:80"
    # environment:
    #  - NGINX_HOST=assets.tawtaw.site
    #  - NGINX_PORT=80
    
networks:
  # this:
  #   name: assets-network
  #   driver: bridge
  proxy:  # TODO turn back on when using proxy
    name: nginx-prox-mgmt-3_default # whatever proxy container network
    driver: bridge
    external: true
```

`PUBLIC_SITE_FILES` is an arbitrary name as once we pass `/files` that will be the files accessed and modified by the user. At least with this one "public" subfolder, the user will understand anything outside or above that folder will be private. 

I recommend putting a `Readme.md` in this `PUBLIC_SITE_FILES` folder warning the user 

> [!warning] This folder is **PUBLIC.** All files put in this folder will be publicly available for anyone around the world to view.
## index.html
The `index.html` isn't necessary, but I think it's a good idea to at least make a little splash page incase someone lands here. I simply explain my site as an "Asset Site" and a few links depending on if your a current customer or want to become one

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>There's a Will There's a Website Static Assets</title>
  <style>
    body {
      font-family: Verdana, Geneva, Tahoma,sans-serif;
      background-color: #384043;
      color: white;
    }
    a {
      color: #8cc0a7;
    }
    p {
      max-width: 60ch;
    }
  </style>
</head>
<body>
  <h1> TAWTAW: Static Assets </h1>
  <p>
    This site delivers static assets uploaded from your <a href="https://cloutdrive.tawtaw.site/login">CloutDrive Account</a>. 
    If you don't have a CloutDrive account, sign up today at 
    <a href="https://www.tawtaw.site">www.tawtaw.site</a>
  </p>
</body>
</html>
```

## Access the Files
This setup between Nextcloud, Nginx, and my Proxy is flexible and fast and it gives my clients the nice and easy to use Nextcloud UI (with local desktop sync)

Files now can be access by a simple url example: 

```shell
https://assets.tawtaw.site/CUSTOMER_A/cute_puppy.jpg
```

If the customer adds subfolders for organization, the site will follow the same path

```shell
https://assets.tawtaw.site/CUSTOMER_A/dogs/puppies/cute_puppy.jpg
```