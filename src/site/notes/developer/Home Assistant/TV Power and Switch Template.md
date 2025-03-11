---
{"dg-publish":true,"tags":["homeassistant","yaml"],"permalink":"/developer/home-assistant/tv-power-and-switch-template/","dgPassFrontmatter":true}
---

A configuration for [[developer/Home Lab/Home Assistant\|Home Assistant]]

I wanted to create a card that reacts to the TV's power state. 
1. When the TV is on --> show TV controls
2. When TV is off --> hide TV controls
2. When TV is changing state --> show transition activity animation (w color code)
3. TV controls: 
	1. an input source switcher (radio style buttons)
	2. volume

## Configuration

`configuration.yml`
```yaml
switch:

  - platform: template
    scan_interval: 90000
    switches:
      moeits_conf_tv_back_wireless_power:
        value_template: >
          {% if is_state("script.moeits_tv_back_off", "on") %}
            off
          {% elif is_state("script.moeits_tv_back_on", "on") %}
            on
          {% elif is_state("media_player.moeits_tv_back_wireless", "on") %}
            on
          {% else %}
            off
          {% endif %}
        turn_on:
          - service: script.turn_on
            data:
              entity_id: script.moeits_tv_back_on
        turn_off:
          - service: script.turn_on
            data:
              entity_id: script.moeits_tv_back_off
        icon_template: >-
          {% if is_state("script.moeits_tv_back_off", "on") %}
            mdi:timer-sand
          {% elif is_state("script.moeits_tv_back_on", "on") %}
            mdi:timer-sand
          {% elif is_state("media_player.moeits_tv_back_wireless", "on") %}
            mdi:television
          {% else %}
            mdi:television-off
          {% endif %}
```

### Helper variable

Needed a helper variable that holds the state of the current **Input Source** via `input_text.moeits_conf_tv_back_source`

### Scripts

Scripts handle the transition as well as make user feedback more robust.
1. `script.turn_on_tv` 
	1. send shell command to turn TV on via [HDMI-CEC](https://www.lifewire.com/hdmi-cec-4158343)
	2. wait `10 seconds` (buffer to smoothly transition UI)
	
2. `script.turn_off_tv`
	1. send shell command to turn TV off via [HDMI-CEC](https://www.lifewire.com/hdmi-cec-4158343)
	2. wait `10 seconds` (buffer to smoothly transition UI)
	
3. `script.tv-setSource--ScreenBeam`
	1. set our **helper** to the current input state
	2. change input of 'media_player'
```yml
alias: MOEITS-Conf-TV-Back--setSource-ScreenBeam
sequence:
  - service: input_text.set_value
    data:
      value: ScreenBeam
    target:
      entity_id: input_text.moeits_conf_tv_back_source
  - service: media_player.select_source
    data:
      source: ScreenBeam
    target:
      entity_id: media_player.moeits_tv_back_wireless
mode: single
icon: mdi:hdmi-port

```

4. `script.tv-setSource--Bulletin`
	1. set our **helper** to the current input state
	2. change input of 'media_player'
```yml
alias: MOEITS-Conf-TV-Back--setSource-Bulletin
sequence:
  - service: input_text.set_value
    data:
      value: Bulletin
    target:
      entity_id: input_text.moeits_conf_tv_back_source
  - service: media_player.select_source
    data:
      source: Bulletin
    target:
      entity_id: media_player.moeits_tv_back_wireless
mode: single
icon: mdi:hdmi-port

```

## lovelace Card
```yml
type: vertical-stack
cards:
  - type: vertical-stack
    cards:
      - type: entities
        entities:
          - entity: switch.moeits_conf_tv_back_wireless_power
            icon: ''
            name: Back TV
  - type: conditional
    conditions:
      - entity: media_player.moeits_tv_back_wireless
        state: 'on'
    card:
      type: vertical-stack
      cards:
        - type: custom:mini-media-player
          entity: media_player.moeits_tv_back_wireless
          group: false
          source: full
          sound_mode: full
          info: short
          name: ' '
          volume_stateless: false
          toggle_power: false
        - type: horizontal-stack
          cards:
            - type: custom:button-card
              entity: input_text.moeits_conf_tv_back_source
              name: ScreenBeam
              icon: mdi:hdmi-port
              tap_action:
                action: call-service
                service: script.moeits_conf_tv_back_source_screenbeam
                service_data:
                  entity_id: input_text.moeits_conf_tv_back_source
              show_state: false
              styles:
                icon:
                  - color: |
                      [[[
                        if (entity.state == 'ScreenBeam') return 'lightgreen';
                        else return 'grey';
                      |[
                        if (entity.state == 'ScreenBeam') return 'lightgreen';
                        else return 'grey';
                      ]]]
            - type: custom:button-card
              entity: input_text.moeits_conf_tv_back_source
              name: Bulletin
              icon: mdi:hdmi-port
              tap_action:
                action: call-service
                service: script.moeits_conf_tv_back_setsource_bulletin
                service_data:
                  entity_id: input_text.moeits_conf_tv_back_source
              show_state: false
              styles:
                icon:
                  - color: |
                      [[[
                        if (entity.state == 'Bulletin') return 'lightgreen';
                        else return 'grey';
                      |[
                        if (entity.state == 'Bulletin') return 'lightgreen';
                        else return 'grey';
                      ]]]


```

[[developer/developer_box📦\|developer_box📦]]