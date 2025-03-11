---
{"dg-publish":true,"tags":["linux"],"permalink":"/developer/Media Software/yt-dlp/","dgPassFrontmatter":true}
---

This is a nice library for all my 🏴‍☠️black flag waving friends

[link](https://github.com/yt-dlp/yt-dlp?tab=readme-ov-file#installation)

#bash script wrapper 

```bash
#!/bin/bash

URL=$1

yt-dlp -x --audio-format mp3 $URL
```

Simple way to run a "preset" of your most used configs

```shell
./mp3 http://www.youtube.com/watch?v=***
```

---

[[developer/Media Software/Docker Compose Media Containers Arrs\|Docker Compose Media Containers Arrs]]
## Credits
- [How to Work with Variables in Bash (howtogeek.com)](https://www.howtogeek.com/442332/how-to-work-with-variables-in-bash/)
## index
- [[developer/developer_box📦\|developer_box📦]]