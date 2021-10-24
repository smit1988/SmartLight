# SmartLight

Here are some Home Assistant YAMLs.

Here's an automation to have a Google Nest say something in French when an ESP8266 is turned on.
```
alias: jadore le pu pu
description: ''
trigger:
  - type: connected
    platform: device
    device_id: 69420
    entity_id: binary_sensor.hacker_usb_status
    domain: binary_sensor
condition: []
action:
  - service: tts.google_say
    data:
      message: J’adore le pu pu
      language: fr
      entity_id: media_player.workshop_speaker
mode: single
```

Here's a script to summon your partner to the bedroom.
```
alias: Sexy Time
sequence:
  - repeat:
      count: '100'
      sequence:
        - type: turn_on
          device_id: 2144
          entity_id: light.light_mn_living_living
          domain: light
        - delay:
            hours: 0
            minutes: 0
            seconds: 0
            milliseconds: 200
        - type: turn_off
          device_id: 2144
          entity_id: light.light_mn_living_living
          domain: light
        - type: turn_on
          device_id: 1c13
          entity_id: light.light_up_chandelier_top_stairs
          domain: light
        - delay:
            hours: 0
            minutes: 0
            seconds: 0
            milliseconds: 200
        - type: turn_off
          device_id: 1c13
          entity_id: light.light_up_chandelier_top_stairs
          domain: light
        - type: turn_on
          device_id: 1e48
          entity_id: light.light_up_loft_top_stairs
          domain: light
        - delay:
            hours: 0
            minutes: 0
            seconds: 0
            milliseconds: 200
        - type: turn_off
          device_id: 1e48
          entity_id: light.light_up_loft_top_stairs
          domain: light
        - type: turn_on
          device_id: 1bb
          entity_id: light.light_up_bedroom_bedroom
          domain: light
        - delay:
            hours: 0
            minutes: 0
            seconds: 0
            milliseconds: 200
        - type: turn_off
          device_id: 1bbd
          entity_id: light.light_up_bedroom_bedroom
          domain: light
mode: single
```
