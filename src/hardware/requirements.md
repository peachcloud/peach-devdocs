# Requirements

Development is currently taking place with the following hardware:

 - Raspberry Pi 3B+
 - Adafruit 128x64 1.3" OLED Bonnet ([product page](https://www.adafruit.com/product/3531))
 - DS1338 Real-Time Clock (RTC) module ([datasheet](http://www.hobbytronics.co.uk/datasheets/DS1338-RTC.pdf))

A breadboard and male-to-female jumper wires are also recommended for prototyping.

If the exact parts specified above are not available, substitutes can be used - provided they conform the following specifications:
 
 - OLED display
   - 128 x 64 pixels
   - SSD1306-compatible
 - Buttons
   - 7 x push-buttons _or_ 2 x push-buttons and 1 x 5-direction joystick
 - Real-Time Clock (RTC)
   - DS1338 or DS1307

Note: the hardware peripherals listed on this page do not work out-of-the-box with Debian Buster ARM64. You will need to patch the device tree and run additional configuration steps (patches are applied automatically if following the 'quick deployment' method using install scripts). See Configuration in the Operating System section of this document for further information.
