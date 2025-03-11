---
{"dg-publish":true,"permalink":"/developer/home-assistant/jinja-code-snippet/","dgPassFrontmatter":true}
---

Create and reuse snippets of logic with the [Jinja](https://palletsprojects.com/p/jinja/)syntax for all sorts of use cases in [[developer/Home Lab/Home Assistant\|Home Assistant]]. 

check out the [link](https://www.home-assistant.io/blog/2023/04/05/release-20234/#macros-for-your-templates) to make reusable snippets. Here is how I reuse code for the dynamic icon

`html-status-icon.jinja`
```jinja
{% macro html_status_icon(entity_id) %}

{% set value = states(entity_id) | int(0) %}
{% if value == 0 %}
  mdi:server-off
{% elif int(value) > 199 and int(value,0) < 400 %}
  mdi:check-circle
{% elif int(value) > 499 %}
  mdi:alert-outline
{% else %}
  mdi:help-rhombus-outline
{% endif %}

{% endmacro %}
```

`sensor.yaml`
```yml
- platform: template
  sensors:
    html_code_makeagram_template:
      friendly_name: HTML Code Make a Gram
      value_template: "{{ states('sensor.html_code_makeagram') }}"
      icon_template: >-
        {% from 'html-status-icon.jinja' import html_status_icon %}
        {{ html_status_icon('sensor.html_code_makeagram') }}
```