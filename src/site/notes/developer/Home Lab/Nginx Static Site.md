---
{"dg-publish":true,"permalink":"/developer/home-lab/nginx-static-site/","dgPassFrontmatter":true}
---

Put up a simple static site that can serve static files like pictures, videos, pdfs or write html, css, js for the most purist web developers.

### Bonus 
I mount a couple [[developer/Home Lab/Nextcloud\|Nextcloud]] volumes so that I can easily sync or drag and drop files that will be instantly available. 

```yml
services:

  nginx:
    image: nginx:latest
    restart: unless-stopped
    volumes:
      ## only used as the splash page
      - ./html:/usr/share/nginx/html
      ## must add line for each asset repo that will live in nextcloud sub folder
      - /mnt/uasis5/nextcloud/data/admin_user/files/webdev/_static-assets/USERNAME_1:/usr/share/nginx/html/USERNAME_1:ro
      - /mnt/uasis5/nextcloud/data/admin_user/files/webdev/_static-assets/USERNAME_2:/usr/share/nginx/html/USERNAME_2:ro
    networks:
      # - this
      - proxy
    # ports:
    #  - "8888:80"
    # environment:
    #  - NGINX_HOST=assets.MYWEB.site
    #  - NGINX_PORT=80
    
networks:
  # this:
  #   name: MYWEB-network
  #   driver: bridge
  proxy:  # TODO turn back on when using proxy
    name: nginx-prox-mgmt_default # whatever proxy container network
    driver: bridge
    external: true   
```