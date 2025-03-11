---
{"dg-publish":true,"tags":["sensor","homeassistant","IoT","DIY"],"permalink":"/developer/home-assistant/esp-home-multi-sensor-configuration/","dgPassFrontmatter":true}
---

Configure a micro controller as a motion, temperature, and humidity sensor

### Devices
- [NodeMCU v3](https://mischianti.org/nodemcu-v3-high-resolution-pinout-and-specs/)
- [DHT22](https://components101.com/sensors/dht22-pinout-specs-datasheet)(temp and RH)
-  [PIR](https://www.newark.com/parallax/555-28027/pir-infared-measurement-sensor/dp/11X5850) (motion)

## Pin Out
maybe I'll create this with `mermaidjs`?
#todo 
- [ ] Temp & RH
- [ ] motion
- [ ] board pic

`/esphome/esp-0-multisensor.yaml`
```yml
esphome:
  name: "esp-0-multisensor"

esp8266:
  board: nodemcuv2
  framework:
    version: recommended

# Enable logging
logger:

# Enable Home Assistant API
api:

ota:

sensor:
  - platform: dht
    pin: D2
    temperature:
      name: "esp-0-temp"
    humidity:
      name: "esp-0-humidity"
    update_interval: 60s

binary_sensor:
  - platform: gpio
    pin: D3
    name: "esp-0-motion"
    device_class: motion

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Esphome-Web-78D7E5"
    password: "ucTgXWUNB2I4"

captive_portal:
    
```