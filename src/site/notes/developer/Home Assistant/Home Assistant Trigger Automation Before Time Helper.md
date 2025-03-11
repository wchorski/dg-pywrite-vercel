---
{"dg-publish":true,"tags":["homeassistant","automation","yaml"],"permalink":"/developer/home-assistant/home-assistant-trigger-automation-before-time-helper/","dgPassFrontmatter":true}
---


Create a [link](https://www.home-assistant.io/docs/automation/trigger/#template-trigger).

  ```yml
  - platform: template     
	value_template: "{{ now().timestamp() | timestamp_custom('%H:%M') == (state_attr('input_datetime.whatever', 'timestamp') - 1800) | timestamp_custom('%H:%M', false) }}"`
```

How it works:

It compares the current time to the `input_datetime`'s time less 30 minutes (1800 seconds).

This part simply reports the current time in HH:MM format:

`now().timestamp() | timestamp_custom('%H:%M')`

This part takes the `timestamp` attribute of your input_datetime entity, subtracts 1800 seconds, and converts it to HH:MM format.

`(state_attr('input_datetime.whatever', 'timestamp') - 1800) | timestamp_custom('%H:%M', false)`

It compares the two results and triggers when they are equal. The template updates every minute because it contains the `now()` function.


> [!note] The assumption here is that the `input_datetime` is time-only.


---

## Credits
- [Trigger an automation BEFORE the time of a time helper - Configuration - Home Assistant Community (home-assistant.io)](https://community.home-assistant.io/t/trigger-an-automation-before-the-time-of-a-time-helper/236667/2)
- https://community.home-assistant.io/t/trigger-an-automation-before-the-time-of-a-time-helper/236667/3?u=billsky
## index
- [[developer/developer_box📦\|developer_box📦]]
- [[developer/Home Lab/Home Assistant\|Home Assistant]]