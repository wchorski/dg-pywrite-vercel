---
{"dg-publish":true,"tags":["homeassistant","automation","template"],"permalink":"/developer/home-assistant/home-assistant-wake-up-alarm-trigger-automation/","dgPassFrontmatter":true}
---

This assumes you've already set up the companion [[developer/Home Lab/Home Assistant\|Home Assistant]] app to your phone an enabled the `Alarm Sensors`

## Testing in Editor
Utilize the template playground editor to understand how to create a template. http://YOUR_HA_IP:8123/developer-tools/template

```jinja
isAlarmRinging: {{now().strftime("%a %h %d %H:%M %Z %Y") ==
              (((state_attr('sensor.phone_next_alarm', 'Time in Milliseconds') |
           int / 1000)) | timestamp_custom('%a %h %d %H:%M %Z %Y'))}}

{# extra examples to understand above code #}
{% set timeNow = now().strftime("%a %h %d %H:%M %Z %Y") %}
{% set myAlarm = (state_attr('sensor.phone_next_alarm', 'Time in Milliseconds') | int / 1000) | timestamp_custom('%a %h %d %H:%M %Z %Y')  %}
timeNow: {{ timeNow }}
myAlarm: {{ myAlarm }}
```

The output 'Result' should look like
```txt
isAlarmRinging: False

timeNow: Wed Oct 02 13:17 CDT 2024
myAlarm: Thu Oct 03 06:12 CDT 2024
```

> [!tip] be sure to replace `phone_next_alarm` with the unique name of your phone sensor

### On Your Phone
You should be able to set the alarm and watch the `myAlarm` update in real time. When the `timeNow` and `myAlarm` match you should see `isAlarmRinging: True` just for a split second. 

You may miss it with the naked eye, but the computer will recognize it as a trigger for automation.

## Configuration.yaml
Now e can move on to creating the sensor in `.yml`. I've code split my templates into a seperate `template.yaml`. 

```yml
## configuration.yaml
template: !include template.yaml
```

```yml
## template.yaml
- sensor:
    - name: "isAlarmRinging_myphone"
      # state: false
      state: >
        {{now().strftime("%a %h %d %H:%M %Z %Y") ==
          (((state_attr('sensor.phone_next_alarm', 'Time in Milliseconds') |
           int / 1000)) | timestamp_custom('%a %h %d %H:%M %Z %Y'))}}
```

don't forget to change `phone_next_alarm ` and `isAlarmRinging_myphone` to your needs

## Home Assistant
Move to the HA UI in your browser to restart home assistant. Confirm the sensor was created at http://hYOUR_HA_IP:8123/config/entities and make sure it's showing as `False`

![attachments/Pasted image 20241002132658.png](/img/user/attachments/Pasted%20image%2020241002132658.png)

### Automation
The automation I've set up with this alarm turns on my lights for my wake up morning routine.

![attachments/Pasted image 20241002132811.png](/img/user/attachments/Pasted%20image%2020241002132811.png)

This routine logic:
1. Trigger when current time equals alarm time (When `isAlarmRinging_myphone` goes `False` to `True`)
2. and if time is between 5am - 9am
3. activate wake up scene

---
## Credit
- https://community.home-assistant.io/t/adding-multiple-sensors-values-into-one/329677