---
{"dg-publish":true,"permalink":"/developer/home-lab/dynamic-dns-choices/","dgPassFrontmatter":true}
---

You got your shiny self hosted app and you about to open it up to the world. No problem, just have people type in your public IP address. i.e. `98.143.344.321` in the address bar to get to you're site. 

> [!warning] Do not share out your Public IP address
> Sharing out your Public IP is just like copy and pasting your personal phone number. It may not be lethal, but it does open you up to a wide range of spam or nefarious hackers

## Domain Name System

getting a domain name from one of the many providers
- [porkbun.com](https://porkbun.com/)(what I'm using)
- [Namecheap.com](https://www.namecheap.com/)

## Point your domain name to your Public IP

Now that you got your shiny new domain name. The next step is to point your Public IP as an *A record* to your domain.

Here is how I set it up in [PorkBun](https://porkbun.com/)'s Dashboard

![attachments/dynamic-dns-a-record 1.png|dynamic-dns-a-record](/img/user/attachments/dynamic-dns-a-record%201.png)**********

> [!tip] Your ISP can change your Public IP at any time
> Without a **Static IP** your service provider can and will change your public IP address. Although I've gone years without it changing, it still can happen without notice. For the extra paranoid you could monitor your IP with [[developer/Home Lab/Home Assistant\|Home Assistant]])

So... Let's get a static IP. Well, [maybe not](https://blog.noip.com/cost-of-a-static-ip-address-2). Here are some estimates of what a static IP could run you. 

> - Brightdata $10.05 per GB
> - AT&T $15 to $40/month
> - Verizon Business Account Needed Starting at $99.95/month
> - Nordlayer +$50/month

Sure there are VPNs that can also do the job for cheap. But there's an easier way
## Dynamic DNS

Instead of pointing your domain to an **A record** of your Public IP, you can create a pretty domain like `mydomain.tplink.com` that you point it too.

> [!note] Using this method it is now a **CNAME** record type

### Built in Router DDNS

You're router probably has a dynamic dns solution that you can enable through the dashboard. You can reach this dashboard by typing this local IP address into your url bar (example `192.168.0.1`).

I have a [Archer AX3000 | Dual Band Gigabit Wi-Fi 6 Router | TP-Link](https://www.tp-link.com/us/home-networking/wifi-router/archer-ax3000/). This is what my dashboard looks like.

![attachments/ddns-tp-link 1.png|ddns-tp-link](/img/user/attachments/ddns-tp-link%201.png)

Last thing to do is go back to your domain name provider and point your domain (or any subdomain) to your created `mydomain.tplink.com` address as a **CNAME** record. I recommend this option for as the easiest way to get up and running as there is very little configuration.

### DuckDNS

This is for the more technically inclined.

I've written a separate guide for [[developer/Home Lab/DuckDNS\|DuckDNS]] that shows you how to setup the scripts with a raspberry pi (or any Debian distro)
### Your Domain Name Provider

Skip the middle man and have the DNS record update automatically. Provided that your DNS provider offers an API. I've yet to experiment with this, but it makes sense to remove an unneeded service and automate the record change strait to the provider. 

Here is a tool for porkbun users [mietzen/porkbun-ddns: porkbun-ddns is an unoffical DDNS-Client for Porkbun Domains. (github.com)](https://github.com/mietzen/porkbun-ddns)

---
## backlinks
- [[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]]
