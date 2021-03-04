# peach-probe

[![GitHub logo](/assets/github_logo.png "peach-probe GitHub repository")](https://github.com/peachcloud/peach-probe) ![Version badge](https://img.shields.io/badge/version-0.1.1-<COLOR>.svg)

Probe PeachCloud microservices to evaluate their state and ensure correct API responses.

`peach-probe` is a CLI tool for contract testing of the public API's exposed by PeachCloud microservices. 
It is composed of JSON-RPC clients which make calls to the methods of their respective servers and 
generates a report with the results.

`peach-probe` also makes use of `systemctl status` commands to test the status of all PeachCloud microservices.

This utility is intended to provide a rapid means of testing a deployed PeachCloud system and allow informed trouble-shooting in the case of errors.

## Installation

After adding releases.peachcloud.org to /etc/sources.list, as described [here](https://github.com/peachcloud/peach-vps/blob/main/README.md),
peach-probe can be installed by running:
`sudo apt-get install peach-probe`

## Usage

```bash
USAGE:
    peach-probe [FLAGS] [services]...

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
    -v, --verbose    prints successful endpoint calls in addition to errors

ARGS:
    <services>...   [possible values: peach_oled, peach_Network, peach_stats, peach_menu, peach_web,
                     peach_Buttons, peach_monitor]
```

If no service arguments are provided, peach-probe will query all services.

## Custom Port Numbers

If peach-microservices are running on ports other than the default ports, 
this can be specified using environmental variables as documented [here](https://github.com/peachcloud/peach-lib/blob/main/README.md).

## Todo

 - On detecting certain errors, suggest possible fixes
 - Finish querying of all peach-network endpoints

## Licensing

AGPL-3.0
