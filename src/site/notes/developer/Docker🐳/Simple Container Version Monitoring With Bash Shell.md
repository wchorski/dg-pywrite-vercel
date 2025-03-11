---
{"dg-publish":true,"tags":["bash","monitor","docker","docker-compose","curl"],"permalink":"/developer/Docker🐳/Simple Container Version Monitoring With Bash Shell/","dgPassFrontmatter":true}
---

## PreReqs

> [!tip] Using jq Library
> I tried this with a mix of `grep awk sed` but wasn't worth the trouble. I ended up using the widely available `jq` library to parse the **JSON**

This code assumes you pull everything from [Docker Hub Container Image Library | App Containerization](https://hub.docker.com/). I plan to support other container repos such as `ghcr.io` when I better understand how to variable this code.

I like when articles put the whole code first, so I'm doing that here. Below that I'll breakdown the thought process.

>[!tip] Packages without NAMESPACE 
> Packages like `mariadb` don't come with a namespace. In this case use `library`. Example `https://hub.docker.com/v2/namespaces/library/repositories/mariadb/tags?page_size=60`
## The Script
```bash
#!/bin/bash
REMOVABLE_WORDS=("latest" "lts" "beta" "stable" "develop" "active" "ubuntu" "apache")
cd /home/icicle/docker/
##! each item must have PREFIX, NAMESPACE, and REPO (even if it's an emty string) 
##! does not take into account anything but 'latest'. there was an error when `docker image inspect...` a `develop` image
##? may use later for `lscr.io/` or `ghcr.io/` packages
PREFIX=(      ""              ""            ""          ""                    ""           ""           ""            ""                        "")
NAMESPACES=(  "portainer"     "linuxserver" "library"   "jc21"                "photoprism" "prom"       "vaultwarden" "library"                 "library")
REPOSITORIES=("portainer-ce"  "duplicati"   "nextcloud" "nginx-proxy-manager" "photoprism" "prometheus" "server"      "wordpress"               "mariadb")
##? if empty string assumed ":latest"
LOCAL_TAGS=(  ""              ""            ":29"       ""                    ""           ""           ""            ":php8.2"                 "")

### todo
## set all the arrays above as one JSON object that is parsed with `jq`

handle_string_clean() {
  if [[ -z "$1" ]]
    then
      echo "not_found"
      ## break out of function if nothing was found in `curl`
      return 1
    fi

  IFS=' ' readarray -t original_array <<< "$1"

  # Declare an associative array (dictionary) for words to remove
  declare -A remove_dict
  for word in "${REMOVABLE_WORDS[@]}"
  do
    remove_dict["$word"]=1
  done

  filtered_array=()
  for word in "${original_array[@]}"
  do
    if [[ -z "${remove_dict["$word"]}" ]]
    then
      filtered_array+=("$word")
    fi
  done

  local longest_string=""
  ## Iterate over each string in the array and return string with most information (this isn't perfect but works for me)
  for str in "${filtered_array[@]}"
  do
    [ ${#str} -gt ${#longest_string} ] && longest_string=$str
  done

  echo "$longest_string"
}
## start with empty file, or delete previous data
# : > "./.logs/all-images.log"
date > "./.logs/all-images.log"
echo "##########" > "./.logs/all-images.log"

for i in "${!REPOSITORIES[@]}"
do
  REPO="${REPOSITORIES[$i]}"
  if [ -z "${LOCAL_TAGS[i]}" ] || [ "${LOCAL_TAGS[i]}" = ":latest" ]
  then
    # Perform action if LOCAL_TAG is empty "" or equals ":latest"
    localDigest=$(docker image inspect --format '{{index .RepoDigests 0}}' "${PREFIX[i]}${NAMESPACES[i]}/$REPO${LOCAL_TAGS[i]}" | awk -F'@' '{print $2}')
    ##? I set `page_size` to 100 as sometimes the image you're looking for either gets burried under new releases or is very old. the max is 100
    localVersions=$(      curl -s "https://hub.docker.com/v2/namespaces/${NAMESPACES[i]}/repositories/$REPO/tags?page_size=100" -H 'Content-Type: application/json' | jq -r '.results[] | select(.digest == "'$localDigest'") | .name')
  else
    localVersions=${LOCAL_TAGS[i]}
  fi

  repoLatestDigest=$(   curl -s "https://hub.docker.com/v2/namespaces/${NAMESPACES[i]}/repositories/$REPO/tags/latest" -H 'Content-Type: application/json' | jq -r '.digest')
  repoLatestVersions=$( curl -s "https://hub.docker.com/v2/namespaces/${NAMESPACES[i]}/repositories/$REPO/tags?page_size=100" -H 'Content-Type: application/json' | jq -r '.results[] | select(.digest == "'$repoLatestDigest'") | .name')

  cleanlocalVersion=$(handle_string_clean "$localVersions")
  cleanRepoLatestVersion=$(handle_string_clean "$repoLatestVersions")
  
  CLEAN_FILENAME=$(sed -E 's/(http|https):\/\///g' <<< "${NAMESPACES[i]}_$REPO" | sed 's/\//_/g')

  ## for one log file
  echo $CLEAN_FILENAME >> "./.logs/all-images.log"
  echo $cleanlocalVersion >> "./.logs/all-images.log"
  echo $cleanRepoLatestVersion >> "./.logs/all-images.log"
  echo "---" >> "./.logs/all-images.log"
  ## for single file logs | `-n` stops 2nd line from being created
  echo -n $cleanlocalVersion > "./.logs/local-version/$CLEAN_FILENAME.log"    
  echo -n $cleanRepoLatestVersion > "./.logs/repo-version/$CLEAN_FILENAME.log"
  ## for local terminal output
  echo $CLEAN_FILENAME
  echo "local : $cleanlocalVersion"
  echo "latest: $cleanRepoLatestVersion"
  echo "---"
done
```
## Explanation
The code above goes over these steps
1. define array of tags that I don't care about. We'll use this later (we want the actual version number i.e. `v2.3.1`)
2. define a few arrays that correspond to
	1. `PREFIX`: where the registry is (will support later)
	2. `NAMESPACES`: The developer (namespace)
	3. `REPOSITORIES`: the package (image, container, app)
3. define a function to be used later. This will clean up the string from unwanted words, or return `not_found` 
4. Now we get into local docker and scraping an API 
	1. `localDigest` gets the current digest of your local images
	2. `localVersions` calls the remote API to tell us what version tags correspond to that digest ID
	3. `repoLatestDigest` grabs whatever image tagged as `latest` and returns it's digest ID
	4. `repoLatestVersions` same as local, we return all tags associated to the registries digest ID
	5. We clean up the strings with unwanted tags
	6. print the output to separate `./.logs/local-version/$CLEAN_FILENAME.log` and `./.logs/repo-version/$CLEAN_FILENAME.log` respectively. 
	7. From there I'm gonna use **Home Assistant** to view and compare these version numbers

Because of the obtuse nature of tag naming, there is no good way a computer can compare and notify how out of date a package can be, but this should be a good alternative for people who want to monitor their container without an auto updater like [Watchtower](https://containrrr.dev/watchtower/)

I'll put this script to run in a `cron job` and my **Home Assistant** dashboard does the rest.
## Output
With echo logs in place, this is what your console will look like. I'm thinking either I keep the `repo` and `local` files separate, or just print 1 file like this per image.

```shell
Thu 18 Jul 2024 02:53:57 PM CDT
##########
portainer_portainer-ce
2.19.5
2.19.5
---
linuxserver_duplicati
v2.0.8.1-2.0.8.1_beta_2024-05-07-ls211
version-v2.0.8.1-2.0.8.1_beta_2024-05-07
---
library_nextcloud
:29
29.0.3-apache
---
jc21_nginx-proxy-manager
2.11.3
2.11.3
---
...
```

## Postmortem
Is there a better way to do this? Maybe. I could have simplified things by just comparing `update_date` value, but that would only tell me how old the version was, not if it was a minor or major change. There is this promising [cli hub-tool](https://github.com/docker/hub-tool), but still too new to tell.

This is a great example of making a lack luster API work for you. We have to visit the API twice per package, and we have to filter through 60 results just to get the 1 we want (`page_size=60`) since the version we want could live lower down the list. It's expensive, but it works. 
## Troubleshooting
Here is a brain dump of how I got to the above code. It's kinda a mess, but it's how I understood the who, what's, and where's

[[developer/Home Lab/Plex.tv\|Plex.tv]] spun up with docker using the image `plexinc/pms-docker:latest`.
```bash
docker inspect -f '{{ index .Config.Labels }}' plex-pms
```

result
```shell
map[com.docker.compose.config-hash:e96e81440276b8a02fec58af98c56d30d07e627cd39a8b2777aff81cf8cb2b83 com.docker.compose.container-number:1 com.docker.compose.depends_on: com.docker.compose.image:sha256:71b670b00350313ebea61b03472558a947f4858701e811ccb35f174c47897182 com.docker.compose.oneoff:False com.docker.compose.project:plex com.docker.compose.project.config_files:/home/spearmint/docker/plex/compose.yml com.docker.compose.project.working_dir:/home/spearmint/docker/plex com.docker.compose.replace:d78e27f7ea6638aa2d39d1a68b16c3fd985e9c129a2f43b3c8a072e9e26cf1fc com.docker.compose.service:plex com.docker.compose.version:2.28.1 org.opencontainers.image.created:2024-06-06T18:28:43.381Z org.opencontainers.image.description:"The Plex Media Server" org.opencontainers.image.licenses:NOASSERTION org.opencontainers.image.ref.name:ubuntu org.opencontainers.image.revision:4d68c05a62e861a4b9b87b48f6df102b62d68666 org.opencontainers.image.source:https://github.com/plexinc/pms-docker org.opencontainers.image.title:Plex Media Server org.opencontainers.image.url:https://github.com/plexinc/plex-media-server org.opencontainers.image.vendor:Plex, GmbH org.opencontainers.image.version:1.40.3.8555-fef15d30c]
```

There was a lot of discrepancy on what data was returned on inspect. So returning the whole json and dropping in a text editor to make it format helped me understand

```bash
docker inspect -f json plex-pms
```

the label I'm interested in is `org.opencontainers.image.version` with value `1.40.3.8555-fef15d30c`

```bash
docker inspect -f '{{ index .Config.Labels "org.opencontainers.image.version"}}' plex-pms
```

local image and container date created
```bash
docker image inspect --format '{{.Created }}' jellyfin/jellyfin
docker container inspect --format '{{.Created }}' jellyfin
docker inspect -f '{{ index .Config.Env 9 }}' duplicati
```

registery image date updated `last_updated`. You can open the url in a browser to see the whole json file
```bash
curl https://registry.hub.docker.com/v2/repositories/jellyfin/jellyfin/tags/latest | grep -o '"last_updated": *"[^"]*"' | sed 's/"last_updated": *"//' | sed 's/"//'
```

get latest version number from github
```bash
curl -s "https://api.github.com/repos/jellyfin/jellyfin/releases/latest" | grep -Po '"tag_name": "\K.*?(?=")'
```

### Get latest version number from docker hub

Here is an example of how I'd grab the latest version number of the [[developer/Home Lab/Duplicati\|Duplicati]] docker hub repo image. Later I'll show how to process an array of different image repos.

```bash
## search dockerhub for images taged with 'latest' and get unique digest number
repoLatestDigest=$(curl -s 'https://hub.docker.com/v2/repositories/duplicati/duplicati/tags' -H 'Content-Type: application/json' | jq -r '.results[] | select(.name == "'latest'") | .digest')

## search dockerhub again with digest number. Filter out any images that don't match
repoLatestVersions=$(curl -s 'https://hub.docker.com/v2/repositories/duplicati/duplicati/tags' -H 'Content-Type: application/json' | jq -r '.results[] | select(.digest == "'$repoLatestDigest'") | .name')

## remove any images named by keyword to only return actual numerical version number
removable_words=("latest" "beta" "stable")
for word in "${removable_words[@]}"
do
  repoLatestVersions=$(echo "$repoLatestVersions" | sed -E "s/\b${word}\b//g")
done
# Remove any extra spaces that may be left
repoLatestVersions=$(echo "$repoLatestVersions" | sed -E 's/ +/ /g' | sed -E 's/^ +| +$//g')

echo $repoLatestVersions > ./.logs/repo-version/duplicati.log
```

---
## Credits
- [LinuxServer.io | Docker Inspect](https://docs.linuxserver.io/images/docker-plex/#support-info)
- [dockerhub - Finding the actual version of latest version of docker image - Stack Overflow](https://stackoverflow.com/questions/58209785/finding-the-actual-version-of-latest-version-of-docker-image)
- [How to split a string into an array in Bash? - Stack Overflow](https://stackoverflow.com/questions/10586153/how-to-split-a-string-into-an-array-in-bash)
- [Check if a Bash array contains a value - Stack Overflow](https://stackoverflow.com/questions/3685970/check-if-a-bash-array-contains-a-value)
- [How to check if a container image exists on GHCR? · community · Discussion #26279 (github.com)](https://github.com/orgs/community/discussions/26279)
- [How can I use Docker Registry HTTP API V2 to obtain a list of all repositories in Docker Hub? - Stack Overflow](https://stackoverflow.com/questions/56193110/how-can-i-use-docker-registry-http-api-v2-to-obtain-a-list-of-all-repositories-i)