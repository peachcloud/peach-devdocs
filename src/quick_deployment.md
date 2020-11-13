# Quick Deployment

[![GitHub logo](/assets/github_logo.png "peach-config GitHub repository")](https://github.com/peachcloud/peach-config)

The instructions, configuration files and scripts referred to in this section can all be found in the [peach-config repo](https://github.com/peachcloud/peach-config).

## Prerequisite Steps

Download the latest Debian Buster preview image for RPi3 and flash it to an SD card.

Note: Be sure to use the correct device location in the `dd` command, otherwise you risk wiping another connected USB device. `sudo dmesg | tail` can be run after plugging in the SD card to determine the correct device location:

```bash
wget https://raspi.debian.net/verified/20200831_raspi_3.img.xz
xzcat 20200831_raspi_3.img.xz | sudo dd of=/dev/mmcblk0 bs=64k oflag=dsync status=progress
```

On Mac OS, use the following command to flash the SD card:

`xzcat 20200831_raspi_3.img.xz | sudo dd of=/dev/sdcarddisc`

Alternatively, use [Etcher](https://www.balena.io/etcher/).

Note: if the above image link stops working, you can find the complete list of Raspberry Pi Debian images [here](https://raspi.debian.net/tested-images/).

## Setup

Quick setup commands to connect to a local WiFi network over the `wlan0` interface (assuming `eth0` connection is not possible):

```bash
# username
root
# password (by default raspberry debian requires no password, so we set the password for root here)
passwd
# set interface up (run command twice if you receive 'link is not ready' error on first try)
ip link set wlan0 up
# append ssid and password for wifi access point
wpa_passphrase <SSID> <PASS> > /etc/wpa_supplicant/wpa_supplicant.conf
# open wpa_supplicant.conf
nano /etc/wpa_supplicant/wpa_supplicant.conf
```

[ Add the following two lines to top of file ]

```plaintext
ctrl_interface=/run/wpa_supplicant
update_config=1
```

[ Save and exit ]

```bash
# open network interfaces config
nano /etc/network/interfaces
```

[ Add the following lines to the file ]

```plaintext
auto lo
iface lo inet loopback

allow-hotplug wlan0
auto wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

[ Save and exit ]

`reboot now`

[ Pi should now be connected to the WiFi network ]

## Scripts

The `setup_dev_env.py` script can be executed once your Pi is internet-connected and `git` and `python` have been installed. It takes a `<USER>` argument to create a new system user. You will be prompted to enter a password for your newly created user. The script will install system requirements and copy configuration files relating to networking, I2C and RTC.

_TODO: Add flags to (de)select I2C, RTC and Rust install & config. Flags should also be added which allow the installer to choose between development environment and release environment configuration._

```bash
apt update
apt install git python
git clone https://github.com/peachcloud/peach-config.git
cd peach-config
python scripts/setup_dev_env.py <USER>
```
