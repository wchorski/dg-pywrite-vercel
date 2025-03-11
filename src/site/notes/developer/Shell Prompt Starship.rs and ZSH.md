---
{"dg-publish":true,"permalink":"/developer/Shell Prompt Starship.rs and ZSH/","dgPassFrontmatter":true}
---


> [!info] [Starship: Cross-Shell Prompt](https://starship.rs/)
> The minimal, blazing-fast, and infinitely customizable prompt for any shell!

### install on Linux Mint
[video tutorial](https://www.youtube.com/watch?v=AK2JE2YsKto)  
[other video tutorial](https://www.youtube.com/watch?v=VgTu1_92U0U&t=176s)

0. grab `zsh` if you want all the pretty icons
```shell
sudo apt-get update
sudo apt-get install zsh
zsh
# press (2)

# make zsh default shell
chsh -s $(which zsh)
chsh -s /bin/bash
```
2. installer script
```shell
curl -sS https://starship.rs/install.sh | sh
```
2. Add the following to the end of `~/.zsh`
```shell
eval "$(starship init zsh)"
```
3. add config file 
```shell
mkdir -p ~/.config && touch ~/.config/starship.toml
```

## Configuration
`./config/starship.toml`
```toml
# ~/.config/starship.toml

add_newline = false
command_timeout = 1000
format = """$os$username$hostname$kubernetes$directory$git_branch$git_status"""

# Drop ugly default prompt characters
[character]
success_symbol = ''
error_symbol = ''

# ---

[os]
format = '[$symbol](bold white) '   
disabled = false

[os.symbols]
Windows = ''
Arch = '󰣇'
Ubuntu = ''
Macos = '󰀵'

# ---

# Shows the username
[username]
style_user = 'white bold'
style_root = 'black bold'
format = '[$user]($style) '
disabled = false
show_always = true

# Shows the hostname
[hostname]
ssh_only = false
format = 'on [$hostname](bold yellow) '
disabled = false

# Shows current directory
[directory]
truncation_length = 1
truncation_symbol = '…/'
home_symbol = '󰋜 ~'
read_only_style = '197'
read_only = '  '
format = 'at [$path]($style)[$read_only]($read_only_style) '

# Shows current git branch
[git_branch]
symbol = ' '
format = 'via [$symbol$branch]($style)'
# truncation_length = 4
truncation_symbol = '…/'
style = 'bold green'

# Shows current git status
[git_status]
format = '[$all_status$ahead_behind]($style) '
style = 'bold green'
conflicted = '🏳'
up_to_date = ''
untracked = ' '
ahead = '⇡${count}'
diverged = '⇕⇡${ahead_count}⇣${behind_count}'
behind = '⇣${count}'
stashed = ' '
modified = ' '
staged = '[++\($count\)](green)'
renamed = '襁 '
deleted = ' '

# Shows kubernetes context and namespace
[kubernetes]
format = 'via [󱃾 $context\($namespace\)](bold purple) '
disabled = false

# ---

[vagrant]
disabled = true

[docker_context]
disabled = true

[helm]
disabled = true

[python]
disabled = true

[nodejs]
disabled = true

[ruby]
disabled = true

[terraform]
disabled = true
```

#### copy fonts via terminal
find fonts at [Nerd Fonts - Iconic font aggregator, glyphs/icons collection, & fonts patcher](https://www.nerdfonts.com/font-downloads)
```shell
sudo cd /usr/share/fonts
sudo wget https://github.com/ryanoasis/nerd-fonts/releases/download/v2.2.2/BigBlueTerminal.zip
sudo unzip -d BigBlueTerminal BigBlueTerminal.zip 
sudo rm BigBlueTerminal.zip 

# refresh 
sudo fc-cache -f -v
```