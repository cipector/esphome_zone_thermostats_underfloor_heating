# esphome_zone_thermostats_underfloor_heating
wifi control (ON/OFF) valves of underfloor heating with pump
I integrte it to Home Assistat and control it from there

# Scheme
<img align="right" src="https://github.com/cipector/esphome_zone_thermostats_underfloor_heating/blob/main/schema.png?raw=true">

# Part list 
 - wemos d1 mini
 - DS18B20 2x
 - 8x SSR relay
 - 5x 230V NO thermostat actuator
 - pump
 - MCP23017
 
# ESPHOME config

```
esphome:
  name: wemos-podlahove-topeni

esp8266:
  board: d1_mini

wifi:
  networks:
  - ssid: "YOUR_SSID"
    password: "YOUR_PASS" 
    priority: 1
    channel: 7 

# Enable logging
logger:

# Enable Home Assistant API
api:
  password: "PASSWORD"

ota:
  password: "PASSWORD"

captive_portal:

dallas:
  - pin: D4

sensor:
  - platform: dallas
    address: 0xe53c01e0761c5128
    name: "Zachod_teplota"
    accuracy_decimals: 2
    filters:
      - calibrate_linear:
          # Map 0.0 (from sensor) to 0.0 (true value)
          - 0.0 -> 0.0
          - 18.5 -> 20.6
    
  - platform: dallas
    address: 0xe53c01e0761c4873
    name: "Rozvadec_teplota"
    accuracy_decimals: 2

i2c:
  sda: GPIO4
  scl: GPIO5
  scan: true
  frequency: 800kHz

mcp23017:
  - id: 'mcp23017_hub'
    address: 0x20

switch:

# rele 1
  - platform: gpio
    id: podlahovka_obyvak1
    name: "podlahovka_obyvak1"
    pin: 
      mcp23xxx: mcp23017_hub
      number: 8
      mode:
        output: true
        pullup: false
      inverted: false

# rele 2
  - platform: gpio
    id: podlahovka_obyvak2
    name: "podlahovka_obyvak2"
    pin: 
      mcp23xxx: mcp23017_hub
      number: 9
      mode:
        output: true
        pullup: false
      inverted: false

# rele 3
  - platform: gpio
    id: podlahovka_kuchyn
    name: "podlahovka_kuchyn"
    pin: 
      mcp23xxx: mcp23017_hub
      number: 10
      mode:
        output: true
        pullup: false
      inverted: false

# rele 4
  - platform: gpio
    id: podlahovka_predsin
    name: "podlahovka_predsin"
    pin: 
      mcp23xxx: mcp23017_hub
      number: 11
      mode:
        output: true
        pullup: false
      inverted: false

# rele 5
  - platform: gpio
    id: podlahovka_koupelna
    name: "podlahovka_koupelna"
    pin: 
      mcp23xxx: mcp23017_hub
      number: 12
      mode:
        output: true
        pullup: false
      inverted: false

# rele 6
  - platform: gpio
    id: podlahovka_cerpadlo
    name: "podlahovka_cerpadlo"
    pin: 
      mcp23xxx: mcp23017_hub
      number: 13
      mode:
        output: true
        pullup: false
      inverted: true

# rele 7
  - platform: gpio
    id: podlahovka_7
    name: "podlahovka_7"
    pin: 
      mcp23xxx: mcp23017_hub
      number: 14
      mode:
        output: true
        pullup: false
      inverted: true

# rele 8
  - platform: gpio
    id: podlahovka_8
    name: "podlahovka_8"
    pin: 
      mcp23xxx: mcp23017_hub
      number: 15
      mode:
        output: true
        pullup: false
      inverted: true

```

# Home Assistant
<img align="right" src="https://github.com/cipector/esphome_zone_thermostats_underfloor_heating/blob/main/HA.png?raw=true">
