---
{"dg-publish":true,"tags":["media","plex","tv","movies"],"permalink":"/developer/linux/plex-manual-symlink-for-media/","dgPassFrontmatter":true}
---

Sometimes I have to manually import a movie into my media library, without the help of [[developer/Media Software/Sonarr\|Sonarr]] or [[developer/Media Software/Radarr\|Radarr]] to manually add metadata.

You may not want to rename the file from your *downloads* folder as to not break any current connections. 
## Create symlink  
`ln -s /path/to/original/file.mkv /path/to/link/file.mkv  `
  
> Don't forget file extension dummy  
## Example  
Annual awards shows, in this case The Grammy's, is technically a [TV show](https://www.thetvdb.com/series/grammy-awards/allseasons/official) that only has One Season. If you follow the `S01E67` format then [[developer/Home Lab/Plex.tv\|Plex.tv]] will play nice with the file and grab the correct metadata. 

```  
ln -s /mnt/octo8/media/imports/manual/The.67th.Annual.Grammy.Awards.2025.720p.WEBRip.6CH.x265.HEVC-PSA.mkv "/mnt/octo8/media/library/tv/Grammy Awards/Grammy.Awards.S01E67.mkv"  
```  
  
## Credits  
- https://www.howtogeek.com/287014/how-to-create-and-use-symbolic-links-aka-symlinks-on-linux/
- https://www.thetvdb.com/series/grammy-awards/allseasons/official
- https://www.reddit.com/r/PleX/comments/7tw32o/how_do_i_add_something_like_grammy_into_plex_and/