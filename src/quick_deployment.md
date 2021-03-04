# Quick Deployment

[![GitHub logo](/assets/github_logo.png "peach-config GitHub repository")](https://github.com/peachcloud/peach-config)

The instructions, configuration files and scripts referred to in this section can all be found in the [peach-config repo](https://github.com/peachcloud/peach-config).
### Prerequisite Steps

Download the latest Debian Buster preview image for RPi3 and flash it to an SD card.

Note: Be sure to use the correct device location in the `dd` command, otherwise you risk wiping another connected USB device. `sudo dmesg | tail` can be run after plugging in the SD card to determine the correct device location:

```bash
wget https://raspi.debian.net/verified/20200831_raspi_3.img.xz
xzcat 20200831_raspi_3.img.xz | sudo dd of=/dev/sdb bs=64k oflag=dsync status=progress
```

On Mac OS, use the following command to flash the SD card:

`xzcat 20200831_raspi_3.img.xz | sudo dd of=/dev/sdcarddisc`

Alternatively, use [Etcher](https://www.balena.io/etcher/).

Note: if the above image link stops working, you can find the complete list of Raspberry Pi Debian images [here](https://raspi.debian.net/tested-images/).

### Setup

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
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wpactrl-user
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

### Scripts

**System Configuration**

The `setup_dev_env.py` script can be executed once your Pi is internet-connected and `git` and `python` have been installed. 

```bash
apt update
apt install git python
git clone https://github.com/peachcloud/peach-config.git
cd peach-config
# run the script with --i2c and --rtc flags to configure
python scripts/setup_dev_env.py -i -r ds3231 <USER>
```

Running the script with the `-h` flag shows the help menu:

```bash
usage: setup_dev_env.py [-h] [-i] [-r {ds1307,ds3231}] user

positional arguments:
  user                  username for the default user account
  
optional arguments:
  -h, --help            show this help message and exit
  -i, --i2c             configure i2c
  -r {ds1307,ds3231}, --rtc {ds1307,ds3231}
                        configure real-time clock
```

A `<USER>` argument must be supplied to create a new system user. You will be prompted to enter a password for your newly created user.

The script also allows optional configuration of I2C and real-time clock (RTC) modules. I2C configuration is necessary for the OLED display and physical interface to work correctly. RTC configuration is required for the real-time clock to work correctly. When passing the `-r` flag, the type of real-time clock module must be included (either ds1307 or ds3231). Selecting real-time clock configuration will not work if the I2C flag is not selected (in other words, the real-time clock requires I2C).

Run the script as follows for a full installation and configuration with I2C and the ds3231 RTC module (username in this case is `peach`):

`python scripts/setup_dev_env.py -i -r ds3231 peach`

**Network**

The RPi connects to other networks with the `wlan0` interface and deploys an access point on the `ap0` interface. Only one of these modes is active at a time (client or access point). Two scripts are included in this repository to allow easy switching between client and access point modes: `activate_ap.sh` and `activate_client.sh`.

Networking is handled with `wpa_supplicant`, `hostapd` and `dnsmasq`.

### Connecting

Once the setup script has been run, connect to the system remotely over the local network using ssh or mosh:

`ssh user@peach.local` or `mosh user@peach.local`

### Connecting Directly Through Ethernet Cable

If you would like to work on the pi by connecting directly through an ethernet cable, 
add the additional lines below to /etc/network/interfaces on the pi.
This is with a laptop having static IP 192.168.0.240 and pi having static IP 192.168.0.241,
but these addresses are arbitrary as long as they are in the same subnet.

```
allow-hotplug eth0
auto eth0
iface eth0 inet static
    address 192.168.0.241 
    # the following lines route all internet traffic not to the laptop away from eth0 interface
    up ip route del 192.168.0.0/24 dev eth0
    up ip route add 192.168.0.240 dev eth0 src 192.168.0.241
```

Then on your laptop (on debian), add the following to /etc/network/interfaces.
The lines below are based on having an ethernet interface with the name ens9.
```
iface ens9 inet static
    address 192.168.0.240 
    netmask 255.255.255.0
    # the following lines route all internet traffic not to the pi away from ens9 interface
    up ip route del 192.168.0.0/24 dev ens9
    up ip route add 192.168.0.241 dev ens9 src 192.168.0.240
```

On Mac OS, you don't need to change the network config on your laptop after changing the config on the pi. 

You should then be able to connect to your pi without wifi via
```ssh user@peach.local or ssh user@192.168.0.240```

Note that in this setup, all other internet traffic on the pi will be routed through the wlan0 interface.
