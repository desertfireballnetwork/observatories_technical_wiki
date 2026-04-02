This page describes the configuration of WiFi networking in DFN camera
systems. DFNEXT type has WiFi interface installed, in the other systems
it is optional. The default configuration is master mode, the camera
system box acts as a WiFi access point (a WiFi network where a user in
proximity of the deployed site can connect to the system wirelessly).
The network name is the camera system hostname (eg DFNEXT007).

On some sites, or for testing before deployment, it might be useful to
set the WiFi interface in camera system to client mode and connect to an
existing WiFi network (WLAN). That allows Internet access to the camera
system and remote access via LAN IP range and DFN VPN as well.

Both master and client configuration options are pre-set in
/etc/network/interfaces, the one that is not currently active is
commended out. When changing the configuration, it is recommended ti
bring the network interface down (ifdown wlan0), change the config file
and then activate the interface again (ifup wlan0).

The default (master mode) state is (here displayed only the USB WiFi
sub-section of the /etc/network/interfaces file):

`### USB WiFi stick`
`auto wlan0`
`allow-hotplug wlan0`
`### DHCP`
`#iface wlan0 inet dhcp`
`### static`
`#set IP address that is not in the DHCP pool of local wifi router`
`#also subnet must not clash with eth1 subnet above`
**`iface wlan0 inet static`**
**`address 172.16.0.101`**
**`network 172.16.0.0`**
**`netmask 255.255.255.0`**
**`post-up service isc-dhcp-server restart`**
`   ### WPA/WPA2 section for client mode ###`
`#      wpa-ssid test1`
`#      wpa-psk fireball`
`      ### following two options are not necessary `
`      ### - but the WiFi AP must be set to use encoding TKIP, not AES!`
`      #wpa-key-mgmt WPA-PSK`
`      #wpa-group TKIP`
`   ### WEP section for client mode ###`
`      #wireless-essid test1`
`      #wireless-key F9674C82E0`
`   ### hostapd - all client mode settings must be commented out ###`
`      `**`hostapd /etc/hostapd/hostapd.conf`**

The details for AP/master configuration including the AP name and
password are configured in /etc/hostapd/hostapd.conf.

For the client mode, we need to comment out the bold text from above and
uncomment a few commented lines. Your local WiFi network most likely
will use WPA2 encryption, but there is WEP option possible as well.

`### USB WiFi stick`
`auto wlan0`
`allow-hotplug wlan0`
`### DHCP`
**`iface wlan0 inet dhcp`**
`### static`
`#set IP address that is not in the DHCP pool of local wifi router`
`#also subnet must not clash with eth1 subnet above`
`# iface wlan0 inet static`
`#       address 172.16.0.101`
`#       network 172.16.0.0`
`#       netmask 255.255.255.0`
`#       post-up service isc-dhcp-server restart`
`   ### WPA/WPA2 section for client mode ###`
`      `**`wpa-ssid test1`**
`      `**`wpa-psk fireball`**
`      ### following two options are not necessary `
`      ### - but the WiFi AP must be set to use encoding TKIP, not AES!`
`      #wpa-key-mgmt WPA-PSK`
`      #wpa-group TKIP`
`   ### WEP section for client mode ###`
`      #wireless-essid test1`
`      #wireless-key F9674C82E0`
`   ### hostapd - all client mode settings must be commented out ###`
`#       hostapd /etc/hostapd/hostapd.conf`

Obviously one needs to put in the specific local WiFi network name
(wpa-ssid) and password (wpa-psk).