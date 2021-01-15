# Networking

PeachCloud has three primary network interfaces: `eth0`, `wlan0` and `ap0` (virtual interface). The device supports two wireless modes: client and access point (AP).

In client mode, the `wlan0` interface is managed by `wpa_supplicant`. This allows the PeachCloud device to connect to a WiFi access point in the vicinity. The IP address of the `wlan0` interface is dynamically-assigned by the WiFi router it is associated with. While the device is running in this mode, the `ap0` interface is set `DOWN` and the `dnsmasq` and `hostapd` processes are stopped.

In access point mode, the `ap0` interface is managed by `hostapd` - with DNS and DHCP leasing being handled by `dnsmasq`. The IP address of the `ap0` interface is set to `11.11.11.10` by default. Connected devices are leased IP addresses in the range of `11.11.11.11 - 11.11.11.30` (_for now_). While the device is running in this mode, the `wlan0` interface is set `DOWN` and the `wpa_supplicant` process is stopped. 

The `peach-network` microservice exposes `activate_ap()` and `activate_client()` RPC calls for simple switching of the networking mode. This functionality is also exposed by the menu system of the physical interface.


## Direct Ethernet Setup

This file contains documentation for two different ways of working with PeachCloud using a direct ethernet connection (useful for development purposes).

### Method 1

If you are close to a router, you can plug your Pi into the router via ethernet and you should be able to SSH into the Pi from any laptop connected to the same router via WiFi or ethernet.

### Method 2

Using a DHCP server on your laptop (instructions for a laptop running Debian). 

**Install the DHCP server:**

`sudo apt-get install isc-dhcp-server`

**Configure the DHCP server:**

In `/etc/dhcp/dhcpd.conf`, add the following section:

```plaintext
subnet 10.0.2.0 netmask 255.255.255.240 {
   range 10.0.2.2 10.0.2.14;
   option routers 10.0.2.1;
   host peach {
      hardware ethernet b8:27:eb:b1:b1:4e;
      fixed-address     10.0.2.4;
   }
}
```

Note that `b8:27:eb:b1:b1:4e` may need to be replaced with the address of your Pi's ethernet interface, which you can look up by running `ip a` on the Pi. This address should be static. The `fixed-address` section of the config tells the DHCP server to always give the specified client (`peach`) the `10.0.2.4` IP address.


In `/etc/default/isc-dhcp-server`, add the following section with the name of your ethernet interface (in this case `ens9`):

```plaintext
INTERFACESv4="ens9"
INTERFACESv6=""
```

In `/etc/network/interfaces`, set a static IP for your ethernet interface by adding this section:

```plaintext
auto ens9
iface ens9 inet static
  address 10.0.2.1
```

**Start the DHCP server:**

`sudo systemctl start isc-dhcp-server`

**Connect the ethernet cable:**

Connect your Pi to the laptop via an ethernet cable. You should then be able to SSH into the Pi using the following command:

`ssh peach@10.0.2.4`

_Note:_ On the Pi, internet traffic will still need to go through `wlan0` interface.

On Mac OS you don't need to change the network config on your laptop. Simply enable internet sharing over ethernet and you should be able to connect to the Pi.

