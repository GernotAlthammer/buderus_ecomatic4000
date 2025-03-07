# HA Panel for Buderus Ecomatic 4000 HS4201 control unit

This are example files of the Home Assistant Panel to show the values of the Buduerus Ecomatic 4000 HS4201 control unit

- the file Buderus.yaml is the RAW file for the HA panel
- the file HA-Settings.yaml is the collection of template sensors used in the HA panel

In addition for Home Assistant the following helpers are needed:

- Integralsensor / Integration Sensor: Brenner Energie
  - sensor.brenner_energie
  - input: sensor.brenner_leistung
  - intergration: left riemann sum

- Integralsensor / Integration Sensor: Pumpe WW Energie
  - sensor.punmpe_ww_energie
  - input: sensor.punpe_ww_leistung
  - intergration: left riemann sum

- Integralsensor / Integration Sensor: Pumpe HK Energie
  - sensor.punmpe_hk_energie
  - input: sensor.punpe_hk_leistung
  - intergration: left riemann sum

- Integralsensor / Integration Sensor: Pumpe FB Energie
  - sensor.punmpe_fb_energie
  - input: sensor.punpe_fb_leistung
  - intergration: left riemann sum

- Integralsensor / Integration Sensor: Heizöl Menge
  - sensor.heizol_menge
  - input: sensor.oelverbrauch
  - intergration: left riemann sum

- Integralsensor / Integration Sensor: Heizöl Energie
  - sensor.heizol_energie
  - input: sensor.heizwert
  - intergration: left riemann sum

- Verbrauchszähler / Consumption Sensor: Heizöl Tagesenergie
  - sensor.heizol_tagesenergie
  - input: sensor.heizol_energie

- Verbrauchszähler / Consumption Sensor: Heizöl Tagesmenge
  - sensor.heizol_tagesmenge
  - input: sensor.heizol_menge

