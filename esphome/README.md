# ESPHome code for ESP32 Mini

This is example code for connecting an ESP32 Mini with an additional RS232 interface to the M404 KM2.0 module of a Buderus HS4201 Ecomatic 4000 heater control unit.

The YAML code for the ESP32 is: buderus-km271.yaml

Note: To compile the ESPHome code for the ESP32 D1 Mini it is no loner necessary to copy

- the subdirectory and files from /esphone/components/km271_wifi
- and the file uart_read_line_sensor.h

to the ESPHome directory of your Home Assistant.
