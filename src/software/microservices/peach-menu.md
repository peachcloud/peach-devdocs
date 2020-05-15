# peach-menu

[![Build Status](https://travis-ci.com/peachcloud/peach-menu.svg?branch=master)](https://travis-ci.com/peachcloud/peach-menu)

[![GitHub logo](/assets/github_logo.png "peach-menu GitHub repository")](https://github.com/peachcloud/peach-menu)

OLED menu microservice module for PeachCloud. A state machine which listens for GPIO events (button presses) by subscribing to `peach-buttons` over websockets and makes [JSON-RPC](https://www.jsonrpc.org/specification) calls to relevant PeachCloud microservices (`peach-network`, `peach-oled`, `peach-stats`).

_Note: This module is a work-in-progress._

### Directory Tree

```bash
.
├── Cargo.lock
├── Cargo.toml
├── README.md
├── src
│   ├── buttons.rs          // JSON-RPC WebSocket client for peach-buttons
│   ├── error.rs            // custom MenuError type & From implementations
│   ├── lib.rs              // launch state machine & RPC client for buttons
│   ├── main.rs             // init logger, call run() & catch application errors
│   ├── network.rs          // JSON-RPC HTTP client for peach-network
│   ├── oled.rs             // JSON-RPC HTTP client for peach-oled
│   ├── state_machine.rs    // state machine & state_changer()
│   ├── states.rs           // state-specific logic called by state machine
│   ├── stats.rs            // JSON-RPC HTTP client for peach-stats
│   └── structs.rs          // data types used by RPC clients
```

### Button Code Mappings

```bash
0 => Center,  
1 => Left,  
2 => Right,  
3 => Up,  
4 => Down,  
5 => A,  
6 => B
```

### States

```bash
Home(0), // home menu
Home(1), // networking selected
Home(2), // system stats selected
Home(3), // display off selected
Home(4), // shutdown selected 
Logo, // logo splash screen
Network, // network device view
NetworkConf(0), // network configuration menu
NetworkConf(1), // client mode selected
NetworkConf(2), // access point mode selected
NetworkMode(0), // client mode activated
NetworkMode(1), // access point mode activated
OledPower(0), // oled display off
OledPower(1), // oled display on
Shutdown, // shutting down
Stats, // system statistics view
```

### Setup

Clone this repo:

`git clone https://github.com/peachcloud/peach-menu.git`

Move into the repo and compile:

`cd peach-menu`  
`cargo build --release`

Run the binary:

`./target/target/peach-menu`

_Note: Will currently panic if `peach_buttons` is not running (connection to ws server fails)._

### Environment

The JSON-RPC HTTP server address and port for the OLED microservice can be configured with the `PEACH_OLED_SERVER` environment variable:

`export PEACH_OLED_SERVER=127.0.0.1:5000`

When not set, the value defaults to `127.0.0.1:5112`.

Logging is made available with `env_logger`:

`export RUST_LOG=info`

Other logging levels include `debug`, `warn` and `error`.

### Resources

This work was made much, much easier by the awesome blog post titled [Pretty State Machine Patterns in Rust](https://hoverbear.org/2016/10/12/rust-state-machine-pattern/) by [hoverbear](https://hoverbear.org/about/). Thanks hoverbear!

### Licensing

AGPL-3.0
