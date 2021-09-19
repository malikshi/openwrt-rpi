# OpenWrt for RPi 4
[![build](https://github.com/damianperera/openwrt-rpi/actions/workflows/build.yml/badge.svg)](https://github.com/damianperera/openwrt-rpi/actions/workflows/build.yml) [![GitHub stars](https://img.shields.io/github/stars/damianperera/openwrt-rpi)](https://github.com/damianperera/openwrt-rpi/stargazers) [![GitHub issues](https://img.shields.io/github/issues/damianperera/openwrt-rpi)](https://github.com/damianperera/openwrt-rpi/issues) [![GitHub license](https://img.shields.io/github/license/damianperera/openwrt-rpi)](https://github.com/damianperera/openwrt-rpi/blob/main/LICENSE) [![Twitter](https://img.shields.io/twitter/url?style=social&url=https%3A%2F%2Fgithub.com%2Fdamianperera%2Fopenwrt-rpi)](https://twitter.com/intent/tweet?url=https%3A%2F%2Fgithub.com%2Fdamianperera%2Fopenwrt-rpi)

Based off the works of [wulfy23/rpi4](https://github.com/wulfy23/rpi4) and includes the following enhancements:
- Configures a DHCP client so that OpenWrt can obtain an IP address from the WAN network using the Ethernet port
- Configures Google DNS servers so that domain names can be resolved <sup>[1](#recommendations)</sup>
- Configures a 5 GHz WiFi access-point on an isolated LAN network using the onboard WiFi adapter <sup>[2](#recommendations)</sup>
- Allows access to the LuCi web interface from both the LAN and WAN networks <sup>[3](#recommendations)</sup>
- Allows SSH access from both the LAN and WAN networks <sup>[3](#recommendations)</sup>

The above enhancements are intended to provide plug-and-play (PnP) support for the RPi 4, so that you can simply flash the image and get started immediately.

## What is OpenWrt?
OpenWrt for RPi 4 allows you to use your RPi 4 Model B as a configurable router, through which you can add an always-on VPN connection, network-wide adblock, a BitTorrent client and more. Simply flash the downloadable image (ref. below [instructions](#installation)) and get started immediately.
> With OpenWrt, users/developers can use their router to run a BitTorrent client, enable VPN, create a guest Wi-Fi network, analyze network traffic, do traffic-shaping or apply QoS rules on packets. The router can also run servers: SSH (and do SSH tunneling), IRC server, HTTP server, FTP server, etc. Mesh networking, port knocking, firewalling, wireless bridging, file sharing and real-time monitoring are some other useful features. When configured as a public hotspot, OpenWrt provides a number of functions to manage the hotspot.
> 
> OpenWrt can also connect to printers, webcams, modems and soundcards. In general, an OpenWrt device can work with any hardware that has Linux support.
> 
> ~ _[Devopedia](https://devopedia.org/openwrt)_

### Use-cases
1. Portable WiFi access point
2. A secure WiFi/LAN router that routes all client traffic via a VPN
    - Secure access to your home network using WireGuard or OpenVPN
    - Secure your internet access and prevent your ISP from snooping on your internet activity
4. A DNS server that blocks ads for all client traffic (adblock)
5. Reduce latency even during heavy traffic
6. Bypass restrictions and access Onion sites for all connected clients via Tor
7. Create additional (highly configurable) guest WiFi access points
8. MultiWAN Manager(MWAN3) configured by default, enabled it in LuCI > system > startup

For more use-cases visit the [OpenWrt Wiki](https://openwrt.org/reasons_to_use_openwrt#extensibility).

## MultiWAN Manager
- Go to MultiWAN Manager - Interfaces, and edit `Tracking hostname or IP address` with your WAN gateway and IP Address from your WAN

## Installation OpenClash OpenWRT
- Install Requirement
```sh
opkg update
opkg install coreutils-nohup bash iptables dnsmasq-full jsonfilter ca-certificates ipset ip-full iptables-mod-tproxy iptables-mod-extra libcap libcap-bin ruby ruby-yaml
```
- Install Openclash ipk
```sh
mkdir /etc/openclash
wget -O /root/luci-app-openclash.ipk $(curl -sL https://github.com/vernesong/OpenClash/releases | grep luci-app-openclash_ | sed -e 's/\"//g' -e 's/ //g' -e 's/rel=.*//g' -e 's#<ahref=#http://github.com#g' | awk 'FNR <= 1')
opkg install /root/luci-app-openclash.ipk
rm -f /root/luci-app-openclash.ipk
```
- Reboot your OpenWRT router

## Installation V2rayA OpenWRT
- Install Requirement
```sh
opkg update
opkg install ca-certificates wget unzip tar
opkg install ip-full kmod-ipt-nat6 iptables-mod-tproxy iptables-mod-filter iptables-mod-conntrack-extra iptables-mod-extra
```

- Create Directory for V2rayA
```sh
mkdir /etc/v2raya
mkdir /etc/v2raya/bin
mkdir /usr/share/v2ray
```

- Install V2ray-core

**FOR RPI4B AND STB**

1st install directly from v2fly
```sh
version=$(curl -s https://api.github.com/repos/v2fly/v2ray-core/releases | jq -r .[].tag_name | head -1)
wget -c -q --show-progress -P /etc/v2raya/bin/ "https://github.com/v2fly/v2ray-core/releases/download/${version}/v2ray-linux-arm64-v8a.zip"
unzip -d /etc/v2raya/bin /etc/v2raya/bin/v2ray-linux-arm64-v8a.zip
rm /etc/v2raya/bin/config.json
rm /etc/v2raya/bin/geoip.dat
rm /etc/v2raya/bin/geosite.dat
rm /etc/v2raya/bin/geoip-only-cn-private.dat
rm /etc/v2raya/bin/v2ray-linux-arm64-v8a.zip
rm /etc/v2raya/bin/vpoint_socks_vmess.json
rm /etc/v2raya/bin/vpoint_vmess_freedom.json
rm -rf /etc/v2raya/bin/systemd
```
2nd install geodata
```sh
wget -O /usr/share/v2ray/geosite.dat https://raw.githubusercontent.com/v2rayA/dist-v2ray-rules-dat/master/geosite.dat
wget -O /usr/share/v2ray/geoip.dat https://raw.githubusercontent.com/v2rayA/dist-v2ray-rules-dat/master/geoip.dat
wget -O /usr/share/v2ray/LoyalsoldierSite.dat https://raw.githubusercontent.com/v2rayA/dist-v2ray-rules-dat/master/geosite.dat
```



- Install V2raya with binary

1st Check your type architecture
```sh
cat /etc/*release
```
For arm64 like RPI4B
```sh
wget -O /usr/bin/v2raya "https://github.com/malikshi/openwrt-rpi/raw/main/bin-or-ipk/v2raya_arm64"
chmod +x /usr/bin/v2raya
```
For arm Orange Pi Zero(Opiz)
```sh
wget -O /usr/bin/v2raya "https://github.com/malikshi/openwrt-rpi/raw/main/bin-or-ipk/v2raya_arm_a7"
chmod +x /usr/bin/v2raya
```
For aarch64_cortex-a53 STB
```sh
wget -O /usr/bin/v2raya "https://github.com/malikshi/openwrt-rpi/raw/main/bin-or-ipk/v2raya_arm64_a5"
chmod +x /usr/bin/v2raya
```

- Setting V2rayA Init
```sh
wget -O /etc/init.d/v2raya "https://github.com/malikshi/openwrt-rpi/raw/main/bin-or-ipk/v2raya.init"
chmod +x /etc/init.d/v2raya
```

- Setting V2rayA Startup Config

```sh
wget -O /etc/config/v2raya "https://github.com/malikshi/openwrt-rpi/raw/main/bin-or-ipk/v2raya.config"
uci commit v2raya
/etc/init.d/v2raya enable
```

- Reboot & Access V2rayA `IP-OpenWRT:2017` example: `192.168.1.1:2017`
- Setting RoutingA for V2rayA dedicated for isp Ts*l : [RoutingA](https://github.com/malikshi/openwrt-rpi/blob/main/bin-or-ipk/routingA.conf)

## Requirements
- RPi 4 Model B
- A fast SD card (ideally larger than 2 GB)

## Installation
1. Download the RPi Imager ([macOS](https://downloads.raspberrypi.org/imager/imager_latest.dmg), [Windows](https://downloads.raspberrypi.org/imager/imager_latest.exe), [Ubuntu](https://downloads.raspberrypi.org/imager/imager_latest_amd64.deb))
2. Download the [latest release](https://github.com/damianperera/openwrt-rpi/releases/latest/download/openwrt.img.gz) from this repository
3. Flash the `openwrt.img.gz` file using the RPi Imager onto your SD card

    ![ezgif com-gif-maker](https://user-images.githubusercontent.com/15967502/121747825-456ed380-cb08-11eb-9fad-4398a87d989d.gif)
  
4. Connect your RPi's onboard Ethernet port to your main network router's LAN port and boot up the RPi
5. Wait for the initial setup to complete (5-7 mins)
6. Obtain the IP address of the RPi using your main network (WAN) router and use it to connect to the LuCi Web Interface over your preferred browser. You can also connect to LuCi via the WiFi access point created by OpenWrt by heading over to `192.168.1.1`

## Default Credentials <sup>[2](#recommendations)</sup>
- **LuCi Web Interface:**
  - Username: `root`
  - Password: `root`
- **WiFi Access Point:** 
  - SSID: `Malik WiFi`
  - PassKey: `changeThisPassKey`

## Release Schedule
Automated builds have been scheduled for once a week. Urgent hotfixes will be manually triggered.

---

### Notes
- For more information on the default features included in this build plus usage instructions, please refer the [wulfy23/rpi4](https://github.com/wulfy23/rpi4) repository.
- In the above context WAN refers to your home/office network, and LAN refers to the network created by OpenWrt.
- Openclash OpenWRT, Credit [vernesong/OpenClash](https://github.com/vernesong/OpenClash)
- V2ray OpenWRT, Credit [kuoruan/openwrt-v2ray](https://github.com/kuoruan/openwrt-v2ray)
- V2rayA OpenWRT, Credit [v2rayA/v2rayA](https://github.com/v2rayA/v2rayA)

### Recommendations
<sup>1</sup> Change to a more secure DNS provider after installation<br>
<sup>2</sup> Change the default credentials after installation<br>
<sup>3</sup> Restrict LuCi and SSH access to the LAN interface after completing configuration<br>
