---
{"dg-publish":true,"permalink":"/developer/linux/bash-read-a-file/","dgPassFrontmatter":true}
---

I was trying to extract a single line from a [[developer/Home Lab/Duplicati\|Duplicati]] log to show on a monitor dash. Here is the shell script and example text log I was trying to read

`readfile.sh`
```bash
#!/bin/bash

read -p "Enter the file name : " file
read -p "Enter the string to search for in the file : " str

grep "^$str" $file | { grep -v grep || true; }
```

`backup.log`
```txt
- Computer | icicle Root:
    ID: '1'
    Last run:
      Duration: 00:05:46
      Started: 22/08/2022
      Stopped: 22/08/2022
    Local database: /config/DKEQAILFXU.sqlite
    Schedule:
      Last run: 22/08/2022
      Next run: 11:30 AM
      Repeat: 1M
    Size:
      Backend: 4.15 GB
      Local: 3.07 GB
    Versions: 3
Stopped: not the real one
```


---
## Credits 
- 

## Backlinks
- [[developer/Linux/Linux\|Linux]]