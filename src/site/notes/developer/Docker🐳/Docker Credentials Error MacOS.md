---
{"dg-publish":true,"tags":["MacOs","docker","error","troubleshooting"],"permalink":"/developer/Docker🐳/Docker Credentials Error MacOS/","dgPassFrontmatter":true}
---

## The Error
```bash
docker compose build

## permission denied
```

```shell
sudo docker compose build

## This error happened
=> ERROR [backend internal] load metadata for docker.io/library/node:20-  0.1s
------
 > [backend internal] load metadata for docker.io/library/node:20-alpine:
------
failed to solve: node:20-alpine: failed to resolve source metadata for docker.io/library/node:20-alpine: error getting credentials - err: exit status 1, out: ``
```
## The Fix
```shell
sudo chown -R $(whoami) ~/.docker
```
---
## Credit
- [macos - open /Users/[user]/.docker/buildx/current: permission denied on macbook? - Stack Overflow](https://stackoverflow.com/questions/75686903/open-users-user-docker-buildx-current-permission-denied-on-macbook)