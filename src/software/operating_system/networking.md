# Networking

PeachCloud has three primary network interfaces: `eth0`, `wlan0` and `ap0` (virtual interface). The device supports two wireless modes: client and access point (AP).

In client mode, the `wlan0` interface is managed by `wpa_supplicant`. This allows the PeachCloud device to connect to a WiFi access point in the vicinity. The IP address of the `wlan0` interface is dynamically-assigned by the WiFi router it is associated with. While the device is running in this mode, the `ap0` interface is set `DOWN` and the `dnsmasq` and `hostapd` processes are stopped.

In access point mode, the `ap0` interface is managed by `hostapd` - with DNS and DHCP leasing being handled by `dnsmasq`. The IP address of the `ap0` interface is set to `11.11.11.10` by default. Connected devices are leased IP addresses in the range of `11.11.11.11 - 11.11.11.30` (_for now_). While the device is running in this mode, the `wlan0` interface is set `DOWN` and the `wpa_supplicant` process is stopped. 

The `peach-network` microservice exposes `activate_ap()` and `activate_client()` RPC calls for simple switching of the networking mode. This functionality is also exposed by the menu system of the physical interface.
