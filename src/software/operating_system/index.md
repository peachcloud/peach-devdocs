# Operating System

PeachCloud runs an _unofficial_ [preview image](https://people.debian.org/~gwolf/raspberrypi3/20190628/) of Debian 10 (Buster). Kernel version: 4.19.0-5-arm64.

Debian was chosen over the Raspbian operating system for several reasons:

 - Full ARM64 support, unlike Raspbian which uses 32 bit mode
 - Flexibile deployment to a range of single board computers
 - Access to latest code in Debian testing
 - Relatively lightweight base install ( < 2GB )

The downsides to this approach are a lack of support (most online resources assume use of Raspbian) and a minimal set of drivers on the base install.
