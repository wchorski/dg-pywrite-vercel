---
{"dg-publish":true,"permalink":"/developer/windows-10/ssh-server-and-key-login/","dgPassFrontmatter":true}
---

I wanted to send commands from other computers to my **Windows 10** machine. Upon initial install of Windows, I logged in with my Microsoft account and was met with `Permission denied, please try again.` when trying passwords

## Install OpenSSH Server
1. install the client. Search for `More Options` in the Windows start menu
2. install `OpenSSH Server`
3. search for `Services` in the Windows Start menu
4. right click `Open SSH SSH Server` and hit `Properties`
5. set `Startup type` to `automatic`

## Setup SSH Keys
1. Edit files as **Administrator**. I use **VS Code** ran as **Administrator** to edit these files
2. create a file named `administrators_authorized_keys` here →`'C:\ProgramData\ssh\administrators_authorized_keys'`
3. in an admin privileged terminal run `icacls .\administrators_authorized_keys /inheritance:r /grant "Administrators:F" /grant "System:F"`
4. create an SSH key on your remote computer
5. copy the `.ssh/id_rsa.pub` contents to the clipboard
6. On **Windows 10** paste key into `'C:\ProgramData\ssh\administrators_authorized_keys'`

`sshd_config` file
```shell
...
PubkeyAuthentication yes
...

# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
PermitEmptyPasswords no
...
```

in `Services` window. Restart `OpenSSH SSH Server` 

## Bonus
Now I'm able to turn off my windows machine with an authorized remote computer via
`ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i /config/.ssh/id_rsa UERSNAME@192.168.x.xxx 'rundll32.exe powrprof.dll, SetSuspendState Sleep'`

---
## Credits
- [SSH To Windows Using Public Key - YouTube](https://www.youtube.com/watch?v=Wx7WPDnwcDg)https://winscp.net/eng/docs/guide_windows_openssh_server
- https://winscp.net/eng/docs/guide_windows_openssh_server


## Backlinks
- [[developer/Windows 10/Microsoft Windows\|Microsoft Windows]]