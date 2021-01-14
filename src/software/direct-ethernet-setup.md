This file contains documentation for two different ways of working with peach cloud 
using a direct ethernet connection (useful for developmetn purposes)

Method 1:
If you are close to a router, you can plug your pi into the router via ethernet, 
and you should be able to ssh into the pi from any laptop connected to the same router
via wifi or ethernet.

Method 2:
Using a DHCP server on your laptop (instructions for a laptop running Debian). 

On your laptop, install isc-dhcp-server
`sudo apt-get install isc-dhcp-server`

Add the following configs:

In `/etc/dhcp/dhcpd.conf`, 
add the following section:
```
subnet 10.0.2.0 netmask 255.255.255.240 {
   range 10.0.2.2 10.0.2.14;
   option routers 10.0.2.1;
   host peach {
      hardware ethernet b8:27:eb:b1:b1:4e;
      fixed-address     10.0.2.4;
   }
}
```
Note that b8:27:eb:b1:b1:4e may need to be replaced with the address of your pi's ethernet interface,
which you can look up on the pi by running `ip a` on the pi. This address should be static.
This section of the config tells the dhcp server ot always give the specified client 
the specified IP address 10.0.2.4 when that client asks for an IP address.


In `/etc/default/isc-dhcp-server`,
add the following section with the name of your ethernet interface (in this case ens9),
```
INTERFACESv4="ens9"
INTERFACESv6=""
```

In `/etc/network/interfaces`,
set a static IP for you ethernet interface,
by adding this section:

```
auto ens9
iface ens9 inet static
  address 10.0.2.1
```

Then start the dhcp server:
```sudo systemctl start isc-dhcp-server```

Connect your pi to the laptop via an ethernet cable, and you should be able to ssh into the pi via
```ssh peach@10.0.2.4```

On the pi, internet traffic will still need to go through wlan0 interface.


On Mac OS, you don't need to change the network config on your laptop, 
if you simply enable internet sharing over ethernet, you should be able to connect to the pi.