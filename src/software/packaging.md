# Debian Packaging

The microservices comprising the PeachCloud software suite are packaged for easy deployment and maintainance on Debian and derivative operating systems.

Each microservice repository includes a `debian` directory which includes a `systemd` service file and Debian maintainer scripts, including `postinst`, `postrm` and `prerm` scripts. These scripts define behavior for installing and uninstalling the respective services. `deb` packages are created using the `cargo-deb` tool ([crate](https://crates.io/crates/cargo-deb)).

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
