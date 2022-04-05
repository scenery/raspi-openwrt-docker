# [Raspberry Pi OpenWrt Docker](https://github.com/scenery/raspi-openwrt-docker)

This repository is used to build custom OpenWrt Docker Images for **Raspberry Pi 4B**. 

***Notice:** Only keep the packages for personal needs that means this Docker Image is not suitable for everyone.*

### Pull the image
```
docker pull hurt/openwrt-rpi
```

### Start a container
You must turn on the promiscuous mode and create a docker network at first:
```
ip link set eth0 promisc on 
docker network create -d macvlan --subnet=10.0.0.0/24 --gateway=10.0.0.1 -o parent=eth0 macnet
```

*You have to change above subnet and gateway address to the same segment of your Rasyberry Pi. 

After this, you can use ```doker network ls``` to check it. Then start the container:
```
docker run --restart always --name openwrt -d --network macnet --privileged hurt/openwrt-rpi /sbin/init
```
### Edit network parameters
Run bash command in the openwrt container:
```
docker exec -it openwrt bash
```
Edit network config:
```
vim /etc/config/network
```

You only have to change ```option ipaddr```, ```option gateway```, ```option dns``` parameters of **interface 'lan'**, for example:
```
config interface 'lan'
        option type 'bridge'
        option ifname 'eth0'
        option proto 'static'
        option netmask '255.255.255.0'
        option ip6assign '60'
        option ipaddr '10.0.0.6'
        option gateway '10.0.0.1'
        option dns '10.0.0.1'
```

Restart netowrk:
```
/etc/init.d/network restart
```

Now you can use OpenWrt's IP address to access the control panel via browser. Default account: ```root```, password: ```password```

### Thanks

SuLingGG/OpenWrt-Docker:

<https://github.com/SuLingGG/OpenWrt-Docker>

P3TERX/Actions-OpenWrt:

<https://github.com/P3TERX/Actions-OpenWrt>

OpenWrt Source Repository:

<https://github.com/openwrt/openwrt/>

Lean's OpenWrt source:

<https://github.com/coolsnowwolf/lede>

CTCGFW's Team:

<https://github.com/project-openwrt>
