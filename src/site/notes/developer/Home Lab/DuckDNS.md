---
{"dg-publish":true,"permalink":"/developer/Home Lab/DuckDNS/","dgPassFrontmatter":true}
---

[Duck DNS](http://www.duckdns.org/)

> free dynamic DNS hosted on AWS

Dynamic DNS. The benefits of a static IP, without shelling out 💲💲💲 for one. Updates our duckdns domain when our home  server's Public IP changes

### Setup

Before you start the setup tutorial, ensure that you have navigated to [https://www.duckdns.org](https://www.duckdns.org/) and setup a user account. We will be using the domain and token that exist on that page

```shell
mkdir duckdns  
cd duckdns  
nano duck.sh
```

`duck.sh`
```shell
echo url="https://www.duckdns.org/update?domains=<YOUR_DOMAIN>&token=<YOUR_TOKEN>=" | curl -k -o ~/duckdns/duck.log -K -
```

```shell
chmod 700 duck.sh  
sudo chown $USER:$USER duckdns/duck.sh
sudo chown $USER:$USER duckdns/duck.log
crontab -e
```

`crontab`
```shell
*/5 * * * * ~/duckdns/duck.sh >/dev/null 2>&1
```

```shell
./duck.sh  
cat duck.log  
sudo service cron start
```

---
## citations
- [How to Setup DuckDNS on a Raspberry Pi! - WunderTech](https://www.wundertech.net/how-to-setup-duckdns-on-a-raspberry-pi/)