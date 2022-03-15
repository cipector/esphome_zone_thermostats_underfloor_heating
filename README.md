# esphome_zone_thermostats_underfloor_heating
wifi control (ON/OFF) valves of underfloor heating with pump
I integrte it to Home Assistat and control it from there

# Scheme
<img align="right" src="https://github.com/cipector/esphome_zone_thermostats_underfloor_heating/blob/main/schema.png?raw=true">

# Part list 
 - wemos d1 mini - https://www.hadex.cz/m430j-modul-wemos-d1-mini-wifi-esp8266/
 - DS18B20 2x - https://www.hadex.cz/r255-teplotni-sonda-ds18b20-100cm/
 - 8x SSR relay - https://www.hadex.cz/m528d-modul-rele-ssr-8x-napajeni-5v-s-optoclenem-hy-m281/
 - 5x 230V NO thermostat actuator - https://www.aliexpress.com/item/4000917735846.html?spm=a2g0o.order_list.0.0.21ef1802ZLEKRH
 - pump - i already had
 - MCP23017 - https://www.aliexpress.com/item/32865063393.html?spm=a2g0o.productlist.0.0.3e6b2ef1qsywrq&algo_pvid=828a1e78-167e-4242-8a6c-cba0782255e6&algo_exp_id=828a1e78-167e-4242-8a6c-cba0782255e6-11&pdp_ext_f=%7B%22sku_id%22%3A%2210000001800719023%22%7D&pdp_pi=-1%3B109.77%3B-1%3B-1%40salePrice%3BCZK%3Bsearch-mainSearch
 
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

Also i have added automatization for ON/ OFF my main source of heating with condition if there are any general termostat heationg or not.

<img align="right" src="https://github.com/cipector/esphome_zone_thermostats_underfloor_heating/blob/main/HA.png?raw=true">

## Configuration.yaml 

add general thermostats

```
climate:
  - platform: generic_thermostat
    name: "Obyvak_topeni"
    heater: switch.podlahovka_obyvak1
    target_sensor: sensor.Obyvak_teplota
    min_temp: 16
    max_temp: 25
    cold_tolerance: 0.2
    hot_tolerance: 0
    ac_mode: false
    precision: 0.1
    initial_hvac_mode: "off"
    away_temp: 18

    
  - platform: generic_thermostat
    name: "Kuchyn_topeni"
    heater: switch.podlahovka_kuchyn
    target_sensor: sensor.Kuchyn_teplota
    min_temp: 16
    max_temp: 25
    cold_tolerance: 0.2
    hot_tolerance: 0
    ac_mode: false
    precision: 0.1
    initial_hvac_mode: "off"
    away_temp: 18

  - platform: generic_thermostat
    name: "Predsin_topeni"
    heater: switch.podlahovka_predsin
    target_sensor: sensor.Predsin_teplota
    min_temp: 16
    max_temp: 25
    cold_tolerance: 0.2
    hot_tolerance: 0
    ac_mode: false
    precision: 0.1
    initial_hvac_mode: "off"
    away_temp: 18
    
  - platform: generic_thermostat
    name: "Koupelna_topeni"
    heater: switch.podlahovka_koupelna
    target_sensor: sensor.Zachod_teplota
    min_temp: 16
    max_temp: 25
    cold_tolerance: 0.2
    hot_tolerance: 0
    ac_mode: false
    precision: 0.1
    initial_hvac_mode: "off"
    away_temp: 18
    
    
  ```
