# OpenWRT for RPi 4
[![build](https://github.com/damianperera/openwrt-rpi/actions/workflows/build.yml/badge.svg)](https://github.com/damianperera/openwrt-rpi/actions/workflows/build.yml)

Based off the works of [wulfy23/rpi4](https://github.com/wulfy23/rpi4) and includes the following enhancements:
- Configures a DHCP client so that OpenWRT can obtain an IP from the WAN network
- Configures Google DNS servers so that domain names can be resolved
- Configures a 5GHz WiFi access-point on an isolated LAN network bridged to the WAN network
- Allows access to the LuCi web interface from both the LAN and WAN networks
- Allows SSH access from both the LAN and WAN networks

For more information on the default features included in this build and usage instructions, please refer the [wulfy23/rpi4](https://github.com/wulfy23/rpi4) repository.

## Requirements
- RPi 4 Model B
- A fast SD card (ideally larger than 2 GB)

## Installation
- Download the RPi Imager ([macOS](https://downloads.raspberrypi.org/imager/imager_latest.dmg), [Windows](https://downloads.raspberrypi.org/imager/imager_latest.exe), [Ubuntu](https://downloads.raspberrypi.org/imager/imager_latest_amd64.deb))
- Download the [latest release](https://github.com/damianperera/openwrt-rpi/releases) from this repository
- Flash the `openwrt.img` file using the RPi Imager onto your SD card

  ![Screen Recording 2021-06-05 at 10 21 13 PM (3)](https://user-images.githubusercontent.com/15967502/120904641-7bfea700-c64d-11eb-8b29-1917431e2921.gif)
- Boot up your RPi and wait for the initial setup to complete (5-7 mins)
- Obtain the IP address of the device using your main network router and use it to connect to the LuCi web interface

## Release Schedule
Scheduled versions will be built and released once a week.
