---
{"dg-publish":true,"permalink":"/developer/home-lab/mqtt-mosquitto/","dgPassFrontmatter":true}
---

hero_image

> [!note] link 
> manufacture_description

## 2. Create base folder for mqtt configuration

```shell
mkdir mqtt5
cd mqtt5

# for storing mosquitto.conf and pwfile (for password)
mkdir config

```

## [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#3-create-mosquitto-config-file---mosquittoconf)

## 3. Create Mosquitto config file - mosquitto.conf

```shell
nano config/mosquitto.conf
```

Basic configuration file content below including websocket config

```
allow_anonymous false
listener 1883
listener 9001
protocol websockets
persistence true
password_file /mosquitto/config/pwfile
persistence_file mosquitto.db
persistence_location /mosquitto/data/
```

## [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#4-create-mosquitto-password-file---pwfile)

## 4. Create Mosquitto password file - pwfile

```shell
touch config/pwfile
```

## [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#5-create-docker-compose-file-called-docker-composeyml)

## 5. Create docker-compose file called 'docker-compose.yml'

```yaml
version: "3.7"
services:
  # mqtt5 eclipse-mosquitto
  mqtt5:
    image: eclipse-mosquitto
    container_name: mqtt5
    ports:
      - "1883:1883" #default mqtt port
      - "9001:9001" #default mqtt port for websockets
    volumes:
      - ./config:/mosquitto/config:rw
      - ./data:/mosquitto/data:rw
      - ./log:/mosquitto/log:rw

# volumes for mapping data,config and log
volumes:
  config:
  data:
  log:

networks:
  default:
    name: mqtt5-network

```

## [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#6-create-and-run-docker-container-for-mqtt)

## 6. Create and run docker container for MQTT

```shell
# In case you don't have docker-compose you can install it
sudo apt install docker-compose

# Run the docker container for mqtt
sudo docker-compose -p mqtt5 up -d

```

### [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#check-if-the-container-is-up-and-working-note-down-container-id)

### Check if the container is up and working (note down container-id)

```shell
sudo docker ps

```

## [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#7-create-a-userpassword-in-the-pwfile)

## 7. Create a user/password in the pwfile

```shell
# login interactively into the mqtt container
sudo docker exec -it <container-id> sh

# add user and it will prompt for password
mosquitto_passwd -c /mosquitto/config/pwfile user1

# delete user command format
mosquitto_passwd -D /mosquitto/config/pwfile <user-name-to-delete>

# type 'exit' to exit out of docker container prompt

```

Then restart the container

```shell
sudo docker restart <container-id>
```

## [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#8-time-to-test-)

## 8. Time to test !!!

### [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#install-mosquitto-client-tools-for-testing)

### Install mosquitto client tools for testing

```shell
sudo apt install mosquitto-clients

```

### [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#let-us-start-subscriber-now---topic-name--hellotopic)

### Let us start Subscriber now - topic name => 'hello/topic'

```shell
# Without authentication
mosquitto_sub -v -t 'hello/topic'

# With authentication
mosquitto_sub -v -t 'hello/topic' -u user1 -P <password>

# Alternate way in url format
# Format => mqtt(s)://[username[:password]@]host[:port]/topic
mosquitto_sub -v -L mqtt://user1:abc123@localhost/test/topic

```

### [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#let-us-start-publising-to-that-topic)

### Let us start Publising to that topic

```shell
# Without authentication
mosquitto_pub -t 'hello/topic' -m 'hello MQTT'

# With authentication
mosquitto_pub -t 'hello/topic' -m 'hello MQTT' -u user1 -P <password>

# Alternate way in url format 
# Format => mqtt(s)://[username[:password]@]host[:port]/topic
mosquitto_pub -L mqtt://user1:abc123@localhost/test/topic -m 'hello MQTT'

```

## [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#you-can-also-install-a-nice-mqtt-web-client)

## You can also install a nice MQTT Web Client

Read more about it here => [https://mqttx.app/](https://mqttx.app/)

```shell
sudo docker run -d --name mqttx-web -p 80:80 emqx/mqttx-web
```

## [link](https://github.com/sukesh-ak/setup-mosquitto-with-docker#sourcereference-for-mosquitto)

## Source/Reference for Mosquitto

Github => [https://github.com/eclipse/mosquitto](https://github.com/eclipse/mosquitto)

> [!warning] don't use `localhost`. Use IP address assigend by router i.e. `192.168.0.111`

---

## connections

## installation

---
## extras

---
## backlinks
[[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]]
- https://github.com/sukesh-ak/setup-mosquitto-with-docker
