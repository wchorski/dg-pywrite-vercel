---
{"dg-publish":true,"permalink":"/developer/tutorials/obsidian-zola/","dgPassFrontmatter":true}
---

a much easier way to publish Obsidian Vaults (although [[developer/tutorials/Quartz-w_Boswell\|Quartz-w_Boswell]] has some percs, this was much faster to publish and, out of the box, it looks nicer)

### setup
1. start a git repo for your **Obsidian Vault**.
	1. Use `.gitignore` to prevent folders / files from being added to your public garden
2. create a `netlify.toml` inside root of your vault. Copy past from `netlify.example.toml`
	1. [ppeetteerrs/obsidian-zola: A no-brainer solution to turning your Obsidian PKM into a Zola site. (github.com)](https://github.com/ppeetteerrs/obsidian-zola)
	2. fork the repo and change  top `command = ` endpoint (if you'd like to change anything in the design)
3. create [netlify](https://app.netlify.com/) site that links with git repo of your **Obsidian Vault** 
4. push **Obsidian Vault** to git and watch it auto build