## DFNEXT systems

These instructions are for system DFNEXT099, please replace with a
corresponding number for your camera system.

### Login to the camera system...

...either [locally using ethernet wire or
WiFi](Logging_in_locally_via_WiFi_or_Ethernet.md "wikilink") or just use
screen and keyboard (HDMI, screen needs to be connected before powering
up the camera box).

### Disable and remove the old VPN config

` cd /etc/openvpn`

` systemctl stop openvpn@DFNEXT099.service`
` systemctl disable openvpn@DFNEXT099.service`
` systemctl mask openvpn@DFNEXT099.service`

` rm -rf DFNEXT099.conf DFNEXT099.tgz keys`

### Transfer the DFNEXT099.zip file provided by the DFN team to /etc/openvpn on the camera (scp/winscp/sftp/rsync or so)

### Unzip the configuration, password protected for security reasons

*Note: the password (hint) will arrive in a separate e-mail message, not
with the VPN config file.*

` cd /etc/openvpn`

` unzip DFNEXT099.zip`

... that creates file DFNEXT099.tar.gz

### Install and activate the new config - on the camera system as root connected using local IP

` root@DFNEXT099:/etc/openvpn# tar -xvzf DFNEXT099.tar.gz`
` client/DFNEXT099.conf`
` client/keys-gfo/`
` client/keys-gfo/DFNEXT099.crt`
` client/keys-gfo/ca.crt`
` client/keys-gfo/ta.key`
` client/keys-gfo/DFNEXT099.key`

` systemctl start openvpn-client@DFNEXT099.service`

` systemctl enable openvpn-client@DFNEXT099.service`

### Verify that it is running

` root@DFNEXT099:/etc/openvpn# systemctl status openvpn-client@DFNEXT099.service`
` ● openvpn-client@DFNEXT099.service - OpenVPN tunnel for DFNEXT099`
`   Loaded: loaded (/lib/systemd/system/openvpn-client@.service; enabled; vendor preset: enabled)`
`   Active: active (running) since Sun 2023-01-29 03:11:35 MST; 2 weeks 3 days ago`
`   Docs: man:openvpn(8)`
`         `[`https://community.openvpn.net/openvpn/wiki/Openvpn24ManPage`](https://community.openvpn.net/openvpn/wiki/Openvpn24ManPage)
`         `[`https://community.openvpn.net/openvpn/wiki/HOWTO`](https://community.openvpn.net/openvpn/wiki/HOWTO)
` Main PID: 15223 (openvpn)`
`   Status: "Initialization Sequence Completed"`
`   CGroup: /system.slice/system-openvpn\x2dclient.slice/openvpn-client@DFNEXT099.service`
`           └─15223 /usr/sbin/openvpn --suppress-timestamps --nobind --config DFNEXT099.conf`

` root@DFNEXT099:/etc/openvpn# ip a | grep tun`
` 15: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 100`
`    inet 10.1.23.99/20 brd 10.1.31.255 scope global tun0`

` root@DFNEXT099:/etc/openvpn# ping -c 3 dfn_vpn`
` PING dfnserver_vpn (10.1.16.1) 56(84) bytes of data.`
` 64 bytes from dfnserver_vpn (10.1.16.1): icmp_seq=1 ttl=64 time=213 ms`
` 64 bytes from dfnserver_vpn (10.1.16.1): icmp_seq=2 ttl=64 time=211 ms`
` 64 bytes from dfnserver_vpn (10.1.16.1): icmp_seq=3 ttl=64 time=220 ms`
` `
` --- dfnserver_vpn ping statistics ---`
` 3 packets transmitted, 3 received, 0% packet loss, time 2002ms`
` rtt min/avg/max/mdev = 211.841/215.206/220.417/3.755 ms`

## DFNKIT & DFNSMALL systems

*Note: this is an example for DFNKIT22. Replace the hostname according
to the system you are working on.*

### Remove the old config file and keys/certificates

(and also the old config tarbal \*.tgz)

` DFNKIT22 ~ # cd /etc/openvpn`
` DFNKIT22 openvpn # rm -rf DFNKIT22.conf DFNKIT22.tgz keys`

*Note: If not sure what to delete, the old certs can stay and the .conf
file actually gets overwritten when unwrapping the tar.gz. However,
there is no use for the old keys/certificate, as that is expired.*

### Then unwrap the new config into /etc/openvpn

(still in folder /etc/openvpn)

` DFNKIT22 openvpn # tar -xvzf DFNKIT22.tar.gz`

That creates new .conf and certs in keys-gfo subfolder.

### Then restart the openvpn client service

` DFNKIT22 openvpn # service openvpn restart`

### Verify that it is running

` DFNKIT22 ~ # service openvpn status`
` [ ok ] VPN 'DFNKIT22' is running.`

` DFNKIT22 openvpn #  ip a | grep tun`
` 4: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 100`
`     inet 10.1.21.22/20 brd 10.1.31.255 scope global tun0`

` DFNKIT22 openvpn # ping -c 3 dfn_vpn`
` PING dfnserver_vpn (10.1.16.1) 56(84) bytes of data.`
` 64 bytes from dfnserver_vpn (10.1.16.1): icmp_req=1 ttl=64 time=234 ms`
` 64 bytes from dfnserver_vpn (10.1.16.1): icmp_req=2 ttl=64 time=234 ms`
` 64 bytes from dfnserver_vpn (10.1.16.1): icmp_req=3 ttl=64 time=233 ms`

` --- dfnserver_vpn ping statistics ---`
` 3 packets transmitted, 3 received, 0% packet loss, time 2003ms`
` rtt min/avg/max/mdev = 233.998/234.104/234.203/0.083 ms`