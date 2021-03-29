# Debian Packaging

The microservices comprising the PeachCloud software suite are packaged for easy deployment and maintainance on Debian and derivative operating systems.

Each microservice repository includes a `debian` directory which includes a `systemd` service file. `deb` packages are created using the `cargo-deb` tool ([crate](https://crates.io/crates/cargo-deb)), which installs the systemd unit file to the correct location.

**Install cargo-deb:**

`cargo install cargo-deb`

**Create the package:**

`cargo deb --target aarch64-unknown-linux-gnu`

Note: the correct linker, objcopy and strip parameters must be defined in `~/.cargo/config` for the above command to execute successfully. Visit the [Compilation](/software/compilation.md) page and look under 'Configure the linker' for more details.

**Install the package:**

`sudo dpkg -i peach-network_0.1.0_arm64.deb`

The service will be automatically enabled and started.

**Uninstall the package:**

`sudo apt-get remove peach-network`

**Remove configuration files** (not removed with `apt-get remove`):

`sudo apt-get purge peach-network`


# Freight Repository 

[peach-package-builder](https://github.com/peachcloud/peach-package-builder) is a set of python scripts that run on the VPS and build all the PeachCloud microservice
packages and add them to the [Freight](https://github.com/rcrowley/freight) repository at apt.peachcloud.org. 

Documentation for peach-package-builder
cand be found [here](https://github.com/peachcloud/peach-package-builder).


# Installing packages from apt.peachcloud.org

To add the PeachCloud Debian package archive as an apt source, run the following commands from your Pi:

```
echo "deb http://apt.peachcloud.org/ buster main" > /etc/apt/sources.list.d/peach.list
wget -O - http://apt.peachcloud.org/pubkey.gpg | sudo apt-key add -
```

You can then install peach packages using apt:

```
sudo apt-get update
sudo apt-get install peach-oled
```




