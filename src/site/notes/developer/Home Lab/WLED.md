---
{"dg-publish":true,"permalink":"/developer/home-lab/wled/","dgPassFrontmatter":true}
---

[Aircoookie/WLED: Control WS2812B and many more types of digital RGB LEDs with an ESP8266 or ESP32 over WiFi! (github.com)](https://github.com/Aircoookie/WLED)

> [!info](https://github.com/Aircoookie/WLED)
> A fast and feature-rich implementation of an ESP8266/ESP32 webserver to control NeoPixel (WS2812B, WS2811, SK6812) LEDs or also SPI based chipsets like the WS2801 and APA102!

[Install WLED](https://install.wled.me/) for wireless control of LEDs over wifi. 

> [!warn] I had trouble during install. Something about "hold reset button try again". I just ended up using a different windows machine and then things worked out... maybe I could have tried a different browser? Idk.

## Edit Endpoint
to easily offload and load in configuration files
- `http://<IPADDRESS>/edit`

## Control over DMX with Jinx
- [DMX to WLED using jinx! - YouTube](https://www.youtube.com/watch?v=K3qnVgu3Txc&t=10s)
- [Jinx Install |Live-LEDs.de | LED Matrix Software & More](http://www.live-leds.de/)

**mac addr of new 104** -  7C:9E:BD:48:27:94

### connections
- [[developer/Home Lab/Home Assistant\|Home Assistant]]
- [[developer/Home Lab/Hyperion\|Hyperion]]
- 