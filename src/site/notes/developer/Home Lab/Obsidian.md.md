---
{"dg-publish":true,"permalink":"/developer/home-lab/obsidian-md/","dgPassFrontmatter":true}
---

#notes #markdown #webdev

[https://obsidian.md/](https://obsidian.md/)

> a note taking app for people who are obsessed with Markdown 


I've dabbled in blogging / tutorial making, but was always bogged down with the speed of [[developer/Home Lab/Wordpress\|Wordpress]]. with **Obsidian** I get 

- the speed of a local text editor, 
- extensibility of an [IDE](https://social.technet.microsoft.com/wiki/contents/articles/29057.the-small-basic-environment-ide.aspx),  
- simplicity that [Markdown](https://daringfireball.net/projects/markdown/) offers. 

Plus I can use it for client tracking, tutorial making, and idea generation. I'd recommend anybody that knows more than 5 keyboard shortcuts to ditch whatever blogging tool they currently use [[🚿 shower thoughts/In Flavor\|In Flavor]] of **Obsidian**

---
### connections
- easily convert and publish with [[developer/tutorials/Obsidian-Zola\|Obsidian-Zola]] or [[developer/tutorials/Quartz-w_Boswell\|Quartz-w_Boswell]] 
- Live Sync instances with [[developer/Obsidian-Livesync.md\|Obsidian-Livesync]] 

### installation
- [https://obsidian.md/](https://obsidian.md/)
- **Linux Users** - I got it off [Flatpak](https://www.flatpak.org/)
> [!warning] Android 10 Users 
> [Android Can't write to SD Card Fix](https://forum.obsidian.md/t/bug-cant-write-to-sd-card-external-storage/20694/11?u=wchorski)
> To utilize storage on an SD card you have to save it in a specific spot `/SD_CARD/Android/data/md.obsidian/VAULT_HERE`

---
## Plugins I use
- [vrtmrz/livesync-bridge (github.com)](https://github.com/vrtmrz/livesync-bridge)
- [pjeby/quick-explorer](https://github.com/pjeby/quick-explorer)
	- Perform file explorer operations (and see your current file path) from the Obsidian.md title bar
- [deathau/cm-editor-syntax-highlight-obsidian](https://github.com/deathau/cm-editor-syntax-highlight-obsidian)
	- allows syntax highlighting for code blocks in the editor.
- [timhor/obsidian-editor-shortcuts](https://github.com/timhor/obsidian-editor-shortcuts)
	- keyboard shortcuts commonly found in code editors such as Visual Studio Code (vscode) or Sublime Text
- [delashum/obsidian-checklist-plugin](https://github.com/delashum/obsidian-checklist-plugin)
	- create checklists from tags, organize inside any file, and display master list in a side panel 
- [blacksmithgu/obsidian-dataview](https://github.com/blacksmithgu/obsidian-dataview)
	- Make dynamically filtered tables & graphs for dashboards
- [tgrosinger/advanced-tables-obsidian](https://github.com/tgrosinger/advanced-tables-obsidian)
	- Improved table navigation, formatting, and manipulation

## Custom CSS via Snippets

## Graph View Customization
1. group colors
2. exclude directories / files

## How I Keep up on Clients
1. metadata
2. tut on dataviews and tables
3. how I organize via directories

## Publish Vault
1. I've use [[developer/tutorials/Obsidian-Zola\|Obsidian-Zola]] because it's dead simple and has nice out of the box features. Publish to **Netlify**
2. I have experimented with [[developer/tutorials/Quartz-w_Boswell\|Quartz-w_Boswell]]. I like the ability to compile locally but looking into how to add more features. Publish to **Github Pages**
3. Currently I use [[developer/Projects/Obsidian Publish with NextJS\|Obsidian Publish with NextJS]] to show off my digital garden.

---
## backlinks
[[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]]

---
#todo 
- [ ] finish "custom CSS ..."
- [ ] finish "Graph View ..."
- [ ] finish "How I Keep up on Clients"