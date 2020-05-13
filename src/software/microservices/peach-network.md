# peach-network

[![Build Status](https://travis-ci.com/peachcloud/peach-network.svg?branch=master)](https://travis-ci.com/peachcloud/peach-network)

[![GitHub logo](/assets/github_logo.png "peach-network GitHub repository")](https://github.com/peachcloud/peach-network)

Networking microservice module for PeachCloud. Query and configure device interfaces using [JSON-RPC](https://www.jsonrpc.org/specification) over HTTP.

Interaction with wireless interfaces occurs primarily through the [wpactrl crate](https://docs.rs/wpactrl/0.3.1/wpactrl/) which provides "a pure-Rust lowlevel library for controlling wpasupplicant remotely". This approach is akin to using `wpa_cli` (a WPA command line client).

_Note: This module is a work-in-progress._

### JSON-RPC API

| Method | Parameters | Description |
| --- | --- | --- |
| `activate_ap` | | Activate WiFi access point (stop `wpa_supplicant` and start `hostapd` and `dnsmasq`) |
| `activate_client` | | Activate WiFi client connection (stop `hostapd` and `dnsmasq` and start `wpa_supplicant`) |
| `add_wifi` | `ssid`, `pass` | Add WiFi credentials to `wpa_supplicant.conf` |
| `disable_wifi` | `id`, `iface` | Disable connection with AP represented by given id |
| `disconnect_wifi` | `iface` | Disconnect given interface |
| `get_id` | `iface`, `ssid` | Return ID of given SSID |
| `get_ip` | `iface` | Return IP of given network interface |
| `get_rssi` | `iface` | Return average signal strength (dBm) for given interface |
| `get_rssi_percent` | `iface` | Return average signal strength (%) for given interface |
| `get_ssid` | `iface` | Return SSID of currently-connected network for given interface |
| `get_state` | `iface` | Return state of given interface |
| `get_status` | `iface` | Return status parameters for given interface |
| `get_traffic` | `iface` | Return network traffic for given interface |
| `if_checker` | | Run AP / client-mode configuration script |
| `list_networks` | | List all networks saved in wpasupplicant config |
| `new_password` | `id`, `iface`, `password` | Set a new password for given network id and interface |
| `ping` | | Respond with `success` if microservice is running |
| `reassociate_wifi` | `iface` | Reassociate with current AP for given interface |
| `reconfigure_wifi` | | Force wpa_supplicant to re-read its configuration file |
| `reconnect_wifi` | `iface` | Disconnect and reconnect given interface |
| `remove_wifi` | `id`, `iface` | Remove WiFi credentials for given network id and interface |
| `save_config` | | Save configuration changes to `wpa_supplicant.conf` |
| `scan_networks` | `iface` | List SSID, flags (security), frequency and signal level for all networks in range of given interface |
| `select_network` | `id`, `iface` | Disable other networks and attempt connection with AP represented by given id |

### Directory Tree

```
.
├── Cargo.lock
├── Cargo.toml
├── README.md
├── src
│   ├── error.rs        // custom NetworkError type & From implementations
│   ├── lib.rs          // RPC server, methods & tests
│   ├── main.rs         // init logger, call run() & catch application errors
│   └── network.rs      // logic for network methods exposed via RPC
```

### Setup

Clone this repo:

`git clone https://github.com/peachcloud/peach-network.git`

Move into the repo and compile:

`cd peach-network`  
`cargo build --release`

Run the binary (sudo needed to satisfy permission requirements):

`sudo ./target/release/peach-network`

### Environment

The JSON-RPC HTTP server address and port can be configured with the `PEACH_NETWORK_SERVER` environment variable:

`export PEACH_NETWORK_SERVER=127.0.0.1:5000`

When not set, the value defaults to `127.0.0.1:5110`.

Logging is made available with `env_logger`:

`export RUST_LOG=info`

Other logging levels include `debug`, `warn` and `error`.

### Example Usage

**Retrieve IP address for wlan0**

With microservice running, open a second terminal window and use `curl` to call server methods:

`curl -X POST -H "Content-Type: application/json" -d '{"jsonrpc": "2.0", "method": "get_ip", "params" : {"iface": "wlan0" }, "id":1 }' 127.0.0.1:5000`

Server responds with:

`{"jsonrpc":"2.0","result":"192.168.1.21","id":1}`

**Retrieve SSID of connected access point for wlan1**

`curl -X POST -H "Content-Type: application/json" -d '{"jsonrpc": "2.0", "method": "get_ssid", "params" : {"iface": "wlan1" }, "id":1 }' 127.0.0.1:5000`

Server response when interface is connected:

`{"jsonrpc":"2.0","result":"Home","id":1}`

Server response when interface is not connected:

`{"jsonrpc":"2.0","error":{"code":-32003,"message":"Failed to retrieve SSID for wlan1. Interface may not be connected."},"id":1}`

**Retrieve list of SSIDs for all networks in range of wlan0**

`curl -X POST -H "Content-Type: application/json" -d '{"jsonrpc": "2.0", "method": "scan_networks", "params" : {"iface": "wlan0" }, "id":1 }' 127.0.0.1:5000`

Server response when interface is connected:

`{"jsonrpc":"2.0","result":"[\"Home\",\"TP-LINK_254700\"]","id":1}`

Server response when interface is not connected:

`{"jsonrpc":"2.0","error":{"code":-32006,"message":"No networks found in range of wlan0"},"id":1}`

**Retrieve network traffic statistics for wlan1**

`curl -X POST -H "Content-Type: application/json" -d '{"jsonrpc": "2.0", "method": "get_traffic", "params" : {"iface": "wlan1" }, "id":1 }' 127.0.0.1:5000`

Server response if interface exists:

`{"jsonrpc":"2.0","result":"{\"received\":26396361,\"transmitted\":22352530}","id":1}`

Server response when interface is not found:

`{"jsonrpc":"2.0","error":{"code":-32004,"message":"Failed to retrieve network traffic for wlan3. Interface may not be connected"},"id":1}`

### Licensing

AGPL-3.0
