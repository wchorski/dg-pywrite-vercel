---
{"dg-publish":true,"permalink":"/developer/Docker🐳/Force Rebuild New Image/","dgPassFrontmatter":true}
---

Instead of just building your images, and then trying to up them, consider this:

```
$ docker-compose down
$ docker-compose build
$ docker-compose up
```

Which can be shortened to:

```
$ docker-compose build
$ docker-compose up --force-recreate
```

This will make sure that even ‘unchanged’ containers are stopped and recreated in any case.

Maybe your issues are happening during the image build. If there’s a caching issue with your Dockerfile, you can rebuild it from scratch every single time, and then recreate all containers with the following two commands:

```
$ docker-compose build --no-cache
$ docker-compose up --force-recreate
$ docker compose up --build # this does it all
```

This will rebuild all images from scratch. If you want to be more selective, you could use:

```
$ docker build --no-cache -t imagename .
```

And recreate the containers with docker-compose afterwards.

All of those are meant to be **temporary** fixes! You should not have to rely on them in the long term.

## The Way Forward

You should dig into your Dockerfile and setup, to find out why you are having these issues! If you don’t your workflows will be unnecessarily slow and otherwise not reliable.

If you are experiencing similar issues with your deployments, you should switch towards more meaningful tags than [:latest](https://vsupalov.com/docker-latest-tag/), and rethink the way you’re pushing out new versions of your applications.

---
## Citations
- [link](https://vsupalov.com/docker-compose-runs-old-containers/#:~:text=The most probable explanation is%2C that you are,container or your build used old cached layers.)