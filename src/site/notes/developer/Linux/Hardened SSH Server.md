---
{"dg-publish":true,"permalink":"/developer/linux/hardened-ssh-server/","dgPassFrontmatter":true}
---

describe_the_problem

## Server

### Unattended Upgrades
```bash
sudo apt install unattended-upgrades
dpkg-reconfigure --priority=low unattended-upgrades
```

> [!warning] Root
> The commands written by the **Server** will all be ran by the `root` user (also assuming ssh was installed when you imaged the machine)

### Create Keys and Harden Server

following [link](https://www.sshaudit.com/hardening_guides.html#ubuntu_22_04_lts) step by step

```bash
rm /etc/ssh/ssh_host_*
ssh-keygen -t rsa -b 4096 -f /etc/ssh/ssh_host_rsa_key -N ""
ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
```

```bash
awk '$5 >= 3071' /etc/ssh/moduli > /etc/ssh/moduli.safe
mv /etc/ssh/moduli.safe /etc/ssh/moduli
```

```bash
sed -i 's/^\#HostKey \/etc\/ssh\/ssh_host_\(rsa\|ed25519\)_key$/HostKey \/etc\/ssh\/ssh_host_\1_key/g' /etc/ssh/sshd_config
```

```bash
echo -e "\n# Restrict key exchange, cipher, and MAC algorithms, as per sshaudit.com\n# hardening guide.\nKexAlgorithms sntrup761x25519-sha512@openssh.com,curve25519-sha256,curve25519-sha256@libssh.org,gss-curve25519-sha256-,diffie-hellman-group16-sha512,gss-group16-sha512-,diffie-hellman-group18-sha512,diffie-hellman-group-exchange-sha256\nCiphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr\nMACs hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,umac-128-etm@openssh.com\nHostKeyAlgorithms ssh-ed25519,ssh-ed25519-cert-v01@openssh.com,sk-ssh-ed25519@openssh.com,sk-ssh-ed25519-cert-v01@openssh.com,rsa-sha2-512,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256,rsa-sha2-256-cert-v01@openssh.com" > /etc/ssh/sshd_config.d/ssh-audit_hardening.conf
```

```bash
service ssh restart
```
### Setup SSH config
```bash
# disable lines in sshd config
nano /etc/ssh/sshd_config
```

```bash
PermitRootLogin no

PasswordAuthentication no
PermitEmptyPasswords no
```

```bash
systemctl restart sshd
```

check your **Standard Audit** & **Policy Audit** score at [SSH Configuration Auditor (ssh-audit.com)](https://ssh-audit.com/)

### Firewall

Allow ssh connections through your firewall
```shell
ufw allow ssh
```

check for open ports
```shell
ss -atpu
```

## Client

the client denotes the machine that you use to remote into the above ssh server (i.e. personal laptop).
### Generate SSH Keys
```bash
# if using rsa make sure u set bit to 4096
ssh-keygen -t rsa -b 4096 

# ed25519 is the new 'better' standard. -C flag is justa comment
ssh-keygen -t ed25519 -C "YOURNAME@mail.com"

# copy key to server
ssh-copy-id USER@SERVERNAME
```

### Test Command from Client
```bash
ssh USER@DOMAN 'touch ~/Documents/file.test'
```

### adding new keys after disabling password login
There is no easy way of adding new keys with `ssh-copy-id` once you disable `PasswordAuthentication no`. That said, hopefully you've already authenticated one PC. Here are the steps to authenticate a new client
1. create new ssh key on the **New Client**
2. copy the `~/.ssh/id_ed25519.pub` key
3. on the **Authenticate Client**, ssh into the **Server**
4. append the key on a new line in the **Server's** `~/.ssh/authorized_keys`
5. Good 2 Go

---
## Credits
- [SSH Configuration Auditor (ssh-audit.com)](https://ssh-audit.com/)
- [5 Easy Ways to Secure Your SSH Server - YouTube](https://www.youtube.com/watch?v=l1iu3iZq1aQ)
- [SSH Hardening Guides (ssh-audit.com)](https://ssh-audit.com/hardening_guides.html)
- [Hardening Access to Your Server | Linux Security Tutorial - YouTube](https://www.youtube.com/watch?v=eeaFoZlSq6I&pp=ygUYaG93IHRvIGhhcmRlbiBzc2ggc2VydmVy)
## Backlinks
- [[developer/Linux/Linux\|Linux]]