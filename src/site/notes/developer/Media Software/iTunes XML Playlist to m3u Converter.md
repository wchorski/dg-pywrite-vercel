---
{"dg-publish":true,"permalink":"/developer/Media Software/iTunes XML Playlist to m3u Converter/","dgPassFrontmatter":true}
---

using this [java](http://www.ericdaugherty.com/dev/itunesexport/scala/)cli app, I'm able to convert playlists living in the `iTunes Library.xml` file into separate `*.m3u` playlist files

> [!info] New Apple Music App
> for those using the new Music app, check out [[developer/MacOs/Apple Music Batch Playlist Export\|Apple Music Batch Playlist Export]]. But you can still follow this method, but it assumes you export your library via "File -> Library -> Export Library" to generate the XML file
## Convert playlist file types

this is just a shell wrapper that runs the `itunesexport.jar` file downloaded from Eric's website

`javacommand.sh`
```bash
#! /bin/bash

java -jar itunesexport.jar \
  -fileTypes=ALL -library="/mnt/octo8/jellyfin/media/iMusicLibrary/Library.xml" \
  -outputDir="/mnt/octo8/jellyfin/media/iMusicLibrary/playlists"  \
  -includePlaylist="\
    000_School_House_rock,\
	WERK_MUSIC,\
	white_kid_music,\
	shyGarage,\
	Nerd_Alert,\
  " 
```

if coming from **iTunes** your file will be `iTunes Library.xml`. The default XML export from **Apple Music** is `Library.xml`

## Absolute to Relative Paths

The file paths to each song will be an absolute path from the host computer. Unless your 2nd machine's directories are set up exactly like the 1st, you'll want to edit the file paths so your new media machine can find the files

`m3u-relativepaths.sh`
```bash
#! /bin/bash 

PLAYLIST_DIR="/Users/USERNAME/Downloads/playlists"
ABSOLUTE_PATH="file:///Volumes/DRIVE/MUSIC_DIRECTORY"

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

you'll want to edit 2 things on this file
1.  Two spots where you target your directory `/mnt/octo8/jellyfin/media/iMusicLibrary/playlists-m3u`
3.  Absolute path of 1st computer directory `file:///Volumes/DRIVE/MUSIC_DIRECTORY` is what is being replaced with  `..` to be a relative path. If your coming from Windows, it may look like `E:/MUSICLIBRARY` 

Your iTunes folder will look like this

![attachments/iTunes-Directory-Tree.png|iTunes-Directory-Tree](/img/user/attachments/iTunes-Directory-Tree.png)

I created another folder `playlists` that will house all my new `.m3u` files. And that relative path `../` will start looking for songs one directory above the `playlists` folder

Replace `USERNAME` & `DRIVE` with paths that are unique to your machine. Also, this assumes `/Volumes/DRIVE` won't clash with any of your music's file structure i.e. *artist*, *album* or *filename* that may have `/Volumes/DRIVE`. If in your library the rock band [Volumes](https://volumesofficial.com/)may have an album titled the same as your drive's name. Very unlikely, but if it did happen, it would replace that text and break the path.

Notice how I replace the *absolute path* with a `..`. This is because I nest the `.m3u` files in it's own `playlist` folder. Think of it as, the path to where the `.m3u` lives, it must go up one directory so it can see the `./iTunes Media/Music/...` library.
## Import to Jellyfin
I use [[developer/Home Lab/Jellyfin\|Jellyfin]] as my online music manager. Note that this is a one way sync. All songs and playlists must be added and edited with **iTunes** and then updated to Jellyfin. 

Cool thing about **Jellyfin** is that plopping `.m3u` files into the imported library will auto find and add those playlists to the frontend. Just make sure those relative paths are GOOD

## Phone
I use [Symfonium](https://www.symfonium.app/)as my phone's dedicated music player. It cost **$5**, and that's fine because it is very feature rich (Android Auto, auto sync, Offline downloads, etc). If you're looking for something light weight (and [[developer/FOSS\|FOSS]]) check out [jmshrv/finamp: A Jellyfin music client for mobile (github.com)](https://github.com/jmshrv/finamp)

## Improvements
With the new Apple Music app, you still have to manually go through and "File -> Library -> Export Library" anytime you make changes. I'm sure I could run an `.applescript` to do this regularly, but for now I'll do a few clicks

How do I get my music files from laptop to server? With [[developer/Home Lab/FreeFileSync\|FreeFileSync]]. Maybe in the future I'll just use [[developer/Linux/rsync\|rsync]]. 

> [!note] Ignore Folders
> Don't forget to ignore the `*/playlists` folder during this sync as you don't want to overwrite those `.m3u` files


---

## Credits
- [Import / Sync media providers playlists - Wiki - Symfonium support](https://support.symfonium.app/t/import-sync-media-providers-playlists/325)
- http://www.ericdaugherty.com/dev/itunesexport/scala/
- [Where is Apple's new "Music app" xml file location? · Issue #3 · jasmith79/playlistrs (github.com)](https://github.com/jasmith79/playlistrs/issues/3)
## index
- [[music/music box\|music box]]
- [[developer/developer_box📦\|developer_box📦]]