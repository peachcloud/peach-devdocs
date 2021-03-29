# Installation From Debian Disc Image

You can also setup PeachCloud by installing Debian onto an sd card, and then installing and running peach-config. 
This is essentially how the PeachCloud disc image is created (see [peach-img-builder](https://github.com/peachcloud/peach-img-builder)).

Here are the steps for installing peach-config on Debian. 

## Step 1: Flash The SD Card

Download the latest Debian Buster preview image for RPi3 and flash it to an SD card.

_Note:_ Be sure to use the correct device location in the `dd` command, otherwise you risk wiping another connected USB device. `sudo dmesg | tail` can be run after plugging in the SD card to determine the correct device location:

```bash
wget https://raspi.debian.net/verified/20200831_raspi_3.img.xz
xzcat 20200831_raspi_3.img.xz | sudo dd of=/dev/sdb bs=64k oflag=dsync status=progress
```

On Mac OS, use the following command to flash the SD card:

`xzcat 20200831_raspi_3.img.xz | sudo dd of=/dev/sdcarddisc`

Alternatively, use [Etcher](https://www.balena.io/etcher/).

_Note:_ If the above image link stops working, you can find the complete list of Raspberry Pi Debian images [here](https://raspi.debian.net/tested-images/).


## Step 2: Connect To The Internet 

Use the following commands to connect to a local WiFi network over the `wlan0` interface (assuming `eth0` connection is not possible):

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
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
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

auto eth0
allow-hotplug eth0
iface eth0 inet dhcp

auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```


[ Save and exit ]

`reboot now`

[ Pi should now be connected to the WiFi network ]

## Step 3: Install PeachCloud


You can run the following commands to add the PeachCloud apt repository and install PeachCloud:
``` bash
echo "deb http://apt.peachcloud.org/ buster main" > /etc/apt/sources.list.d/peach.list
wget -O - http://apt.peachcloud.org/pubkey.gpg | sudo apt-key add -
apt-get update
apt-get install -y python3-peach-config
peach-config setup -i -n -d peach
```


#### Connecting

Once the setup script has been run, connect to the system remotely over the local network using ssh or mosh:

`ssh user@peach.local` or `mosh user@peach.local`

There is a file with detailed instructions on how to connect via a direct ethernet cable located in `docs/direct-ethernet-setup.md`


#### Network

Networking is handled by `wpa_supplicant` and `systemd-networkd`.

The RPi connects to other networks with the `wlan0` interface and deploys an access point on the `ap0` interface. Only one of these modes is active at a time (client or access point). The RPi boots in client mode by default.

To switch to access point mode:

`sudo systemctl start wpa_supplicant@ap0.service`

To switch to client mode:

`sudo systemctl start wpa_supplicant@wlan0.service`

_Note:_ No stopping of services or rebooting is required.

To enable access point mode on boot:

```bash
sudo systemctl disable wpa_supplicant@wlan0.service
sudo systemctl enable wpa_supplicant@ap0.service
```

A standalone networking configuration script is included in this repository (`scripts/setup_networking.py`). Network-related documentation can also be found in this repository (`docs`).

This repository also contains a script for automatically starting an access point on `ap0` if the `wlan0` service is active but not connected (`scripts/ap_auto_deploy.sh`). The executable script is installed at `/usr/local/bin/ap_auto_deploy` and can either be run once-off or scheduled for repeated execution using a `systemd` service file and timer file (`conf/network/ap-auto-deploy.service` and `conf/network/ap-auto-deploy.timer`). When the timer is enabled for repeated execution, the script is automatically run 60 seconds after boot and every 180 seconds after that.

To stop and disable the access point auto deploy service:

```bash
sudo systemctl stop ap-auto-deploy.timer
sudo systemctl disable ap-auto-deploy.timer
```


## Troubleshooting

You may encounter DNS issues if your system time is inaccurate. Please refer to this [StackExchange answer](https://unix.stackexchange.com/a/570382/450882) for details. The steps to remedy the situation are offered here in brief:

```bash
sudo -Es
timedatectl set-ntp 0
# edit this line according to your current date & time
timedatectl set-time "2021-01-13 11:37:10"
timedatectl set-ntp 1
exit
