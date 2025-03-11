---
{"tags":["linux","Thunderbird","flstudio","music"],"dg-publish":true,"permalink":"/developer/backup-solutions/","dgPassFrontmatter":true}
---

This is my chain of programs that help me backup and store data whether it's local clone, version controlled, and or remote sight backups

- [[developer/Home Lab/FreeFileSync\|FreeFileSync]]
	- user friendly GUI
	- clones files 1 to 1. Great for parodying data onto a duplicate machine
- [[developer/Home Lab/Duplicati\|Duplicati]]
	- version control
	- auto deletion of older data
	- user friendly GUI
	- automated backups via the app
- [[developer/Linux/rsync\|rsync]]
	- command line
	- automated backups over SSH
	- great for remote site backups
### Image, a Perfect Clone
Maybe you want a 1 to 1 copy of your OS. Something that you can install on new hardware and be back and running exactly how you left off. For the [[developer/Linux/Linux\|Linux]] users out there check out [[developer/Linux/Create an Cloned Image of Your Raspberry Pi\|Create an Cloned Image of Your Raspberry Pi]]

---
## App Specific

### Thunderbird Email client
#### backup
1. find hamburger menu top right
2. help > more troubleshooting information > Profile Folder > <button>Open Folder</button> (Local Drive)
3. close Tunderbird app
4. Go 3 directory levels up to "Roaming" folder

#### restore
1. install & run Thunderbird
2. skip setting up accounts
3. menu > help > troubleshooting info > Profile > <button>Open Folder</button> (Local Drive)
4. just like backup, Go 3 directory levels up to "Roaming" folder
5. paste in backup `Thunderbird` over itself
	1. agree to merge, replace directories


#todo
- [ ] finish "Linux Disk Image with DD Command and Compress"