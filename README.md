# SmartLight

Here are some Home Assistant YAMLs.

Here's an ESPHome yaml to a light switch that works even when it's offline.
```
esphome:
  name: light_mn_kitnook_kitnook
  platform: ESP8266
  board: esp01_1m

wifi:
  ssid: "YOUR_SSID"
  password: "YOUR_PASSWORD"

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Light Mn Kitnook Kitnook"
    password: "PASSWORD"

captive_portal:

# Enable logging
logger:

# Enable Home Assistant API
api:

ota:
binary_sensor:
  - platform: gpio
    pin:
      number: GPIO2
      mode: INPUT_PULLUP
      inverted: True
    id: button_1
    on_press:
      then:
        - light.toggle: light_1

  - platform: status
    name: "light_mn_kitnook_kitnook"

output:
  - platform: gpio
    pin:
      number: GPIO0
      inverted: True
    id: relay_1

light:
  - platform: binary
    name: "light_mn_kitnook_kitnook"
    id: light_1
    output: relay_1
```

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

And here's a Node-RED flow. This was used for the konami code on the light switches. 
The logic for setting up the light switch code actually worked pretty well. If you look at the debug tab you can see a variable incrementing by one every time you click the next button correctly. When an incorrect button is pressed, the sequence is broken and the variable goes back to zero. The chromecast to play the video is nearly useless. It seems to only be able to play videos from the internet, and only when it is a direct link to the video file itself. So that means no youtube, no google drive, no onedrive, no dropbox. I wasn't able to figure out how to play a local video. My jank solution was to upload a video to every internet storage medium I could find until one was generous enough to provide a direct link to the .mp4. I ended up on a 24 hour file sharing site. I.e. this would only open the video for 24 hours and then the link would break. Hey, good enough for a youtube video.

