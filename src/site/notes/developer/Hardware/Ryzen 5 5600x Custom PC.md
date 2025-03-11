---
{"codename":"FrostByte","tags":["hardware"],"dg-publish":true,"created":"2024-07-19","updated":"2024-07-19","permalink":"/developer/Hardware/Ryzen 5 5600x Custom PC/","dgPassFrontmatter":true}
---


- aio  dimensions - 27 cm tall x 13 cm length x 5
- find out case radiator compatibility 

my computer for gaming, music making, & video editing 

DNS  - **frost.lan** *-via [[developer/Home Lab/Pi-hole\|Pi-hole]]*

## Build
| part         | brand                                                                                                                                                                                   | model                               | price   |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- | ------- |
| OS           | Windows                                                                                                                                                                                 | 10                                  |         |
| CPU          | AMD RYZEN                                                                                                                                                                               | Ryzen 5 5600x (6 cores, 12 threads) | $237.50 |
| Motherboard  | Gigabyte Aorus                                                                                                                                                                          | elite wifi mobo x570                |         |
| RAM          | G Skill                                                                                                                                                                                 | 32G Trident RGB                     |         |
| ~~GPU~~      | ~~Gigabyte Aorus~~                                                                                                                                                                      | ~~Windforce OC RTX 2080 SUPER 8G~~  |         |
| GPU          | AMD                                                                                                                                                                                     | ???                                 |         |
| ~~Cooler~~   | ~~IBuyPower~~ [link](https://www.reddit.com/r/iBUYPOWER/comments/bjdu9j/what_aio_cooler_does_ibuy_power_use_and_how_is/?utm_source=BD&utm_medium=Search&utm_name=Bing&utm_content=PSR1) | ~~(oem Apaltek Kraken M22 NZXT)~~   |         |
| Cooler       | Liquid Freezer II 240mm                                                                                                                                                                 | Arctik                              |         |
| ~~Cooler~~   | ~~AMD Wraith cooler~~                                                                                                                                                                   |                                     |         |
| Storage main | SanDisk Extreme PRO M.2                                                                                                                                                                 | Sandisk NVMe 1tb                    |         |
| Storage      | SanDisk Extreme PRO M.2                                                                                                                                                                 | NVMe 2tb                            |         |
| Storage      |                                                                                                                                                                                         | 500gb 2.5 SSD                       |         |
| Storage      |                                                                                                                                                                                         | 500gb 2.5 SSD                       |         |
| PSU          | Seasonic                                                                                                                                                                                | 850W gold+                          |         |
| Case         | Corsair                                                                                                                                                                                 |                                     |         |

Bought as a prebuilt of Facebook Marketplace **$1,600** with a AMD Ryzen 9 3900x. I swapped out the CPU with the Ryzen 5 5600x because of the better single core performance. 
## Programs & Apps 
- Music writing & arrangement - [FL Studio (image-line.com)](https://www.image-line.com/fl-studio/)
- Cutting music to picture -  [Ableton Live 9 ](https://www.ableton.com/en/blog/live-9-7-available-now/)
- Video editing - [DaVinci Resolve 18 | Blackmagic Design](https://www.blackmagicdesign.com/products/davinciresolve/)
- control onboard RGB - [OpenRGB](https://openrgb.org/)

## Peripherals 
| gear       | brand       | model | price |
| ---------- | ----------- | ----- | ----- |
| monitor    |             |       |       |
| speakers   | Yamaha      |       |       |
| keyboard   |             |       |       |
| mouse      |             |       |       |
| headphones |             |       |       |
| soundcard  | Solid State |       |       |

## Enable SSH
- [link](https://winaero.com/enable-openssh-server-windows-10/#:~:text=Windows 10 Enable OpenSSH Server 1 Open the,Install button. 5 Restart Windows 10. See More.)
- [link](https://adamtheautomator.com/openssh-windows/#Adding_a_Windows_Firewall_Rule_to_Allow_SSH_Traffic)
- `Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'`


## future silent build
[De Fausti](https://www.youtube.com/channel/UCu8jiWN4yBjYw5W9Jgn0YHQ)
I use open headphones and am also super sensible to loud PCs. My latest build has full PWM 140mm SilentWings 3 and a DarkRock 4 cooler, the loudest part is now the GPU and backup HDD. But the best part IMO is my Seasonic fanless titanium PSU, that thing is a fucking beast. The pump noise on AIO was exactly what I was worried about and it seems like I made the right choice in the end. Thanks for this!

[[developer/Hardware/Homelab Hardware PC Builds\|Homelab Hardware PC Builds]]

#todo 
- [ ] finish this page