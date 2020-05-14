# Configuration

### Real-Time Clock (RTC)

Additional configuration is required for Debian Buster ARM64 before I²C devices can function correctly. The following steps are required for the DS1338 module / DS1307 chip:

Run an I²C scan to verify that the module is correctly wired to the Pi:

```
sudo apt-get install python-smbus i2c-tools
sudo modprobe i2c-dev
sudo i2cdetect -y 1
```

The final command in the sequence prints an array to the console, with `68` denoting the presence of the RTC module. This is a sign that the device is properly wired and connected.

Next, append `dtoverlay=i2c-rtc,ds1307` and `dtparam=i2c_arm=on` to `/boot/firmware/config.txt` and `i2c-dev` to `/etc/modules`.

The device tree must be patched in order to set the clock frequency for I²C devices:

Copy `/boot/firmware/bcm2837-rpi-3-b.dtb` from the Pi microSD card to a computer running Linux. Run the following commands:

```
sudo apt-get install device-tree-compiler
cd /place/where/dtb/file/was/pasted
dtc -I dtb -O dts > bcm2837-rpi-3-b.dts
```

The third command outputs a human-readable decompiled device tree (`dts`). The clock frequency must be manually added to the generated `dts` file (line 570):

`clock-frequency = <0x186a0>`

Once patched, recompile the `dts` to binary blob format:

`dtc -O dtb -o bcm2837-rpi-3-b.dtb bcm2837-rpi-3-b.dts`

Copy the resulting `dtb` onto the Pi microSD card and overwrite `/boot/firmware/bcm2837-rpi-3-b.dtb`.

Finally, run the following commands to complete the process:

```
sudo modprobe i2c-bcm2835
su
echo ds1307 0x68 > /sys/class/i2c-adapter/i2c-1/new_device
exit
sudo modprobe rtc-ds1307
```

Run `sudo i2cdetect -y 1` to confirm RTC initialization.

### OLED Bonnet

A device tree overlay is required to activate the internal pull-up resistors on the Pi. Without this overlay, some of the buttons on the OLED bonnet will not work.

Begin by downloading the device tree overlay file: `mygpio.dtbo` (TODO: add download link...maybe add the file to one of the repos). Then run the following commands to apply the overlay:

```
sudo mkdir /boot/firmware/overlays
sudo cp mygpio.dtbo /boot/firmware/overlays/
```

Append the following line to `/boot/firmware/config.txt`:

`device_tree_overlay=overlays/mygpio.dtbo`

Reboot to apply the changes. All buttons on the OLED bonnet should now function correctly.
