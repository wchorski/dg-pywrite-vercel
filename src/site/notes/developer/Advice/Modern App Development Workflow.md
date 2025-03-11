---
{"dg-publish":true,"tags":["webdev","nodejs","commandline","gamedev"],"permalink":"/developer/Advice/Modern App Development Workflow/","dgPassFrontmatter":true}
---

This is advice for anyone developing source code and want an efficient way of splitting projects but retaining a lot of the benefits of your #templates. 

You could apply this to any digital assets but I'll be focusing on benefits of people developing source code for things like
- [[developer/NodeJS/NodeJS\|NodeJS]]
- [[developer/ReactJS/ReactJS\|ReactJS]]
- [[developer/NextJS/NextJS\|NextJS]]
- [[developer/Home Lab/GitHub\|GitHub]]
## Cloud Storage
This is going to be your main driver for keeping your local development in sync. I switch between laptop and desktop with similar developer environments. I want that transition between machines to be seamless. PLUS I also have a [[developer/Hardware/Intel NUC\|Production Server]] That deploys these projects.

You could use any sorts of cloud providers, but here I'm using [[developer/Home Lab/Nextcloud\|Nextcloud]]. 
## Desktop & Laptop
Here I'll explain the setup for your development machines. I assume you have [[developer/NodeJS/NodeJS\|NodeJS]] and [[developer/Home Lab/Docker\|Docker]] installed already.
### Ignore files
an `.env` file should be unique to the environment that it lives. *duh*. And we'll follow the same process as each machine is it's own environment. 

On your development machines you should be able to edit an `ignore` file for your cloud sync ([[developer/Home Lab/Nextcloud\|Nextcloud]] has this feature)

![[attachments/nextcloud-ignore.PNG|nextcloud-ignore.PNG]]

```txt
# .ignore
.env
.node_modules
.next
.keystone
.react-mail
.DS_Store
```

This keeps your configurations flexible enough without having too much fuss. I use `.env.prod` and `.env.dev` files synced to the cloud. That helps me keep a reminder of what I use between folders wile ignoring them to the public repo
## Server
my [[developer/Hardware/Intel NUC\|Production Server]] is the same machine that runs [[developer/Home Lab/Nextcloud\|Nextcloud]]. So in this instance, I'm able to dive right into the filesystem and run `docker compose build` / `docker compose up -d` at the root of whatever project I want to update / deploy
## Git Repo
I assume you also want to share some of these revolutionary apps to the world as well? With this set up you can stop using [[developer/Home Lab/GitHub\|GitHub]] as a backup solution (like firing off `git push` every time a file is edited).

The cloud is doing it's thing to keep all files up to date, so now you can more methodically `git commit -m ''` when a new feature makes progress.

---
## Credits
- [[developer/developer_box📦\|developer_box📦]]
- 