```
[{"id":"d3184cf3.dce22","type":"tab","label":"Flow 1","disabled":false,"info":""},{"id":"832ddea.1d9f52","type":"debug","z":"d3184cf3.dce22","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":430,"y":380,"wires":[]},{"id":"589b107e.2c733","type":"server-state-changed","z":"d3184cf3.dce22","name":"Light Switch","server":"bac52cc5.38513","version":3,"exposeToHomeAssistant":false,"haConfig":[{"property":"name","value":""},{"property":"icon","value":""}],"entityidfilter":"light.light_mn_living_living","entityidfiltertype":"exact","outputinitially":false,"state_type":"str","haltifstate":"","halt_if_type":"str","halt_if_compare":"is","outputs":1,"output_only_on_state_change":true,"for":0,"forType":"num","forUnits":"minutes","ignorePrevStateNull":false,"ignorePrevStateUnknown":false,"ignorePrevStateUnavailable":false,"ignoreCurrentStateUnknown":false,"ignoreCurrentStateUnavailable":false,"outputProperties":[{"property":"payload","propertyType":"msg","value":"","valueType":"entityState"},{"property":"data","propertyType":"msg","value":"","valueType":"eventData"},{"property":"topic","propertyType":"msg","value":"","valueType":"triggerId"}],"x":130,"y":400,"wires":[["a767e546.321cd8"]]},{"id":"a767e546.321cd8","type":"function","z":"d3184cf3.dce22","name":"","func":"var count=flow.get('count') || 0;\nif((count==0)||(count==1)||(count==5)||(count==7)||(count==9)||(count==10)){\n    count+=1;\n}else{//failed the sequence\n    count=0;\n}\nmsg.payload=count;\nflow.set('count',count);\nreturn msg;\n//up,up,down,down,left,right,left,right,b,a,start\n//light,light,fan,fan,fan,light,fan,light,fan,light,light\n//count = 0, looking to a light to progress\n//1: looking for a light to progress\n//2: fan\n//3:fan\n//4:fan\n//5:light\n//6:fan\n//7:light\n//8:fan\n//9:light\n//10:light and then trigger something","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":280,"y":300,"wires":[["832ddea.1d9f52","f78e45e6.cc5f78"]]},{"id":"91df8be6.6086a8","type":"debug","z":"d3184cf3.dce22","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":430,"y":540,"wires":[]},{"id":"bd62b005.524d6","type":"server-state-changed","z":"d3184cf3.dce22","name":"Fan Switch","server":"bac52cc5.38513","version":3,"exposeToHomeAssistant":false,"haConfig":[{"property":"name","value":""},{"property":"icon","value":""}],"entityidfilter":"light.fan_mn_living_living","entityidfiltertype":"exact","outputinitially":false,"state_type":"str","haltifstate":"","halt_if_type":"str","halt_if_compare":"is","outputs":1,"output_only_on_state_change":true,"for":0,"forType":"num","forUnits":"minutes","ignorePrevStateNull":false,"ignorePrevStateUnknown":false,"ignorePrevStateUnavailable":false,"ignoreCurrentStateUnknown":false,"ignoreCurrentStateUnavailable":false,"outputProperties":[{"property":"payload","propertyType":"msg","value":"","valueType":"entityState"},{"property":"data","propertyType":"msg","value":"","valueType":"eventData"},{"property":"topic","propertyType":"msg","value":"","valueType":"triggerId"}],"x":120,"y":560,"wires":[["4bf374cf.50bf3c"]]},{"id":"4bf374cf.50bf3c","type":"function","z":"d3184cf3.dce22","name":"","func":"var count=flow.get('count') || 0;\nif((count==2)||(count==3)||(count==4)||(count==6)||(count==8)){\n    count+=1;\n}else{//failed the sequence\n    count=0;\n}\nmsg.payload=\"F2 \"+msg.payload+\" \"+count;\nflow.set('count',count);\nreturn msg;\n//up,up,down,down,left,right,left,right,b,a,start\n//light,light,fan,fan,fan,light,fan,light,fan,light,light\n//count = 0, looking to a light to progress\n//1: looking for a light to progress\n//2: fan\n//3:fan\n//4:fan\n//5:light\n//6:fan\n//7:light\n//8:fan\n//9:light\n//10:light and then trigger something","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":280,"y":460,"wires":[["91df8be6.6086a8"]]},{"id":"98864dba.7fc43","type":"cast-to-client","z":"d3184cf3.dce22","name":"","url":"https://storage.cloudconvert.com/tasks/0fdf8f66-14ce-49a7-86bf-0f01de6dca87/beginning.mp4?AWSAccessKeyId=cloudconvert-production&Expires=1632869442&Signature=buOAve1mp5S10UYamqa96OC216o%3D&response-content-disposition=inline%3B%20filename%3D%22beginning.mp4%22&response-content-type=video%2Fmp4","contentType":"video/mp4","message":"","language":"en","ip":"192.168.1.8","port":"8009","volume":"","x":700,"y":180,"wires":[[]]},{"id":"f78e45e6.cc5f78","type":"switch","z":"d3184cf3.dce22","name":"","property":"payload","propertyType":"msg","rules":[{"t":"eq","v":"11","vt":"str"}],"checkall":"true","repair":false,"outputs":1,"x":500,"y":320,"wires":[["37f00ce5.30de24","7199b8e4.e1c268"]]},{"id":"37f00ce5.30de24","type":"debug","z":"d3184cf3.dce22","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":650,"y":320,"wires":[]},{"id":"7199b8e4.e1c268","type":"api-call-service","z":"d3184cf3.dce22","name":"Announce on speaker","server":"bac52cc5.38513","version":3,"debugenabled":false,"service_domain":"tts","service":"google_say","entityId":"media_player.workshop_speaker","data":"{\"language\":\"en-us\",\"message\":\"{{Hello, Noah. I see you're trying to show off to guests, again. Let me help with that. Also stop messing with the lights.}}\"}","dataType":"jsonata","mergecontext":"","mustacheAltTags":false,"outputProperties":[],"queue":"none","x":540,"y":100,"wires":[["59ade224.d98c1c"]]},{"id":"7da83eb.0cad7c","type":"inject","z":"d3184cf3.dce22","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":210,"y":200,"wires":[["159d5310.c9b43d"]]},{"id":"59ade224.d98c1c","type":"delay","z":"d3184cf3.dce22","name":"","pauseType":"delay","timeout":"6","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":720,"y":100,"wires":[["98864dba.7fc43"]]},{"id":"159d5310.c9b43d","type":"delay","z":"d3184cf3.dce22","name":"","pauseType":"delay","timeout":"6","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":320,"y":80,"wires":[["7199b8e4.e1c268"]]},{"id":"bac52cc5.38513","type":"server","name":"Home Assistant","version":1,"addon":true,"rejectUnauthorizedCerts":true,"ha_boolean":"y|yes|true|on|home|open","connectionDelay":true,"cacheJson":true}]
```
