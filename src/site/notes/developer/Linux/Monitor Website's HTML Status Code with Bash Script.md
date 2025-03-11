---
{"dg-publish":true,"tags":["linux","bash","automation","webdev"],"permalink":"/developer/linux/monitor-website-s-html-status-code-with-bash-script/","dgPassFrontmatter":true}
---

Monitor your websites in bulk by using `curl` on the page and returning the status code 
- `200` - good
- `302` - good redirect
- `500` - server side error
- `000` - website or DNS is down

```bash
#!/bin/bash

cd ~/html-status-codes

declare -a WEBSITES
WEBSITES+=( 
  "https://make-a-gram.williamusic.com"
  "https://www.tawtaw.site/home"
  "https://cutefruit.tawtaw.site/home" 
  "https://partyvibeonline.tawtaw.site/home"
  "http://prowlarr.lan"
  "http://dupdash.lan"
  "https://150events-ks.tawtaw.site/home"
  "https://blesspress.williamusic.com"
)

# commented code is for monitoring in a single document
# printf '%(%Y-%m-%d %H:%M:%S)T\n' -1 >> all-codes.log

for website in "${WEBSITES[@]}"
do
  # curl -o /dev/null -s -w "%{http_code} " $website >> all-codes.log
  # echo "$website" >> all-codes.log

  # remove any bad chars for filenames
  FILENAME=$(sed -E 's/(http|https):\/\///g' <<< $website | sed 's/\//_/g')
  test -e "./logs/$FILENAME.log" || touch ./logs/$FILENAME.log
  curl -o /dev/null -s -w "%{http_code}" $website > "./logs/$FILENAME.log"
done

# echo "---" >> all-codes.log
```

This will create a file for each website that houses the latest code returned. If you want a single file that houses the log with timestamp, uncomment out the other snippets. 

## Cron

Run this regularly with a `crontab -e` config every 15 minutes

```shell
*/15 * * * * ~/html-status-codes/html-code-monitor.sh
```

## Home Assistant Sensor

I leverage [[developer/Home Lab/Home Assistant\|Home Assistant]] as a way to track history and send alerts. Here is how to set up the sensor

```yml
sensor:
	- platform: template
	  sensors:
	    html_code_makeagram:
	      friendly_name: HTML Code Make a Gram
	      value_template: "{{ int(value,0)}}"
	      icon_template: >-
	        {% if int(value,0) == 0 %}
	          mdi:server-off
	        {% elif int(value,0) > 199 and int(value,0) < 400 %}
	          mdi:check-circle
	        {% elif int(value,0) > 499 %}
	          mdi:alert-outline
	        {% else %}
	          mdi:help-rhombus-outline
	        {% endif %}
```

## Credit
- [Creating Template Sensor - Configuration - Home Assistant Community (home-assistant.io)](https://community.home-assistant.io/t/creating-template-sensor/273724)
- https://stackoverflow.com/questions/6852951/use-sed-to-replace-all-backslashes-with-forward-slashes
- https://stackoverflow.com/questions/4014074/how-to-read-output-of-sed-into-a-variable
- https://www.unix.com/shell-programming-and-scripting/66986-if-file-not-exist-create-new-one.html

- [[developer/Linux/Linux\|Linux]]
- [[developer/developer_box📦\|developer_box📦]]