# Compilation

Since the majority of PeachCloud is written in the Rust programming language, a compiler and associated toolchain(s) is required to build the software from source. Visit the [rustup](https://rustup.rs/) website to download the Rust toolchain installer. In addition to the default `stable` installation of Rust, many PeachCloud crates require the `nightly` toolchain channel in order to compile. Instructions for installing and updating toolchains can be found in the README of the [rustup GitHub repo](https://github.com/rust-lang/rustup).

## Cross-Compilation

While PeachCloud crates can be compiled directly on the Raspberry Pi or equivalent single board computer, this process is resource intensive and relatively slow - especially when compiling release builds. Cross-compilation is an effective means of leveraging greater compute resources to reduce compilation time.

These instructions cover cross-compilation for Debian Buster running on a Raspberry Pi 3B+, using a host machine running Debian Stretch with an x86_64 architecture:

**Install target platform:**

`rustup target add aarch64-unknown-linux-gnu`

**Install toolchain:**

`rustup toolchain install nightly-aarch64-unknown-linux-gnu`

**Install aarch64-linux-gnu-gcc:**

`sudo apt-get install gcc-aarch64-linux-gnu`

**Configure the linker:**

`export CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_LINKER=/usr/bin/aarch64-linux-gnu-gcc`

Alternatively, create a file named `config` in the `~/.cargo` directory of the home user and add the following:

```bash
[target.aarch64-unknown-linux-gnu]
linker = "aarch64-linux-gnu-gcc"
objcopy = { path ="aarch64-linux-gnu-objcopy" }
strip = { path ="aarch64-linux-gnu-strip" }
```

The `objcopy` and `strip` parameters are needed by `cargo deb` when cross-compiling Debian packages.

**Compile release build:**

`cargo build --release --target=aarch64-unknown-linux-gnu`

The generated binary will be saved at `target/aarch64-unknown-linux-gnu/release/name_of_crate`
