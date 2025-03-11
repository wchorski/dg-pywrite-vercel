---
{"dg-publish":true,"permalink":"/developer/home-lab/glances/","dgPassFrontmatter":true}
---


> [!info](https://nicolargo.github.io/glances/)
> Glances is a cross-platform system monitoring tool written in Python.

### connections
- [[developer/Home Lab/Home Assistant\|Home Assistant]]

### installation on Linux
```shell
sudo apt-get install glances
```

## as a System Service
```shell
nano /etc/systemd/system/glancesweb.service
```

`glancesweb.service` → change `User` to your current user 
```shell
[Unit]
Description = Glances in Web Server Mode
After = network.target

[Service]
User = spearmint
ExecStart = /usr/bin/glances  -w  -t  5

[Install]
WantedBy = multi-user.target
```

```shell
[Unit]
Description = Glances in Web Server Mode
After = network.target

[Service]
User = pi4
ExecStart = /usr/local/bin/glances  -w  -t  5

[Install]
WantedBy = multi-user.target
```

enable and test 
```shell
sudo systemctl enable glancesweb.service
sudo systemctl start glancesweb.service
sudo systemctl status glancesweb.service
```

### installation on Windows
install `.exe` at [Download Python | Python.org](https://www.python.org/getit/)
run `pip install glances` & `pip install bottle`

### Home Assistant Addons Glances to Influxdb
- [InfluxDB, Grafana, Glances & IDE in Home Assistant within 15 minutes! - YouTube](https://www.youtube.com/watch?v=m9qIqq104as&t=2s)


## helpful links
[How to Use Glances to Monitor System On Ubuntu (maketecheasier.com)](https://www.maketecheasier.com/glances-monitor-system-ubuntu/)
[link](https://www.tecmint.com/glances-monitor-remote-linux-in-web-server-mode/#:~:text=Run Glances in Web Server Mode as a,start and view its status as follows. )

## Webpage Blank White
The Debian apt package breaks the webpage ([github issue](https://github.com/nicolargo/glances/issues/2021) tells a tale ofminified #js and open source being obfuscated)

Here is a one-liner code that 
1. that reads the current apt glances version
2. grabs the corresponding source code from github
3. copies to local library files
```bash
sudo sh -c 'wget -O - https://github.com/nicolargo/glances/archive/refs/tags/v$(glances -V|cut -zd" " -f2|tr -d v).tar.gz | tar -xz -C /usr/lib/python3/dist-packages/glances/outputs/static/ --strip-components=4 --wildcards glances-*/glances/outputs/static/public/'
```

---
## Credits