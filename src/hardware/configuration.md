# Configuration

This page contains the detailed, step-by-step software configuration instructions required to ensure that all the hardware works correctly. Skip to the quick deployment section for the scripted deployment instructions.

**GPIO**

GPIO (General Purpose Input Output) appears to be working out-of-the-box, but be aware that all pin numbers are offset by 458 for Debian on the Raspberry Pi 3 (as mentioned in the [Debian RaspberryPi3 wiki](https://wiki.debian.org/RaspberryPi3)).

**RTC over I²C**

Additional configuration is required for Debian Buster ARM64 before I²C devices can function correctly. The following steps are required for the DS1338 module / DS1307 chip:

_Tested with DS1338 RTC module._

Ensure `i2c-tools` and `python-smbus` Debian packages have been installed. Then run the following to ensure the RTC is correctly wired and connected to the Pi:

`sudo modprobe i2c-dev`  
`sudo i2cdetect -y 1`

The final command in the sequence prints an array to the console, with 68 denoting the presence of the RTC module. This is a sign that the device is properly wired and connected.

Append the following two lines to `/boot/firmware/config.txt`:

`dtoverlay=i2c-rtc,ds1307`  
`dtparam=i2c_arm=on`

Append the following line to `/etc/modules`:

`i2c-dev`  

Patch the device tree in order to set the clock frequency for I²C devices:

_Note: the steps below can be shortened by downloading the [pre-patched `dtb` file](https://github.com/peachcloud/peach-config/raw/main/conf/bcm2710-rpi-3-b.dtb) from the [peach-config repo](https://github.com/peachcloud/peach-config) and using it to replace the `dtb` file on the Pi's SD card._

Copy the existing `dtb` (device tree blob) from the Pi microSD card to a more powerful computer running Linux (exact location is up to you). You will find the `dtb` in the following location:

`/boot/firmware/bcm2710-rpi-3-b.dtb`  

_Note: the file may be named `bcm2837-rpi-3-b.dtb`. In that case, replace all instances of `bcm2710` in these instructions with `bcm2837`._

Once it has been copied to your Linux computer, run the following commands (on the Linux machine):

Install the compiler / decompiler:

`sudo apt-get install device-tree-compiler`

Change into the direction where you copied the `dtb` from the Pi:

`cd /place/where/dtb/file/was/pasted`

Generate a human-readable decompiled device tree (`dts`):

`dtc -I dtb -O dts > bcm2710-rpi-3-b.dts`

Open the `dts` in a text editor and add the clock-frequency property (line 570):

`clock-frequency = <0x186a0>;`

Save and exit the document. Recompile the patched `dts` to binary blob format:

`dtc -O dtb -o bcm2710-rpi-3-b.dtb bcm2710-rpi-3-b.dts`

Copy the resulting `dtb` onto the Pi microSD card and overwrite the old `dtb` file (you may wish to backup the old `dtb` first):

`cp bcm2710-rpi-3-b.dtb /boot/firmware/bcm2710-rpi-3-b.dtb`

Finally, run the following commands to complete the process:

`sudo modprobe i2c-bcm2835`  
`su`  
`echo ds1307 0x68 > /sys/class/i2c-adapter/i2c-1/new_device`  
`exit`  
`sudo modprobe rtc-ds1307`

Run the `i2cdetect` tool to ensure the RTC kernel driver is working:

`sudo i2cdetect -y 1`

A `UU` entry in the output arrays denotes success. If you still see `68` or similar, the module is not being controlled by the kernel driver.

Ensure no `i2c could not read clock-frequency property` errors persist in the kernel logs (`/var/log/kern.log`).

_Note: This Scuttlebutt dev-diary post by @glyph documents the process step-by-step (%aEVy1gyTSl4qrbazrwrgnrLs4pRVobELwQjH/TUtsAc=.sha256)._

**OLED Bonnet**

A device tree overlay is required to activate the internal pull-up resistors on the Pi. Without this overlay, some of the buttons on the OLED bonnet will not work.

Begin by downloading the device tree overlay file: `mygpio.dtbo`.

`wget https://github.com/peachcloud/peach-config/raw/main/conf/mygpio.dtbo`

Create an `overlays` directory on the Pi filesystem:

`sudo mkdir /boot/firmware/overlays`

Copy the overlay file into the `overlays` directory:

`sudo cp mygpio.dtbo /boot/firmware/overlays/`

Append the following line to `/boot/firmware/config.txt`:

`device_tree_overlay=overlays/mygpio.dtbo`

Reboot to apply the changes. All buttons on the OLED bonnet should now function correctly.
