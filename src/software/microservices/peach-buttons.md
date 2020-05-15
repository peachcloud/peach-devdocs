# peach-buttons

[![Build Status](https://travis-ci.com/peachcloud/peach-buttons.svg?branch=master)](https://travis-ci.com/peachcloud/peach-buttons)

[![GitHub logo](/assets/github_logo.png "peach-buttons GitHub repository")](https://github.com/peachcloud/peach-buttons)

GPIO microservice module for handling button presses. `peach-buttons` implements a JSON-RPC server with [Publish-Subscribe extension](https://docs.rs/jsonrpc-pubsub/11.0.0/jsonrpc_pubsub/). Each button press results in a JSON-RPC request being sent over websockets to any subscribers. A button code for the pressed button is sent with the request to subscribers, allowing state-specific actions to be taken by the subscriber.

In the case of PeachCloud, the `peach-menu` microservice subscribes to `peach-buttons` in order to update the state of the menu after each button press.

_Note: This module is relatively stable but is still a work-in-progress._

### Directory Tree

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

### Pin to Button to Button Code Mappings

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

### Setup

Clone this repo:

`git clone https://github.com/peachcloud/peach-buttons.git`

Move into the repo and compile:

`cd peach-buttons`  
`cargo build --release`

Run the binary with sudo:

`sudo ./target/release/peach-buttons`

Logging is made availabe with `env_logger`:

`sudo RUST_LOG=info ./target/release/peach-buttons`

_Other logging levels include debug, warn and error._

### Testing Subscription

Request:
  
`{"id":1,"jsonrpc":"2.0","method":"subscribe_buttons"}`

Response:

`{"jsonrpc":"2.0","result":1,"id":1}`

Event:

`{"jsonrpc":"2.0","method":"button_press","params":[0]}`

### Licensing

AGPL-3.0
