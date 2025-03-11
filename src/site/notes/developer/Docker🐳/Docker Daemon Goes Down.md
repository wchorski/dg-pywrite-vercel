---
{"dg-publish":true,"tags":["docker","docker-compose"],"permalink":"/developer/Docker🐳/Docker Daemon Goes Down/","dgPassFrontmatter":true}
---

> Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.

I removed the file `/etc/docker/daemon.json` and started it with sudo systemctl start docker and it worked!!
## Credits
- [Failed to start Docker Application Container Engine - Stack Overflow](https://stackoverflow.com/questions/49110092/failed-to-start-docker-application-container-engine)