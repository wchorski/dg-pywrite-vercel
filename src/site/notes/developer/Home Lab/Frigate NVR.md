---
{"dg-publish":true,"permalink":"/developer/home-lab/frigate-nvr/","dgPassFrontmatter":true}
---

A complete CCTV system that is integrated into [[developer/Home Lab/Home Assistant\|Home Assistant]], This was impart inspired by the lack luster results from [[motion eye\|motion eye]] with it's integrated Object Detection
## installation
I run [[developer/Home Lab/Home Assistant\|Home Assistant]] on a [[developer/Hardware/Pi4\|Raspberry Pi 4 4gb]]. Looking at it's current resource usage,

- 20% CPU
- 42% (2 / 4 gb) RAM

I wanted to see how'd it would handle a couple camera feeds with **Frigate**
## Camera Config
check the `frigate.yaml`

```yaml
mqtt:
	# same IP as HAOS. Using Mosquitto add-on
  host: 192.168.0.0 
  # I set a user up as a basic HA user with login
  user: USERNAME
  password: PASSWORD
cameras:
	# Tapo Camera
  foyer: 
    ffmpeg:
      inputs:
        - path: rtsp://USERNAME:PASSWORD@cam-1.lan/stream2
          roles:
            - rtmp
            - record
            - detect
    detect:
      width: 1280
      height: 720
    record:
      enabled: True
      retain:
        days: 0
        mode: all
    
	# USB Camera
  livingroom:
    ffmpeg:
      inputs:
      - path: /dev/video0
        roles:
          - rtmp
          - detect
          - record
      input_args: -f video4linux2 -avoid_negative_ts make_zero -fflags +genpts+discardcorrupt -use_wallclock_as_timestamps 1
    detect:
      width: 640
      height: 480
      
database:
  path: /config/frigate.db
  
```


## First Impressions
setting up my cheap [Tapo C200](https://www.tp-link.com/us/home-networking/cloud-camera/tapo-c200/) camera via RTSP, and USB [PlayStation Eye](https://en.wikipedia.org/wiki/PlayStation_Eye) camera was fairly strait forward
## Recommended Cameras
- [Amazon.com: Customer reviews: Amcrest 5MP Turret POE Camera, UltraHD Outdoor IP Camera POE with Mic/Audio, 5-Megapixel Security Surveillance Cameras, 98ft NightVision, 103° FOV, MicroSD (256GB), (IP5M-T1179EW-28MM)](https://www.amazon.com/Amcrest-5-Megapixel-NightVision-Weatherproof-IP5M-T1179EW-28MM/product-reviews/B083G9KT4C/ref=cm_cr_dp_d_show_all_btm?ie=UTF8&reviewerType=all_reviews)
## Home Assistant Notifications
Here is a premade automation blueprint  by [hunterjm)](https://gist.github.com/hunterjm)- [Frigate Notification Blueprint (github.com)](https://gist.github.com/hunterjm/23c1588a9f2b8b9c2a62ffc364e17f8c)
## Troubleshooting 

```shell
frigate.comms.mqtt             ERROR   : Unable to publish to stats: client is not connected
```

I never set up a MQTT server so I just installed the [mosquitto)](https://github.com/home-assistant/addons/blob/master/mosquitto/DOCS.md) addon available inside of Home Assistant.

## Docker Container
For those who installed [[developer/Home Lab/Home Assistant\|Home Assistant]] Containered version, **Frigate** must be installed like so
### compose.yml
```yml
version: "3.9"
services:
  frigate:
    container_name: frigate
    privileged: true # this may not be necessary for all setups
    restart: unless-stopped
    image: ghcr.io/blakeblackshear/frigate:stable
    shm_size: "64mb" # update for your cameras based on calculation above
    devices:
      - /dev/bus/usb:/dev/bus/usb # passes the USB Coral, needs to be modified for other versions
      - /dev/apex_0:/dev/apex_0 # passes a PCIe Coral, follow driver instructions here https://coral.ai/docs/m2/get-started/#2a-on-linux
      - /dev/dri/renderD128 # for intel hwaccel, needs to be updated for your hardware
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./config:/config
      - ./storage:/media/frigate
      - type: tmpfs # Optional: 1GB of memory, reduces SSD/SD Card wear
        target: /tmp/cache
        tmpfs:
          size: 1000000000
    ports:
      - "5000:5000"
      - "8554:8554" # RTSP feeds
      - "8555:8555/tcp" # WebRTC over tcp
      - "8555:8555/udp" # WebRTC over udp
    environment:
      FRIGATE_RTSP_PASSWORD: "password"
```

nest a `./config/config.yml` to setup cameras
### MQTT Broker
follow the [[developer/Home Lab/Mosquitto MQTT Broker\|Mosquitto MQTT Broker]] article if you need to setup a stand alone MQTT broker

## HA Notifications without blueprint
https://github.com/blakeblackshear/frigate/discussions/2898

---
## Credits
- [Frigate and Home Assistant -Home Automation Guy](https://www.youtube.com/watch?v=gQdtGLRzKRI)
- [Try to use a webcam · Issue #1184 · blakeblackshear/frigate (github.com)](https://github.com/blakeblackshear/frigate/issues/1184)
- [Home Assistant Frigate integration for local image recognition - YouTube](https://www.youtube.com/watch?v=Q2UT78lFQpo)
- [FRIGATE NVR Part 3 - Actionable Notifications and Dashboard Cards - YouTube](https://www.youtube.com/watch?v=RWsT-x7yYXI)
- [Installation | Frigate](https://docs.frigate.video/frigate/installation)
- [Configuring the Mosquitto MQTT Docker container for use with Home Assistant — Home Automation Guy](https://www.homeautomationguy.io/blog/docker-tips/configuring-the-mosquitto-mqtt-docker-container-for-use-with-home-assistant)