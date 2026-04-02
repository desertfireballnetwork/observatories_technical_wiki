In order to log in remotely over the network you need to use our
[VPN](wikipedia:Virtual_private_network.html).

# Re-configuring VPN in camera systems - 2023 change

Please refer to this wiki page: [Reconfigure
VPN](Reconfigure_VPN.html).

# Setting up VPN configuration in your laptop/desktop

## How VPN works?

<div>

If you do not have much experience with virtual private networks,
[here](How_VPN_works.html) you can find very brief and condensed
information how VPNs work and why you eg do not need to have a public
fixed IP to connect to an observatory remotely.

</div>

## Getting a VPN Certificate

<div>

In order to use the VPN you will need to request a VPN certificate from
us. Contact [DFN camera help](Getting_Help.html) and we will
provide you with one. Please tell us what platform you will be using
(Linux, Mac OS X, Windows, Android) as sometimes we need to modify the
configuration depending on your platform (Windows required four IP
addresses instead of two for example).

</div>

## Installing and OpenVPN Client

You will then need to install and configure an OpenVPN client that
supports your platform.

### Linux

===== Debian \<=7 & Ubuntu earlier versions with system V init: =====

<div class="toccolours mw-collapsible">

Install OpenVPN by running the command:

`apt-get install openvpn`

Then, place your certificate in `/etc/openvpn` (extract if from the
archive if required). You can now connect to the VPN by running the
command:

`sudo service openvpn start`

To enable OpenVPN so that it starts on boot, run the command:

`sudo update-rc.d openvpn enable`

</div>

##### Debian 8+ & Ubuntu later versions (16.04+ ?) with systemd:

<div class="toccolours mw-collapsible">

Install OpenVPN by running the command:

`apt-get install openvpn`

Then in NetworkManager frontend (the network interfaces icon in task
panel) in VPN connections, add new VPN, select OpenVPN, open the
provided .ovpn file.

... or:

Manually place your certificate in `/etc/openvpn` (extract if from the
.tgz archive if required). You can now connect to the VPN by running the
command:

`sudo systemctl start openvpn@ALIAS.service`

To enable OpenVPN so that it starts on boot, run the command: (replacing
`ALIAS` as required with the config file name without extension)

`sudo systemctl enable openvpn@ALIAS.service`

</div>

#### CentOS 6, RHEL 6 & Fedora 14 (and earlier versions) with system V init

<div class="toccolours mw-collapsible mw-collapsed">

Install OpenVPN by running the command:

`yum install openvpn`

Then, place your certificate in `/etc/openvpn` (extract if from the .tgz
archive if required). You can now connect to the VPN by running the
command:

`sudo service openvpn start`

To enable OpenVPN so that it starts on boot, run the command:

`chkconfig --level 345 openvpn on`

</div>

#### CentOS 7, RHEL 7 & Fedora 17 (and later versions) with systemd

<div class="toccolours mw-collapsible mw-collapsed">

Install OpenVPN by running the command:

`yum install openvpn`

or

`dnf install openvpn`

Then you can either use NetworkManager or set up the certificetes/keys
manually.

##### NetworkManager way

EPEL repository has NetworkManager-openvpn and
NetworkManager-openvpn-gnome which should integrate into the graphical
network management.

`dnf install NetworkManager-openvpn NetworkManager-openvpn-gnome`

Then in NetworkManager frontend (the network interfaces icon in task
panel) in VPN connections, add new VPN, select OpenVPN, open the
provided .ovpn file.

##### Manual way

Place your certificate in `/etc/openvpn` (extract if from the .tgz
archive if required). You can now connect to the VPN by running the
command:

`sudo systemctl start openvpn@ALIAS.service`

To enable OpenVPN so that it starts on boot, run the command: (replacing
`ALIAS` as required with the config file name without extension)

`sudo systemctl enable openvpn@ALIAS.service`

''Note: Later versions like RHEL8 & clones have 'server' and 'client'
subfolders in `/etc/openvpn`, to make things more tidy and better
organised. It is more 'proper' to put the provided keys/certs into the
'client' subfolder. The systemd service name is the slightly different:
"

`sudo systemctl start openvpn-client@ALIAS.service`
`sudo systemctl enable openvpn-client@ALIAS.service`

</div>

#### OpenSuse 13.1 (and later versions) with systemd

