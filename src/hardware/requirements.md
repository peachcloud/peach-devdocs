# Requirements

Minimal hardware component requirements for prototyping and development:

 - Raspberry Pi 3B+ / 4 (or similar spec single-board computer)
 - OLED display
   - 128 x 64 pixels
   - SSD1306-compatible
 - Buttons
   - 7 push-buttons _or_ 2 push-buttons and 1 5-direction joystick
 - Real-Time Clock (RTC)
   - DS1338 or DS1307

Note: these peripherals do not work out-of-the-box with Debian Buster ARM64. You will need to patch the device tree and run additional configuration steps. See the Hardware Configuration section of the documentation for further information.
