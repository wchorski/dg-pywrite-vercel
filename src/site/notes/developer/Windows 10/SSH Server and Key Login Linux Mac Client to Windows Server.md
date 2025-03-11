---
{"dg-publish":true,"tags":["ssh","terminal","IoT","homelab"],"permalink":"/developer/windows-10/ssh-server-and-key-login-linux-mac-client-to-windows-server/","dgPassFrontmatter":true}
---

I wanted to send commands from other computers to my **Windows 10** machine. Upon initial install of Windows, I logged in with my Microsoft account and was met with `Permission denied, please try again.` when trying passwords

## On The Client
The **client** refers to your personal machine that you use everyday. This is the physical laptop (etc) you use to login
### Create Key
```shell
ssh-keygen -t ed25519
```

#### Output
You can just hit enter to skip over this
```output
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username/.ssh/id_ed25519):

Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\username/.ssh/id_ed25519.
Your public key has been saved in C:\Users\username/.ssh/id_ed25519.pub.
The key fingerprint is:
SHA256:OIzc1yE7joL2Bzy8!gS0j8eGK7bYaH1FmF3sDuMeSj8 username@LOCAL-HOSTNAME

The key's randomart image is:
+--[ED25519 256]--+
|        .        |
|         o       |
|    . + + .      |
|   o B * = .     |
|   o= B S .      |
|   .=B O o       |
|  + =+% o        |
| *oo.O.E         |
|+.o+=o. .        |
+----[SHA256]-----+
```

This key will live at `~/.ssh/id_ed25519.pub`. We will use this id later
## On the Server
the **server** refers to the remote machine that you want to control with the client. In this case making a ssh tunnel that connects your **client** device to the **server**
### Install SSH Service
```shell
# Install the OpenSSH Server 
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```
### Start Service
```shell
# Start the sshd service 
Start-Service sshd 
# OPTIONAL but recommended: 
Set-Service -Name sshd -StartupType 'Automatic'
```

## Back to Client
Here you can test logging into the server via password
```shell
ssh L150DDVR07
```

You will be prompted to enter your password and should see your terminal confirm that you are now tunneled into the **Windows Server**

```shell
Microsoft Windows [Version 10.0.20348.2582]
(c) Microsoft Corporation. All rights reserved.

l150\wchorski@L150D4DVR C:\Users\wchorski>
```

Now we can setup ssh key login so no more need for passwords.
## Back on the Server
On the **Windows Server** desktop, open PowerShell as an administrator. You can either right click the app icon or run `Start-Process powershell -Verb runAs` in a normal user launched Powershell.
### Copy in Auth Key

```powershell
## create auth key storage file
New-Item -Force -Path (Join-Path "C:\ProgramData\ssh" "administrators_authorized_keys")

## add in your ssh key from your client
Add-Content -Path 'C:\ProgramData\ssh\administrators_authorized_keys.txt' -Value "ssh-ed25519 A6AAC3UzaC1lZDI1NTE5AZAA3ImLyZ6YNxdiCOc8asDb7ZV1GT1ha6tEmGoctM3hU7qE USERNAME@moeits.com"
```

The key `ssh-ed25519 A6AAC3UzaC1lZDI1NTE5AZAA3ImLyZ6YNxdiCOc8asDb7ZV1GT1ha6tEmGoctM3hU7qE USERNAME@moeits.com` is an example. Your key is the contents found in your **client** file ``~/.ssh/id_ed25519.pub``

#### GUI experience
If you'd like to do the above not in terminal you can open `notepad.exe`. 
```
## open notepad as administrator
Start-Process 'C:\windows\system32\notepad.exe'
```

Type in `%programdata%/ssh` into the address bar to find hidden folder and create a file called `administrators_authorized_keys` to drop your ssh key into

Notepad appends `.txt` to the file so you'll need to remove it
```powershell
## take off `.txt` extension
Rename-Item 'C:\ProgramData\ssh\administrators_authorized_keys.txt' 'C:\ProgramData\ssh\administrators_authorized_keys'
```
#### Set Correct Permissions
This isn't a requirement, but just in case you created the file with a *user notepad* this on command will set permissions to the file correctly.

This, effectively, grants Full Control privileges to the `administrators_authorized_keys` file to both `Administrators` and `SYSTEM`.
```powershell
 $acl = Get-Acl C:\ProgramData\ssh\administrators_authorized_keys
    $acl.SetAccessRuleProtection($true, $false)
    $administratorsRule = New-Object system.security.accesscontrol.filesystemaccessrule("Administrators","FullControl","Allow")
    $systemRule = New-Object system.security.accesscontrol.filesystemaccessrule("SYSTEM","FullControl","Allow")
    $acl.SetAccessRule($administratorsRule)
    $acl.SetAccessRule($systemRule)
    $acl | Set-Acl

## I also had this shorter version here. Idk if it is any different
icacls 'C:\ProgramData\ssh\administrators_authorized_keys' /inheritance:r /grant "Administrators:F" /grant "System:F"
```
### Configure SSH Service
edit with Notepad `C:\ProgramData\ssh\sshd_config` 
```powershell
Start-Process 'C:\WINDOWS\system32\notepad.exe' 'C:\ProgramData\ssh\sshd_config'
```
#### sshd_config file
Change these 3 lines 
```txt
...
PubkeyAuthentication yes
...

# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
PermitEmptyPasswords no
...
```
#### restart ssh service in powershell
```powershell
Restart-Service sshd
```
## Example Login
```shell
# assumes you have the same username on client and server
ssh L150DDVR07

# if targeting specific user
ssh wchorski@L150DDVR07
```

---
## Credits
- [How to Use SSH to Remotely Connect to Windows 10 or 11 – TheITBros](https://theitbros.com/ssh-into-windows/)
- [Get started with OpenSSH for Windows | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=powershell)
- [Key-based authentication in OpenSSH for Windows | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement)
- [Windows SSH server refuses key based authentication from client - Super User](https://superuser.com/questions/1445976/windows-ssh-server-refuses-key-based-authentication-from-client)
- [windows 10 - Using New-Item cmdlet with Literalpath in Powershell? - Stack Overflow](https://stackoverflow.com/questions/54197595/using-new-item-cmdlet-with-literalpath-in-powershell)
- [How to Use SSH to Remotely Connect to Windows 10 or 11 – TheITBros](https://theitbros.com/ssh-into-windows/)
- [Running a command as Administrator using PowerShell? - Stack Overflow](https://stackoverflow.com/questions/7690994/running-a-command-as-administrator-using-powershell)