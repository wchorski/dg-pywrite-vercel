---
{"dg-publish":true,"permalink":"/developer/home-lab/mosquitto-mqtt-broker/","dgPassFrontmatter":true}
---

Frigate with Home Assistant requires an MQTT broker. Here is how to set it up
####  compose.yml
```yml
version: '3.0'

services: 
 mosquitto:
    image: eclipse-mosquitto
    container_name: mosquitto
    volumes:
      - ./:/mosquitto
      - ./data:/mosquitto/data
      - ./log:/mosquitto/log
    ports:
      - 1883:1883
      - 9001:9001 
```

#### /mosquitto/config/mosquitto.conf
```conf
persistence true
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log
listener 1883

## Authentication ##
allow_anonymous true
```

#### setup mosquitto authentication

add a username password so only allowed devices can access this broker

> [!note] execute command via `/bin/sh`

```bash
mosquitto_passwd -c /mosquitto/config/password.txt hass

# set new password
```

#### /mosquitto/config/mosquitto.conf

update this file to lock down the broker 

```conf
persistence true
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log
listener 1883

## Authentication ##
allow_anonymous false
password_file /mosquitto/config/password.txt
```

There will be a permission's issue, resolve this by changing ownership of all files in that folder

```bash
sudo chown USER -R /mosquitto
```