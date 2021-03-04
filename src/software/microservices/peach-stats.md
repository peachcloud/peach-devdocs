# peach-stats

[![GitHub logo](/assets/github_logo.png "peach-stats GitHub repository")](https://github.com/peachcloud/peach-stats) [![Build Status](https://travis-ci.com/peachcloud/peach-stats.svg?branch=master)](https://travis-ci.com/peachcloud/peach-stats) ![Version badge](https://img.shields.io/badge/version-0.1.3-<COLOR>.svg)

System statistics microservice module for PeachCloud. Provides a JSON-RPC wrapper around the [probes](https://crates.io/crates/probes) and [systemstat](https://crates.io/crates/systemstat) crates.

## JSON-API

| Method | Description | Returns |
| --- | --- | --- |
| `cpu_stats` | CPU statistics | `user`, `system`, `nice`, `idle` |
| `cpu_stats_percent` | CPU statistics as percentages | `user`, `system`, `nice`, `idle` |
| `disk_usage` | Disk usage statistics (array of disks) | `filesystem`, `one_k_blocks`, `one_k_blocks_used`, `one_k_blocks_free`, `used_percentage`, `mountpoint` |
| `load_average` | Load average statistics | `one`, `five`, `fifteen` |
| `mem_stats` | Memory statistics | `total`, `free`, `used` |
| `ping` | Microservice status | `success` if running |
| `uptime` | System uptime | `secs`, `nanos` |

## Directory Tree

```bash
.
├── Cargo.lock
├── Cargo.toml
├── README.md
└── src
    ├── error.rs        // custom StatError type & From implementations
    ├── lib.rs          // RPC server, methods & tests
    ├── main.rs         // init logger, call run() & catch application errors
    ├── stats.rs        // logic for stats methods exposed via RPC
    └── structs.rs      // data types for stats
```

## Setup

Clone this repo:

`git clone https://github.com/peachcloud/peach-stats.git`

Move into the repo and compile a release build:

`cd peach-stats`  
`cargo build --release`

Run the binary:

`./target/release/peach-stats`

## Environment

The JSON-RPC HTTP server address and port can be configured with the `PEACH_STATS_SERVER` environment variable:

`export PEACH_STATS_SERVER=127.0.0.1:5000`

When not set, the value defaults to `127.0.0.1:5113`.

Logging is made available with `env_logger`:

`export RUST_LOG=info`

Other logging levels include `debug`, `warn` and `error`.

## Debian Packaging

A `systemd` service file and Debian maintainer scripts are included in the `debian` directory, allowing `peach-stats` to be easily bundled as a Debian package (`.deb`). The `cargo-deb` [crate](https://crates.io/crates/cargo-deb) can be used to achieve this.

Install `cargo-deb`:

`cargo install cargo-deb`

Move into the repo:

`cd peach-stats`

Build the package:

`cargo deb`

The output will be written to `target/debian/peach-stats_0.1.0_arm64.deb` (or similar).

Build the package (aarch64):

`cargo deb --target aarch64-unknown-linux-gnu`

Install the package as follows:

`sudo dpkg -i target/debian/peach-stats_0.1.0_arm64.deb`

The service will be automatically enabled and started.

Uninstall the service:

`sudo apt-get remove peach-stats`

Remove configuration files (not removed with `apt-get remove`):

`sudo apt-get purge peach-stats`

## Example Usage

**Get CPU Statistics**

With microservice running, open a second terminal window and use `curl` to call server methods:

`curl -X POST -H "Content-Type: application/json" -d '{"jsonrpc": "2.0", "method": "cpu_stats", "id":1 }' 127.0.0.1:5113`

Server responds with:

`{"jsonrpc":"2.0","result":"{\"user\":4661083,\"system\":1240371,\"idle\":326838290,\"nice\":0}","id":1}`

**Get System Uptime**

With microservice running, open a second terminal window and use `curl` to call server methods:

`curl -X POST -H "Content-Type: application/json" -d '{"jsonrpc": "2.0", "method": "uptime", "id":1 }' 127.0.0.1:5113`

Server responds with:

`{"jsonrpc":"2.0","result":"{\"secs\":840968,\"nanos\":0}","id":1}`

## Licensing

AGPL-3.0

