---
{"dg-publish":true,"permalink":"/developer/home-assistant/home-assistant-outlook-imap-with-subject-sensor/","dgPassFrontmatter":true}
---


Integrating a Outlook IMAP sensor with [[developer/Home Lab/Home Assistant\|Home Assistant]] was a bit convoluted. So here is a step by step of my setup.

## Add the integration
I assume you know how to add a new Integration already. Search for "IMAP". fill in the details as such

- Server: `outlook.office365.com`
- Port: `993`
- Username: Your full email address
- Password: Your password
- Charset: `US-ASCII`
- Folder: keep as 'INBOX' if you want to check everything. In my case I'm using it to track deliveries, so I'm only checking a folder I created called 'deliveries'
- IMAP search: 'UnSeen UnDeleted'. I assume this will be what most people want.

## Subject Sensor
This newly created sensor is essentially an **integer** that tracks how many 'UnSeen UnDeleted' are in the 'deliveries' folder. Any time I check the email and 'read' it, this number will tick back down to `0`. But How do we get more data from the sensor like **Subject**. **Message Body**, **Sender Email Address**, etc? There is a way to configure **Templates** in the 'Helpers' tab, but any time I have to do more conditional stuff, editing the `configuration.yaml` is just easier.

`configuration.yaml`
```yml
template: 
	- trigger:
	    - platform: event
	      event_type: "imap_content"
	      id: "custom_event"
	  sensor:
	    - name: "Last Delivered Package"
	      state: >
	        {{ trigger.event.data["subject"] }}
```

## Conditional Card
And for the final knot to tie this together. A conditional card that pops up on the dashboard only if the 'deliveries' folder has 1 or more unread email

```yml
type: conditional
conditions:
  - condition: numeric_state
    entity: sensor.imap_wc_deliveries_outlook_com
    above: 0
card:
  type: vertical-stack
  cards:
    - type: entities
      entities:
        - entity: sensor.last_delivered_package
        - entity: sensor.imap_wc_deliveries_outlook_com
      state_color: false
      header:
        type: picture
        image: https://clipground.com/images/parcel-png-9.png
        tap_action:
          action: none
        hold_action:
          action: none

```

![parcel|400](https://clipground.com/images/parcel-png-9.png)
Just for fun, I've even added a header image to make it super obvious

## 3rd party alternatives
check out [RogerSelwyn/O365-HomeAssistant: Office 365 integration for Home Assistant (github.com)](https://github.com/RogerSelwyn/O365-HomeAssistant) if you want a more integrated Outlook API
## Credits
- [IMAP - Home Assistant (home-assistant.io)](https://www.home-assistant.io/integrations/imap)
- [Template - Home Assistant (home-assistant.io)](https://www.home-assistant.io/integrations/template/)
- [POP, IMAP, and SMTP settings for Outlook.com - Microsoft Support](https://support.microsoft.com/en-us/office/pop-imap-and-smtp-settings-for-outlook-com-d088b986-291d-42b8-9564-9c414e2aa040)
- [Headers & Footers for dashboard cards - Home Assistant (home-assistant.io)](https://www.home-assistant.io/dashboards/header-footer/)

[[developer/Home Lab/Home Lab 🏠\|Home Lab 🏠]]
