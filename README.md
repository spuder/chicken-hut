## Chicken Hut

Hardware to automate opening/closing a chicken door 


![](https://imgur.com/tMxvC4B.jpg)


## Hardware

- [Wemos D1 Mini](https://www.amazon.com/MakerFocus-NodeMcu-Development-ESP8266-Compatible/dp/B07KW54YSK)
- Any TTL Level N Channel Mosfet with a VGO < 3.3 volts 
  - [IRLZ34N is one I have used](https://www.amazon.com/BOJACK-IRLZ34N-IRLZ34NPBF-N-Channel-transistors/dp/B08L8S3154)


Mosfets are connected to pins 5 and 6 of a wemos D1 mini

HomeAssistant then uses the ESPHome plugin to flash the code to the micro controller. 

It takes about 8 seconds for my door to open, so I use the following yaml in my esphome

<details>
  <summary>ESPHome YAML</summary>

```
switch:
  - platform: gpio
    pin: D5
    name: "Open Relay"
    id: open_switch
    internal: true
    restore_mode: RESTORE_DEFAULT_OFF
  - platform: gpio
    pin: D6
    name: "Close Relay"
    id: close_switch
    internal: true
    restore_mode: RESTORE_DEFAULT_OFF
cover:
  - platform: template
    name: "Chicken Door"
    # icon: "mdi:emoticon-outline"
    device_class: door
    open_action:
      # Cancel any previous action
      - switch.turn_off: close_switch
      - switch.turn_on: open_switch
      - delay: 18.0s
      - switch.turn_off: open_switch
    close_action:
      - switch.turn_off: open_switch
      - switch.turn_on: close_switch
      - delay: 18.0s
      - switch.turn_off: close_switch
    stop_action:
      - switch.turn_off: close_switch
      - switch.turn_off: open_switch
    optimistic: true
    assumed_state: true
```

</details> 



![](https://imgur.com/VTxB0m1.jpg)

I have an automation to open the door in the morning

![](https://imgur.com/CNtVQGY.jpg)
Note in this image I reused an old board using transistors instead of mosfets. 

![](https://i.imgur.com/Oq3iS1v.jpg)


![](https://imgur.com/ZBennBE.jpg)
![](https://i.imgur.com/HpaAbO7.jpg)


![](https://imgur.com/dlTMcb8.jpg)