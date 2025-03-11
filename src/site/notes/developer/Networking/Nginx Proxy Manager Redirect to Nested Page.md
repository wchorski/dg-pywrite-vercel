---
{"dg-publish":true,"permalink":"/developer/Networking/Nginx Proxy Manager Redirect to Nested Page/","dgPassFrontmatter":true}
---

I wanted to create an 'under construction' homepage for my personal site. I'm already running a site service with [[developer/Home Lab/Nginx Static Site\|Nginx Static Site]] serves public assets (pictures, videos). I will refer to this as my **Static Asset Site**

Instead of spinning up a separate **nginx** server just to write a single html file for my 'under construction' message, I'm able to leverage [[developer/Home Lab/Nginx Proxy Manager\|Nginx Proxy Manager]] to point to a nested directory
### Senario
Here my site is already setup with a proxy host `assets.MYWEB.site`. I've created a temporary homepage nested in `assets.MYWEB.site/pages/tmp-homepage/index.html`.

I want to take my current personal site `www.MYWEB.site` and point it to that `tmp-homepage/index.html`

### Nginx Config

Edit the proxy host for `www.MYWEB.site` and go to the 

**Advanced Tab**
```conf
location = / {
    return 301 $forward_scheme://$host/pages/tmp-homepage;
}
```

- `$forward_schema` auto decides between http or https
- `$host` takes the current domain the browser entered
### Uses
I could also see this being useful for site wide redirects to reroute old and stale links. Let's say you setup your e-commerce shop to point to `/products` but later you change all your purchasable items to page `/shop`. You can do that with this method. Here is that example

```conf
location = /products {
    return 301 $forward_scheme://$host/shop;
}
```

---
## Credit
- https://stackoverflow.com/a/41349192/15579591