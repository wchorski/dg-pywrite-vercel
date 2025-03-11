---
{"dg-publish":true,"permalink":"/developer/Windows 10/Host Static Website on Windows 10 IIS/","dgPassFrontmatter":true}
---

## Enable IIS Feature
You may need to enable IIS first before using the app.

![attachments/Pasted image 20240702111949.png](/img/user/attachments/Pasted%20image%2020240702111949.png)

![attachments/Pasted image 20240702112002.png](/img/user/attachments/Pasted%20image%2020240702112002.png)

## Create New Site
Now you can search in your apps for `IIS` or **Internet Information Services** and run the GUI

Right click to open context menu to "Add Website..."
![attachments/Pasted image 20240702112557.png](/img/user/attachments/Pasted%20image%2020240702112557.png)

Configure the site name and file location
![attachments/Pasted image 20240702112701.png](/img/user/attachments/Pasted%20image%2020240702112701.png)

In this example, the root of the website files lives at `C:\inetpub\wwwroot\mysite`. You may organize your files anyway you'd like. It's recommended that you have these files outside and above your `./User` files
## Edit Host File
We must tell the #windows machine to resolve the `mysite.local` domain name

Run **Notepad** as "Administrator" and open this file

```shell
C:\Windows\System32\drivers\etc\hosts
```

Add in a line at the bottom. Use the server's IP address (static or DHCP given address)

```shell
10.42.12.6:80 mysite.local
```

---
## Credits
- [How to install IIS (Internet Information Services) on Windows 10? (koskila.net)](https://www.koskila.net/how-to-install-iis-internet-information-services-on-windows-10/#:~:text=To%20enable%20IIS%20on%20a%20Windows%2010%20computer%2C,Practice%20your%20patience%20...%205%20That%E2%80%99s%20it%21%20)
- [Windows Server | #09: How to Host a Website in IIS on Windows Server 2016/2019? (youtube.com)](https://www.youtube.com/watch?v=r8UOKHCQEW8)