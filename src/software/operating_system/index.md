# Operating System

## Images

PeachCloud runs an _unofficial_ [preview image](https://people.debian.org/~gwolf/raspberrypi3/20190628/) of Debian 10 (Buster). Kernel version: 4.19.0-5-arm64.

Tested images are now available via the [Raspberry Pi Debian images](https://raspi.debian.net/) page on the Debian wiki. According to [Gunnar Wolf](https://gwolf.org/), project maintainer and Debian developer, the page: "should serve as a go-grab-it destination for regularly autobuilt images for the Raspberry Pi family". Note that these images are considered unofficial, ie. they are not part of an official Debian project.

## Why Debian?

In order to be flexible in supporting single-board computers beyond the Raspberry Pi alone, and to take full advantage of 64-bit support on ARMv8 devices, development for PeachCloud is being targeted at Debian ARM64.

Debian was chosen over the Raspbian operating system for several reasons:

 - Full ARM64 support, unlike Raspbian which uses 32 bit mode
 - Flexibile deployment to a range of single board computers
 - Access to latest code in Debian testing
 - Relatively lightweight base install ( < 2GB )

The downsides to this approach are a lack of support (most online resources assume use of Raspbian) and a minimal set of drivers on the base install.
