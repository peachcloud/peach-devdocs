# peach-buttons

[![GitHub logo](/assets/github_logo.png "peach-buttons GitHub repository")](https://github.com/peachcloud/peach-buttons) [![Build Status](https://travis-ci.com/peachcloud/peach-buttons.svg?branch=master)](https://travis-ci.com/peachcloud/peach-buttons) ![Version badge](https://img.shields.io/badge/version-0.1.3-<COLOR>.svg)

GPIO microservice module for handling button presses. `peach-buttons` implements a JSON-RPC server with [Publish-Subscribe extension](https://docs.rs/jsonrpc-pubsub/11.0.0/jsonrpc_pubsub/). Each button press results in a JSON-RPC request being sent over websockets to any subscribers. A button code for the pressed button is sent with the request to subscribers, allowing state-specific actions to be taken by the subscriber.

In the case of PeachCloud, the `peach-menu` microservice ([repo](https://github.com/peachcloud/peach-menu)) subscribes to `peach-buttons` in order to update the state of the menu after each button press.

_Note: This module is relatively stable but is still a work-in-progress._

## Directory Tree

```bash
.
├── Cargo.lock
├── Cargo.toml
├── README.md
├── src
│   ├── error.rs        // custom ButtonError type and From implementation
│   ├── interrupt.rs    // interrupt handler with GPIO polling
│   ├── lib.rs          // RPC server and pubsub handler, pin definitions
│   └── main.rs         // init logger, call run() & catch application errors
```

## Pin to Button to Button Code Mappings

```bash
4 => Center => 0,
27 => Left => 1,
23 => Right => 2,
17 => Up => 3,
22 => Down => 4,
5 => A => 5,
6 => B => 6
```

_Note: `peach-buttons` utilizes the GPIO character device ABI. This API, stabilized with Linux v4.4, deprecates the legacy sysfs interface to GPIOs that is planned to be removed from the upstream kernel after year 2020._

## Environment

The JSON-RPC WS server address and port can be configured with the `PEACH_BUTTONS_SERVER` environment variable:

`export PEACH_BUTTONS_SERVER=127.0.0.1:5000`

When not set, the value defaults to `127.0.0.1:5111`.

Logging is made availabe with `env_logger`:

`export RUST_LOG=info`

Other logging levels include `debug`, `warn` and `error`.

## Setup

Clone this repo:

`git clone https://github.com/peachcloud/peach-buttons.git`

Move into the repo and compile:

`cd peach-buttons`  
`cargo build --release`

Run the binary with sudo:

`sudo ./target/release/peach-buttons`

## Debian Packaging

A `systemd` service file and Debian maintainer scripts are included in the `debian` directory, allowing `peach-buttons` to be easily bundled as a Debian package (`.deb`). The `cargo-deb` [crate](https://crates.io/crates/cargo-deb) can be used to achieve this.

Install `cargo-deb`:

`cargo install cargo-deb`

Move into the repo:

`cd peach-buttons`

Build the package:

`cargo deb`

The output will be written to `target/debian/peach-buttons_0.1.0_arm64.deb` (or similar).

Build the package (aarch64):

`cargo deb --target aarch64-unknown-linux-gnu`

Install the package as follows:

`sudo dpkg -i target/debian/peach-buttons_0.1.0_arm64.deb`

The service will be automatically enabled and started.

Uninstall the service:

`sudo apt-get remove peach-buttons`

Remove configuration files (not removed with `apt-get remove`):

`sudo apt-get purge peach-buttons`

## Testing Subscription

Request:

`{"id":1,"jsonrpc":"2.0","method":"subscribe_buttons"}`

Response:

`{"jsonrpc":"2.0","result":1,"id":1}`

Event:

`{"jsonrpc":"2.0","method":"button_press","params":[0]}`

## Licensing

AGPL-3.0
