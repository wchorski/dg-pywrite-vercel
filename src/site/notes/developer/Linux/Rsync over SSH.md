---
{"dg-publish":true,"permalink":"/developer/linux/rsync-over-ssh/","dgPassFrontmatter":true}
---


Here is a simple backup solution for a client to remote server. Assuming you've already set up a [[developer/Linux/Hardened SSH Server\|Hardened SSH Server]] with key login

In this example I'm backing up my [[developer/Home Lab/Nextcloud\|Nextcloud]] data (pictures, documents, etc.) to a remote server for my 'offsite backup'

- `LOCALDRIVE` === assuming you mounted your external drive with lots of space here
- `REMOTESERVER.lan` === could be an offsite domain or you could just backup to a local IP on the same network
- `LOG_DIR` === this is where the `status.log` & `sync.log` will be created and updated
	- `status.log` is a simple 'OK' or 'ERROR' with date synced. Great to print to dashboards for a quick glance
	- `sync.log` is the full details of the last [[developer/Linux/rsync\|rsync]] run. Useful if the script ERRORs out and you have to see what went wrong.

```bash
#!/bin/bash

# Variables
TARGET="nextcloud"
SOURCE_DIR="/mnt/LOCALDRIVE/${TARGET}"
DEST_USER="REMOTEUSER"
DEST_HOST="REMOTESERVER.lan"
## directory is inclusive, it will also copy folder along with it
DEST_DIR="/mnt/REMOTESTORAGE"

LOG_DIR="/home/LOCALUSER/scripts/backups/${TARGET}"
NOW=`date +'%Y-%m-%d_%H%M%S'`
LOG_FILE="${LOG_DIR}/sync.log" 
STATUS_FILE="${LOG_DIR}/status.log"

echo "$NOW \n" > "$LOG_FILE"

# Rsync command
rsync -avzP -e ssh "$SOURCE_DIR" "${DEST_USER}@${DEST_HOST}:${DEST_DIR}" >> "$LOG_FILE" 2>&1

RSYNC_EXIT_CODE=$?

LINECOUNT=$(wc -l sync.log)

# Update status based on exit code
if [ $RSYNC_EXIT_CODE -eq 0 ]; then
    echo "OK $NOW $LINECOUNT" > "$STATUS_FILE"
else
    echo "ERROR $NOW $LINECOUNT" > "$STATUS_FILE"
fi
```

## Troubleshooting

I'm getting 2 different errors when trying to sync my nextcloud data files
1. `could not make way for new regular file: /FILEPATH`
2. `cannot delete non-empty directory: /FILEPATH`

---
## Backlinks
- [[developer/Linux/rsync\|rsync]]