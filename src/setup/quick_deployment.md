# Quick Deployment

This guide contains the steps for setting up PeachCloud on a raspberry pi using a PeachCloud disc image. 

## Step 1: Flash The SD Card

Download the latest PeachCloud image from http://releases.peachcloud.org and flash it to an SD card.

_Note:_ Be sure to use the correct device location in the `dd` command, otherwise you risk wiping another connected USB device. `sudo dmesg | tail` can be run after plugging in the SD card to determine the correct device location:

```bash
wget http://releases.peachcloud.org/peach-imgs/20210225/20210225_peach_raspi3.img
sudo dd 20210225_peach_raspi3.img of=/dev/sdb bs=64k oflag=dsync status=progress
```

On Mac OS, use the following command to flash the SD card:

`xzcat 20200831_raspi_3.img.xz | sudo dd of=/dev/sdcarddisc`

Alternatively, use [Etcher](https://www.balena.io/etcher/).

Your SD card now has a complete PeachCloud installation on it and is ready to use. 


## Step 2: Connecting To The Internet 

#### Via access point

When PeachCloud first starts it will start in access point mode. From your laptop or phone look for a wifi network 
named "peach" and connect to it using the password "cloudless". 

Once you are connected, in your browser navigate to peach.local,
which should load the PeachCloud web interface. 

From the PeachCloud web interface, you can connect to the internet:
1. Settings -> Add Wifi Network (add your wifi credentials)
2. Click "Enable Wifi"

This will take PeachCloud out of access point mode and into client mode. 
If you connect to the same WIFI network as the peach, 
you should now also be able to access the web interface via peach.local. 


#### Via screen

If you connect a screen and keyboard to the raspberry pi, you can login to the pi directly using 
the following username and password:
```
username: peach
password: peachcloud
```

You can then edit `/etc/wpa_supplicant/wpa_supplicant-wlan0.conf`
and add your wifi credentials to the file:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="your_ssid"
    psk="your_wifi_password"
}
```

#### Via sd card 

If you create a file on the RASPIFIRM partition of the sdcard named `wpa_supplicant.conf`,
while PeachCloud is booting this file will be copied to `/etc/wpa_supplicant/wpa_supplicant-wlan0.conf`
and used for connecting to wifi. 

This is a third way you can get the PeachCloud connected to the internet. 

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="your_ssid"
    psk="your_wifi_password"
}
```


## Step 3: Getting Started

You can then continue to configure your PeachCloud via the web interface (accessed by peach.local),
through the physical interface, or through ssh (`ssh peach@peach.local`).

There is a file with detailed instructions on how to connect via a direct ethernet cable located [here](/software/operating_system/networking.md)

TODO: link to PeachCloud caretaker guides for more detailed info on how to configure the device.

When sshed into the device you can also configure and update the device using the peach-config CLI tool. 
Documentation on how to use peach-config can be found [here](https://github.com/peachcloud/peach-config).