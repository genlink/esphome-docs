Sonoff T1 US 3 Gang
===================

.. code-block:: yaml

    #This is Configuration for Sonoff T1 US 3 Gang

    esphome:
      name: livingroom
      platform: ESP8266
      board: esp01_1m

    wifi:
      ssid: !secret wifi_ssid
      password: !secret wifi_password

   # Enable logging
    logger:

    # Enable Home Assistant API
    api:
      password: !secret api_password

    ota:
      password: !secret ota_password


    switch:
      - platform: gpio
        name: "Living Room Lamp 1"
        pin: GPIO12
        id: relay1
      - platform: gpio
        name: "Living Room Lamp 2"
        pin: GPIO5
        id: relay2
      - platform: gpio
        name: "Living Room Lamp 3"
        pin: GPIO4
        id: relay3
      - platform: restart
        name: "Living Room Restart"

    binary_sensor:
      - platform: gpio
        pin:
          number: GPIO0
          mode: INPUT_PULLUP
          inverted: True
        name: "Living Room Lamp 1 Touch"
        on_press:
         - switch.toggle: relay1
      - platform: gpio
        pin:
          number: GPIO9
          mode: INPUT_PULLUP
          inverted: True
        name: "Living Room Lamp 2 Touch"
        on_press:
            - switch.toggle: relay2
      - platform: gpio
        pin:
          number: GPIO10
          mode: INPUT_PULLUP
          inverted: True
        name: "Living Room Lamp 3 Touch"
        on_press:
            - switch.toggle: relay3
      - platform: status
        name: "Living Room Lamp Status"


    output:
      # Register the blue LED as a dimmable output ....
      - platform: esp8266_pwm
        id: blue_led
        pin: GPIO13
        inverted: True

    light:
      # ... and then make a light out of it.
      - platform: monochromatic
        name: "Living Room LED"
        output: blue_led
    sensor:
      - platform: wifi_signal
        name: "Living Room WiFi Signal "
        update_interval: 60s
      - platform: uptime
        name: "Living Room Uptime"
    text_sensor:
      - platform: version
        name: "ESPHome Living Room Version"
