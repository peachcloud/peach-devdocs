# Physical Interface

The PeachCloud physical interface consists of a 5-way joystick, two push-buttons and a 128x64 OLED display.

Development is taking place on the [AdaFruit 128x64 1.3" OLED Bonnet](https://www.adafruit.com/product/3531). This bonnet comes in the pHAT form-factor and requires 3.3V power. It uses 9 GPIO pins and communicates over I2C. The included OLED display utilises the SSD1306 driver.

_Note: the 5-way joystick may be replaced by four or five push-buttons in the release version of the physical interface._

### Control

The OLED display shows the PeachCloud logo on start-up. Pressing the `A` button (`#5`) loads the root of the menu system. The `UP` and `DOWN` directions on the joystick allow scrolling through the menu options, while the `A` button functions as Select and the `B` button (`#6`) functions as Back.

Any button or joystick direction can be pressed to wake the display after selecting Display Off.

### Menu Structure

```
.
├── Networking
│   └── MODE
│   └── STATUS
│   └── NETWORK
│   └── IP
│   └── SIGNAL
│   └── Configuration
│       ├── Client Mode
│       └── Access Point Mode
├── System Stats
│   └── CPU
│   └── MEM
│   └── LOAD
│   └── UPTIME
│   └── DATA RX
│   └── DATA TX
├── Display Off
├── Reboot
└── Shutdown
```
