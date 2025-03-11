---
{"dg-publish":true,"permalink":"/developer/linux/bash-script-status-animation/","dgPassFrontmatter":true}
---

```bash
#!/bin/bash

anim() {
    spinner="/|\\-"
    count=0
    while true; do
        printf "\r[${spinner:$count:1}] Loading..."
        count=$(( (count + 1) % 4 ))
        sleep 0.1
    done
}
```

save as `loading_animation.sh` and make it executable via `chmod +x loading.sh`

## importing animation to other scripts

In this example I'm importing the animation script into an [[developer/Linux/rsync\|rsync]] shell script 

```shell
#!/bin/bash

# Source (import) the external script
source ./loading_animation.sh

# Start the loading animation in the background
start_loading_animation &

# Store the PID of the loading animation
loading_animation_pid=$!

# Run your rsync command
rsync -av source/ destination/

# After rsync is done, stop the loading animation
kill $loading_animation_pid

# Clear the loading animation line
printf "\r%s\n" "                        "

```

---
## Credits
- 

## Backlinks
- [03 Shell Script Spinner Animation Tutorial - YouTube](https://www.youtube.com/watch?v=93i8txD0H3Q)