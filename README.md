
# nspanel-frickelui
![frickel-ui-home](https://user-images.githubusercontent.com/98703239/199835836-0d1642c9-849c-4640-a3dc-21c97ee9b02d.png)
![frickel-ui-mail](https://user-images.githubusercontent.com/98703239/199835869-cc616bbb-a8e3-424b-9023-10553d773237.png)
![frickel-ui-alarm](https://user-images.githubusercontent.com/98703239/199836716-34efd4a4-eabf-475e-8079-87583a08eaf5.png)
![frickel-ui-lights](https://user-images.githubusercontent.com/98703239/199835889-c11cc086-7bc7-472d-a23b-20d4dd833271.png)
![frickel-ui-shutters](https://user-images.githubusercontent.com/98703239/199835907-ce07ee27-0a61-48cb-b34b-d5aaf4590374.png)
![frickel-ui-garage](https://user-images.githubusercontent.com/98703239/199835917-9ee15e28-cadb-41f7-bebc-9f345cb1f397.png)

## Disclaimer:
I uploaded my custom NSPanel UI because some of you asked for it after watching https://www.youtube.com/watch?v=MCayBntBlqk. The project was released as is and this ui is absolutely tailored to my needs, so I'm not sure if you find it useful. If it fits for you: go for it! But I don't plan any further development or adjustments. If you want this, you have to do it yourself and use my files as a base. But be warned, working with the Nextion display is anything but straightforward and takes up a lot of time.

## Quick notes I wrote down for myself to remember (sorry, not a full guide, maybe later)
- Flash Tasmota on the NSPanel (search the net for existing tutorials)
- copy nspanel.be to Tasmota Filesystem
- adjust/copy autoexec.be to load nspanel.be 
- Start the Tasmota Console and run the following command: FlashNextion http://openhab:8080/static/fickelui.tft
- Hint: I used the openhab web server to provide the file, but you can use any webserver that tasmota can reach. The Reason you need a Webserver is, that the file is to large to copy on the esp32, so it has to be streamed chunk by chunk when flashing the nextion display
- if the flash process hangs, restart and try again (the display was then oddly rotated by 90°, but normal again after flashing)
- Create a MQTT thing in openhab with 3 channels:
```
UID: mqtt:topic:aba3b3fea8:d72f7479da
label: NSPanel Haustür
thingTypeUID: mqtt:topic
configuration: {}
bridgeUID: mqtt:broker:aba3b3fea8
channels:
  - id: Command
    channelTypeUID: mqtt:string
    label: Command
    description: ""
    configuration:
      commandTopic: cmnd/nspanel/nextion
  - id: Result
    channelTypeUID: mqtt:string
    label: Result
    description: ""
    configuration:
      stateTopic: tele/nspanel/RESULT
      transformationPattern: JSONPATH:$.nextion
  - id: Buzzer
    channelTypeUID: mqtt:string
    label: Buzzer
    description: ""
    configuration:
      commandTopic: cmnd/nspanel/buzzer
```
- create a rule in openhab to feed the display with data and react to touch events (see NSPanel.rules)

Hint: I do not use the json protocol from Sonoff to communicate with the nextion panel, but the standard instruction set from nextion -> https://nextion.tech/instruction-set/ e.g.: to change the weather icon to "new mail arrived" you have to send `home.weather_icon.pic=57` as plain text to the `cmnd/nspanel/nextion` mqtt topic. You can also test commands directly on the Tasmota console by typing `Nextion home.weather_icon.pic=57`
