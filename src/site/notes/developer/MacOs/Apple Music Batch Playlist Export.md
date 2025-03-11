---
{"dg-publish":true,"tags":["Apple","script","automation","music","bash"],"permalink":"/developer/mac-os/apple-music-batch-playlist-export/","dgPassFrontmatter":true}
---

I have a lot of iTunes... I mean Apple Music playlists that I'd like to export as `.m3u`. I wanted a way to have a select few playlists to export, not export every single playlist

> [!note] using old XML conversion tool
> This method below is a bit convoluted. I'd recommend trying out my [[developer/Media Software/iTunes XML Playlist to m3u Converter\|iTunes XML Playlist to m3u Converter]] directions first.
## Set Key Shortcut
The below script relies on a key shortcut **⌘E** to the `Export Playlist…` in **System Preferences** > **Keyboard** > **Shortcuts** > **App Shortcuts** for the **Music.app**

## Select Playlists by Name
create an applescript file `nano playlist-export-automation.scpt`

I manually set the `playlistArray` to an array of strings. 
```applescript
log "--- Start Playlist Export ---"

activate application "Music"
delay 1

tell application "Music"
	set playlistArray to {"bangersonly", "playlist2", "playlist3"}
	log "Selected Playlists"
	log playlistArray
	set allPlaylistArray to (get name of every playlist)
	log "All Playlists"
	log allPlaylistArray
	
	# Foreach playlist...
	repeat with currPlaylist in playlistArray
		
		if playlistArray contains currPlaylist then
			say currPlaylist
			tell application "Music"
				# Set the current playlist in the sidebar
				set the view of the front browser window to playlist (currPlaylist as string)
				delay 1
				
				# Start export
				tell application "System Events"
					keystroke "e" using command down # it's necessary to have set the shortcut CMD+E
					delay 2 # delay in seconds
					key code 36 # simulation return key
					delay 2
				end tell
				
			end tell
			
			delay 0.5
		end if
		
	end repeat
end tell

log "--- Playlist Export Complete ---"
```

First time you run this script, you be asked to allow Accessibility through settings. Allow this. `System Preferences -> Security & Privacy -> Accessibility` and flip the **Script Editor** toggle

> [!info] Folders
the line `set allPlaylistArray to (get name of every playlist)` grabs playlist folders as there name too. If you have any overlap between folder names and playlist names, I could see this being a problem

## Run the Script
you can run the script in the command line `osascript ./playlist-export-automation.scpt` or you could build and test the script directly in Apple's **Script Editor** app

## Absolute Path to Relative Path
If you poke into one of those `.m3u` files, you see that the paths are absolute. You'll want to make those a relative path to your music file so that [[developer/Home Lab/Jellyfin\|Jellyfin]] or other music software can find the files if you plan to move or copy your music library to a different machine

```bash
#! /bin/bash 

PLAYLIST_DIR="/Users/USERNAME/Downloads/playlists"
ABSOLUTE_PATH="/Volumes/DRIVE/MUSIC_DIRECTORY"

cd "${PLAYLIST_DIR}"
PLAYLISTS=(*.m3u)

for ((i=0; i<${#PLAYLISTS[@]}; i++))
  do
	echo "------- ${PLAYLISTS[$i]} ---------" 
  
	### s| LOOK FOR | REPLACE WITH | g=find all instances
	LC_ALL=C  sed -i    "s|${ABSOLUTE_PATH}|..|g" "${PLAYLIST_DIR}/${PLAYLISTS[$i]}"

	echo "[[ SUCCESS! | SUCCESS! ]]"
done
```

Replace `USERNAME` & `DRIVE` with paths that are unique to your machine. Also, this assumes `/Volumes/DRIVE` won't clash with any of your music's file structure i.e. *artist*, *album* or *filename* that may have `/Volumes/DRIVE`. If in your library the rock band [Volumes](https://volumesofficial.com/)may have an album titled the same as your drive's name. Very unlikely, but if it did happen, it would replace that text and break the path.

Notice how I replace the *absolute path* with a `..`. This is because I nest the `.m3u` files in it's own `playlist` folder. Think of it as, the path to where the `.m3u` lives, it must go up one directory so it can see the `./iTunes Media/Music/...` library.

> [!warning] MacOS
> I tried running this bash script on MacOS and it would keep throwing an error `sed: 1: "/Users/USERNAME/Music ...": unterminated substitute in regular expression`. Since my [[developer/Home Lab/Jellyfin\|Jellyfin]] lives on a [[developer/Linux/Linux\|Linux]] server, I do all the conversion my linux machine so this is a non issue to me.

## Chat GPT Example
I asked ChatGPT how to check if a string is found in an array

```applescript
set myArray to {"apple", "banana", "orange"}
set myString to "banana"

if myArray contains myString then
    return "The array contains the string."
else
    return "The array does not contain the string."
end if

```


## improvements
a few improvements I may want to make in the future...

### Delay
waiting x amount of seconds to run the next export seems like a fragile solution. If there was a way to confirm that the save was completed, or maybe the save dialogue box is closed to run the next export in the loop. Maybe this [snippet](https://apple.stackexchange.com/questions/121810/waiting-until-a-window-exists-in-applescript) will help

### Save and Replace
If the playlist already exists, how do I confirm to save over the old file?

---
## Credits
- [if statement - AppleScript "if contain" - Stack Overflow](https://stackoverflow.com/questions/43780648/applescript-if-contain)
- [AppleScript Application is not allowed to send keystrokes - Stack Overflow](https://stackoverflow.com/questions/54973241/applescript-application-is-not-allowed-to-send-keystrokes)
- [Waiting until a window exists in Applescript? - Ask Different (stackexchange.com)](https://apple.stackexchange.com/questions/121810/waiting-until-a-window-exists-in-applescript)
- [[developer/MacOs/index\|index]]