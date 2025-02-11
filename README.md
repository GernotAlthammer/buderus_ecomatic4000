# buderus_ecomatic4000
ESPHome code for Buderus Ecomatic 4000 HS4201 with KM2.0 Serial Module M404.

<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Pictures/HA-Buderus-Panel.png" style="width: 100%;">

The code is based on the KM271-WiFi-Module software developed as replacement of Logamatic 2107 KM271 serial module.

See: https://github.com/the78mole/esphome_components/tree/main

To compile the ESPHome code for the ESP32 it is necessary to copy
- the subdirectory and files from /my_components/km271_wifi
- and the file uart_read_line_sensor.h 

to the ESPHome directory of your Home Assistant.

<h3 tabindex="-1" class="heading-element" dir="auto">Limitations</h3>
The software only provides a sub-set of the available data.

Currently only some sensor values are working (see km271_params.h)
- Aussentemperatur                (0x2109)                  
- Kesselvorlaufisttemperatur      (0x1105)
- Warmwasseristtemperatur         (0x721A)
- HK1 Vorlaufisttemperatur        (0x420f)
- HK1 Mischerstellung             (0x6116)
- Kesselvorlaufsolltemperatur     (0x1206)
- Warmwassersolltemperatur        (0x320c)
- HK1 Vorlaufsolltemperatur       (0x410e)

Currently only some binary states are working (see km271_params.h)

Steuer-Zustände                   (0x0803)
- ST-Bit 0: Brenner EIN
- ST-Bit 1: Ladepumpe Warmwasser
- ST-Bit 2: Zirkulationspumpe Heizkörper
- ST-Bit 3: Mischer AUF
- ST-Bit 4: Mischer ZU
- ST-Bit 5: Brenner AUS
- ST-Bit 7: Zirkulationspumpe Fußboden

Betriebs-Zustände                 (0x3108)
- BTR-Bit 0: Betrieb Tag
- BTR-Bit 1: Betrieb Automatik
- BTR-Bit 2: Betrieb Sommer

Changing of input values is not supported yet, as i have not figured out the command codes that are needed to send the parameters.
There is some more logging and analyzing necessary... 

<h3 tabindex="-1" class="heading-element" dir="auto">Notes</h3>
I'm working on this project as a hobby. My work on this software is in no way associated with a company. If you like to use it, or improve on it, feel free. Use it at your own risk - it might work perfectly or it might not.


THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


Edit: 2025-01-18
