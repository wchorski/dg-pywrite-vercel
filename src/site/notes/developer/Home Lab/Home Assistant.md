---
{"dg-publish":true,"permalink":"/developer/Home Lab/Home Assistant/","dgPassFrontmatter":true}
---

[Home Assistant (home-assistant.io)](https://www.home-assistant.io/)

> Open source home automation that puts local control and privacy first. Powered by a worldwide community of tinkerers and DIY enthusiasts.

installed on [Raspberry Pi 4 Model B  4GB](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/) with Debian 11 64bit

---

### connections
- securely accessed via [[developer/Home Lab/PiVPN\|PiVPN]] (I don't forward face any apps that I solo use)
- [[developer/Home Lab/Hyperion\|Hyperion]]
- [[developer/Home Lab/WLED\|WLED]]
- [[developer/Home Lab/Divoom Pixoo\|Divoom Pixoo]] digital sign (via bluetooth)
- [[developer/Home Lab/Glances\|Glances]] - via Grafana & InfluxDB

### installation 

While I would first recommend installation via [ Linux - Home Assistant ](https://www.home-assistant.io/installation/linux#install-home-assistant-supervised), I'm also trying out the containered version for it's flexibility and portability
### compose.yml
```yml
version: '3'
services:
  homeassistant:
    container_name: homeassistant
    image: "ghcr.io/home-assistant/home-assistant:stable"
    volumes:
      - ./config:/config
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped
    privileged: true
    network_mode: host

```


### plugins
1. ESPHome
2. Grafana
3. InfluxDB
4. motionEye
5. Node-RED
6. Rhasspy Assistant
7. Samba Backup
8. Samba Share
9. VSCode
10. HACS

---

*Edit* - I end up going with **Hass.io** install instead, but leaving the **Supervised** notes here for reference
## troubleshooting 
> [!warning]
> I'm running the [home-assistant/supervised](https://github.com/home-assistant/supervised-installer) installation because I want to utilize my [[developer/Hardware/Pi4\|Pi4]] for multiple apps.
> 
> I encountered a boot loop error from one of the Home Assistant services, effectively freezing any other service running on my [[developer/Hardware/Pi4\|Pi4]]
> 

```shell
... entered blocking state
... entered forwarding state
... entered disabled state
```

I don't update my Home Assistant very often so I thought why not reinstall it. The [reinstall](https://github.com/home-assistant/supervised-installer)  brought my **Home Assistant** back to life 😅. 

### POE+ Hat
the fan gave off a nasty whine, and I knew I could remedy it by tweaking the temp thresholds. But "Home Assistant OS" doesn't have a simple `/boot/config.txt` path. So I took out the SD card and popped it into my **Windows 10 Machine**

There was a file right in the root called `config.txt` and I put these lines right at the bottom

```
# PoE+ FAN TEMPERATURE CONTROL  
dtoverlay=rpi-poe-plus  
dtparam=poe_fan_temp0=45000,dtparam=poe_fan_temp0_hyst=3000  
dtparam=poe_fan_temp1=50000,dtparam=poe_fan_temp1_hyst=3000  
dtparam=poe_fan_temp2=62000,dtparam=poe_fan_temp2_hyst=4000  
dtparam=poe_fan_temp3=70000,dtparam=poe_fan_temp3_hyst=5000_
```

## finally moving to Hassio.os
I got a warning that looked this this 

>[!warning] Unsupported system - CGroup version
>System is unsupported because the wrong version of Docker CGroup is in use. Use the link to learn the correct version and how to fix this.

after a lot of effort went into fixing and updating that I've finally got a dedicated Raspberry Pi 4b 4gb to serve as my baked in "hassio.os" 

## Press Start on Plugin does nothing
I migrated my Home Assistant config onto a new machine, when I viewed my plugins I saw everything was there, yet when I hit the `start` button, it would spin for a bit and then... nothing.

All I had to do was restart my whole Home Assistant 

## system monitor
```yml
# Example configuration.yaml entry with all entry types (delete/comment out as necessary)
sensor:
  - platform: systemmonitor
    resources:
      - type: disk_use_percent
        arg: /
      - type: memory_use_percent
      - type: processor_use
      - type: processor_temperature
      - type: last_boot
```

## Xamoi Temp / Humidity Sensor
- [Telink Flasher (atc1441.github.io) ](https://atc1441.github.io/TelinkFlasher.html)


---
## Credits
- https://community.home-assistant.io/t/pi-poe-hat-fan-control/149115/20?u=billsky