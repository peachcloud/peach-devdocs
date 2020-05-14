# GPIO Pinout

The OLED bonnet (with buttons) and DS138 RTC module are connected to the Pi via GPIO pins.

```
3v3 Power                       -   1       // used by oled bonnet & rtc
BCM 2       (SDA)               -   3       // used by oled bonnet & rtc
BCM 3       (SCL)               -   5       // used by oled bonnet & rtc
Ground                          -   6       // used by oled bonnet & rtc
BCM 4       (joystick center)   -   7       // used by oled bonnet
BCM 17      (joystick up)       -   11      // used by oled bonnet
BCM 27      (joystick left)     -   13      // used by oled bonnet
BCM 22      (joystick down)     -   15      // used by oled bonnet
BCM 23      (joystick right)    -   16      // used by oled bonnet
BCM 5       (button A)          -   29      // used by oled bonnet
BCM 6       (button B)          -   31      // used by oled bonnet
```
