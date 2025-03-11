---
{"dg-publish":true,"permalink":"/developer/tutorials/find-mac-address-of-any-ap/","dgPassFrontmatter":true}
---

It's criminal when a manufacturer doesn't print the MAC address onto the device. With these steps you should be able to find any MAC address

1. connect to the device's AP

2. `ipconfig /all`
```shell
Wireless LAN adapter Wi-Fi:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Marvell AVASTAR Wireless-AC Network Controller
   Physical Address. . . . . . . . . : F0-6E-0B-D1-CF-A2
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::55a4:79fe:4b7f:98a7%18(Preferred)
   IPv4 Address. . . . . . . . . . . : 192.168.175.100(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Lease Obtained. . . . . . . . . . : Tuesday, November 22, 2022 2:09:48 PM
   Lease Expires . . . . . . . . . . : Tuesday, November 22, 2022 3:09:47 PM
   Default Gateway . . . . . . . . . : 192.168.175.1 # ← here is what you're looking for
   DHCP Server . . . . . . . . . . . : 192.168.175.1
   DHCPv6 IAID . . . . . . . . . . . : 149974539
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-24-9C-C0-8F-F0-6E-0B-D1-CF-A2
   DNS Servers . . . . . . . . . . . : 192.168.175.1
   NetBIOS over Tcpip. . . . . . . . : Disabled
```

3. ping the Default Gateway → `ping 192.168.175.1`
```shell
Pinging 192.168.175.1 with 32 bytes of data:
Reply from 192.168.175.1: bytes=32 time=1ms TTL=255
Reply from 192.168.175.1: bytes=32 time=3ms TTL=255
Reply from 192.168.175.1: bytes=32 time=4ms TTL=255
Reply from 192.168.175.1: bytes=32 time=2ms TTL=255

Ping statistics for 192.168.175.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 4ms, Average = 2ms
```

4. find the attributes of the Default gateway → `ARP -A`
```
Interface: 192.168.175.100 --- 0x12
  Internet Address      Physical Address      Type
  192.168.175.1         38-1f-8d-bc-4e-6e     dynamic # ← this is the default gateway IP & MAC
  224.0.0.2             01-00-5e-00-00-02     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static
```
 38-1f-8d-bc-4e-6f
 
---
## Citations
- [How to Find the MAC Address of an Access Point (azcentral.com) ](https://yourbusiness.azcentral.com/mac-address-access-point-19756.html)

## Backlinks
- [[developer/developer_box📦\|developer_box📦]]