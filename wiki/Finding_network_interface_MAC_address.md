The MAC address of a specific network interface (wired, wifi) may be
needed, eg on sites where one wants to assign the device a static IP
address.

The MAC addresses of the (wired) ethernet ports are on stickers on the
connectors inside the box - on the embedded PC board.

Alternatively, you can list them if you log in to the observatory system
using ssh, the command is "ip a" or "ifconfig", without parameter lists
all interfaces, or you can specify an interface like eth1. That hat
includes wifi interface wlan0. below is an example for DFNEXT009, with
the relevant information in bold face.

*Note: this observatory is connected to the Internet using mobile
network (ppp0), therefore the WAN ethernet interface has no IP address
assigned, as it not connected to anything.*

`root@DFNEXT009:~# ip a`
`1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1`
`   link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00`
`   inet 127.0.0.1/8 scope host lo`
`      valid_lft forever preferred_lft forever`
`   inet6 ::1/128 scope host`
`      valid_lft forever preferred_lft forever`
`2: `**`eth0`**`: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000`
`   link/ether `**`00:03:1d:11:30:f4`**` brd ff:ff:ff:ff:ff:ff`
`   inet 172.16.1.101/24 brd 172.16.1.255 scope global eth0`
`      valid_lft forever preferred_lft forever`
`3: `**`eth1`**`: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc pfifo_fast state DOWN group default qlen 1000`
`   link/ether `**`00:03:1d:11:30:f3`**` brd ff:ff:ff:ff:ff:ff`
`4: wwan0: <BROADCAST,MULTICAST,NOARP> mtu 1500 qdisc noop state DOWN group default qlen 1000`
`   link/ether 72:4b:06:a6:01:07 brd ff:ff:ff:ff:ff:ff`
`5: `**`wlan0`**`: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000`
`   link/ether `**`18:d6:c7:10:29:5f`**` brd ff:ff:ff:ff:ff:ff`
`   inet 172.16.0.101/24 brd 172.16.0.255 scope global wlan0`
`      valid_lft forever preferred_lft forever`
`   inet6 fe80::1ad6:c7ff:fe10:295f/64 scope link`
`      valid_lft forever preferred_lft forever`
`6: ppp0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 3`
`   link/ppp`
`   inet 10.113.20.244 peer 10.64.64.64/32 scope global ppp0`
`      valid_lft forever preferred_lft forever`
`7: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 100`
`   link/none`
`   inet 10.1.23.9 peer 10.1.24.9/32 scope global tun0`
`      valid_lft forever preferred_lft forever`
`   inet6 fe80::b3b7:c886:84ea:448/64 scope link flags 800`
`      valid_lft forever preferred_lft forever`