<div class="toccolours mw-collapsible mw-collapsed">

Install OpenVPN by running the command:

`zypper in openvpn`

Then, place your certificate in `/etc/openvpn` (extract if from the .tgz
archive if required). Note the `ALIAS` you have. You can now connect to
the VPN by running the command: (replacing `ALIAS` as required)

`sudo systemctl start openvpn@ALIAS.service`

To enable OpenVPN so that it starts on boot, run the command: (replacing
`ALIAS` as required with the config file name without extension)

`sudo systemctl enable openvpn@ALIAS.service`

</div>

### Mac OS X

We recommend [Tunnelblick](https://tunnelblick.net/cConfigT.html)
OpenVPN client.

Install Tunnelblick from [here](https://tunnelblick.net/downloads.html)
and then configure it using the .opvpn file provided by [DFN camera
help](Getting_Help.html) by following the instructions
[here](https://tunnelblick.net/cConfigT.html#creating-and-installing-a-tunnelblick-vpn-configuration)
(*Creating and Installing a Tunnelblick VPN Configuration* section).

### Windows

- [Install
  OpenVPN](https://openvpn.net/index.php/open-source/downloads.html)
  (bottom row of the first table) - download version 2.x, not 3.x - we
  had issues reported with ver 3.
- Place your .opvpn file containing your certificate, key and config
  options into `Program Files/OpenVPN/config`.
- To connect to the VPN, run OpenVPN from the start menu, and double
  click on the new OpenVPN system icon in the taskbar.
- Wait a few seconds; the window will disappear, and the icon will turn
  mostly green once you're connected
- You can now connect to any camera using its VPN [IP
  address](IP_Addresses.html).

### Android

Camera maintenance can also be carried out from a phone or tablet.
Having one of these devices is a great backup in case your laptop runs
out of battery or you can't use it for some other reason. Setting up
Android to use the VPN for remote maintenance is also quite simple.

- Install the [OpenVPN Connect
  app](https://play.google.com/store/apps/details?id=net.openvpn.openvpn).
- Download your .opvpn file onto your phone
- Launch OpenVPN, press "Import" from the three dot menu and find your
  .opvpn file
- Press Connect to initiate your VPN connection, a persistent
  notification should appear in the notification bar

An alternative client we have a good experience with is [OpenVPN for
Android](https://play.google.com/store/apps/details?id=de.blinkt.openvpn).
This one claims it does not contain any adds.

You'll also need an SSH app to connect to the camera (a web based
control panel for the cameras will also be available soon). We recommend
[JuiceSSH](https://play.google.com/store/apps/details?id=com.sonelli.juicessh).

# Logging In via the Terminal

Once you have the certificate installed on your computer and you have
connected to the VPN you will be able to connect to the camera as if you
had a local Ethernet or WiFi connection (although the response time to
commands will be a bit longer). The only difference is that you will
need to use the [VPN IP address](IP_Addresses.html) of the camera
instead of the local login IP address.

If you are on Linux or Mac OS X open a terminal and log into your
observatory using SSH. For example, if you have [set up your hosts file
for convenience](Hosts_File.html) you would into DFNEXT17 by
running the command:

`ssh root@DFNEXT17`

If you have not set up your local [hosts file](Hosts_File.html),
you would run the command:

`ssh root@10.1.23.17`

You will then be prompted to [unlock your SSH key to log
in](SSH_Keys.html) or enter the root user password if you aren't
using an SSH key.

## Logging In via the Terminal from Windows

If you're on Windows, you will need to use PuTTY or Microsoft Windows
Subsystem for Linux; for more information see: [Logging in via Terminal
from
Windows](Logging_in_locally_via_WiFi_or_Ethernet#Logging_in_via_the_Terminal_from_Windows.html).
You will need to use the relevant [VPN IP
address](IP_Addresses.html) instead of the local addresses though.

# Logging in via the Web Interface

If you have [set up your hosts file](Hosts_File.html), you can now
log in via the web interface by entering it in your browser's address
bar with the web interface's port number (8080). For example, to log
into DFNEXT12 you would enter `DFNEXT:8080`.

If you haven't [set up your hosts file for
convenience](Hosts_File.html) then you will need to look up your
observatories [VPN IP address](IP_Addresses.html) and enter that
instead. For example, to log into DFNEXT12 you would enter
`10.1.23.12:8080`.