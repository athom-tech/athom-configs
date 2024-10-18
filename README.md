# Only applicable to ESP8285,[Please click for ESP32 devices](https://github.com/athom-tech/esp32-configs)

### Athom ESPHome configurations

This repository contains a bunch of ESPHome configurations for https://athom.tech devices.

If you are prompted that there is not enough space, you should upgrade `ESP8266_MINI.bin` first

- mini is a transit firmware, after running, it will generate a hotspot of "ESP_UPDATE_XXXXXX"
- Connect to the hotspot and visit `http://192.168.4.1/update` in the browser
- Upload updated ESPHome firmware

### Migrating to Tasmota

- Select firmware upgrade, upload `tasmota.bin.gz` and click Update, ***Please don't choose `tasmota.bin`***
- Download Tasmota firmware here http://ota.tasmota.com/tasmota/release/tasmota.bin.gz

### Migrating from Tasmota

- First execute `SetOption78 1` in the console of Tasmota
- Select firmware upgrade, upload `tasmota-minimal.bin.gz` and click start upgrade
- Select the firmware upgrade again, upload the firmware of ESPHome and click to start upgrade

### Device List
device|picture|file name|notice
:---:|:---:|:---:|:---:
Athom_Plug_V2|<img src="/images/Athom_EU_Plug_V2.png" width="50%" height="20%"><img src="/images/Athom_US_Plug_V2.png" width="50%" height="20%"><img src="/images/Athom_UK_Plug_V2.png" width="50%" height="20%"><img src="/images/Athom_BR_Plug_V2.png" width="50%" height="20%"><img src="/images/Athom_AU_Plug_V2.png" width="50%" height="20%"><img src="/images/Athom_IL_Plug_V2.png" width="50%" height="20%"><img src="/images/Athom_IT_Plug_V2.png" width="50%" height="20%">|athom-smart-plug-v2.yaml
Athom_Wall_Outlet|<img src="/images/Athom_Wall_Outlet.png" width="50%" height="20%">|athom-wall-outlet.yaml
Athom_1Gang_Switch|<img src="/images/Athom_1Gang_Touch_Switch.png" width="50%" height="20%"><img src="/images/Athom_1Gang_Key_Switch.png" width="50%" height="20%"><img src="/images/Athom_1Gang_US_Switch.png" width="50%" height="20%">|athom-sw01.yaml
Athom_2Gang_Switch|<img src="/images/Athom_2Gang_Touch_Switch.png" width="50%" height="20%"><img src="/images/Athom_2Gang_Key_Switch.png" width="50%" height="20%">|athom-sw02.yaml
Athom_3Gang_Switch|<img src="/images/Athom_3Gang_Touch_Switch.png" width="50%" height="20%"><img src="/images/Athom_3Gang_Key_Switch.png" width="50%" height="20%">|athom-sw03.yaml
Athom_4Gang_Switch|<img src="/images/Athom_4Gang_Touch_Swtich.png" width="50%" height="20%">|athom-sw04.yaml
Athom_7W_Bulb<br/>Athom_GU10_Bulb|<img src="/images/Athom_7w_Bulb.png" width="50%" height="20%"><img src="/images/Athom_GU10_Bulb.png" width="50%" height="20%">|athom-rgbww-light.yaml
Athom_15W_Bulb|<img src="/images/Athom_15w_Bulb.png" width="50%" height="20%">|athom-rgbct-light.yaml
Athom_12W_Bulb|<img src="/images/Athom_BR30_Bulb.png" width="50%" height="20%">|athom-rgbct-light.yaml|<h4>Temporarily discontinued</h4>
Athom_RGB_Controller|<img src="/images/Athom_RGB_Controller.png" width="50%" height="20%">|athom-rgb-light.yaml
Athom_Addressable_Light_Strip_Controller|<img src="/images/Athom_3Pin_Controller.png" width="50%" height="20%">|athom-ws2812b.yaml
Athom_High_Power_RGBW_Light_Strip_Controller|<img src="/images/Athom_RGBW_Controller.png" width="50%" height="20%">|athom-rgbw-light.yaml
Athom_High_Power_Addressable_Light_Strip_Controller|<img src="/images/Athom_4Pin_Controller.png" width="50%" height="20%">|athom-ls-4p-3wire.yaml
Athom_Mini_Switch|<img src="/images/Athom_Mini_Relay.png" width="50%" height="20%">|athom-mini-switch.yaml
Athom_CB02_Switch|<img src="/images/Athom_CB02.png" width="50%" height="20%">|athom-cb02.yaml
Athom_Presence_Sensor|<img src="/images/Athom_Presence_Sensor.png" width="50%" height="40%">|athom-presence-sensor.yaml
Athom_Garage_Door_Opener|<img src="/images/Athom_Garage_Door_Opener.png" width="50%" height="20%">|athom-garage-door.yaml
Athom_Plug|<img src="/images/Athom_EU_Plug.png" width="50%" height="20%"><img src="/images/Athom_UK_Plug.png" width="50%" height="20%">|athom-smart-plug.yaml|<h4>Discontinued</h4>
