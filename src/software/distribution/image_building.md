# PeachCloud Image Building 

PeachCloud microservices are packaged and installed onto a custom disc image running Debian,
using [peach-img-builder](https://github.com/peachcloud/peach-img-builder), a utility which runs on the PeachVPS. 

peach-img-builder is based off of a fork of [a vmdb2 script used for creating a Debian image for Raspberry pi](https://salsa.debian.org/raspi-team/image-specs/-/tree/master).

peach-img-builder creates a working Debian image, 
adds apt.peachcloud.org as an apt source,
and then uses peach-config to install all PeachCloud microservices.


## Publishing A New Image

To build a new peach image, compress the image, and publish it to http://releases.peachcloud.org/, on the VPS run:
```shell
cd /srv/peachcloud/automation/peach-img-builder
./build.sh
```

This script additionally creates a manifest file listing the versions of all PeachCloud microservices included in the image.
The image, log of the build, and the manifest, are all copied to folder named with the current date,
in `/var/www/releases.peachcloud.org/html/peach-imgs/`.