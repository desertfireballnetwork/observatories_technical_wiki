This page describes the configuration of Mobile data network in DFN
camera systems. Upper level page is
[Mobile_network_-_3G,_4G](Mobile_network_-_3G,_4G.md "wikilink").

First make sure you know [which camera system you
have](Which_camera.md "wikilink"). Most of the GFO systems are DFNEXTs. We
use DFNEXTs pretty much only within the Curtin - operated DFN in Western
and South Australia.

For modem module installation in DFNEXT observatories with blue PCB
modem adaptor board please refer to [DFNEXT Modem
Installation](DFNEXT_Modem_Installation.md "wikilink").

For modem module installation in DFNSMALL/DFNEXT observatories with
green PCB modem adaptor board please refer to [DFN Modem Installation -
Green card](DFN_Modem_Installation_-_Green_card.md "wikilink").

In case of DFNSMALL camera systems with Commell LE-37D PC boards and
modem mPCIe card in the slot on-board the PC (no USB adopter used),
please make sure the BIOS is configured accordingly - please refer to
[DFNSMALL observatories BIOS
configuration](DFNSMALL_observatories_BIOS_configuration.md "wikilink") how
to enable the WLAN card support. (For DFNEXT systems no specific BIOS
configuration is needed.)

There are sevaral modem/operator combinations pre-defined in the DFN
observatory operating system. These are those we use in Australia. For
use with other operators or overseas, a specific configuration needs to
be added.

Below are example configurations for specific modem / camera system
combinations - please note the bold part of the text - that is
identifier of the specific modem/operator setup, that need to match in
both configuration files.

## Sierra MC8705 3G modem, all DFN camera system types

For Sierra MC8705 3G modem with Telstra M2M plan is the configuration as
shown on a corresponding section of file /etc/network/interfaces.

`### Optus/Telstra mobile data GSM/3G/4G`
`### with modem/ppp interface`
`### primary network interface in the field, default route`
`auto ppp0`
`allow-hotplug ppp0`
`iface ppp0 inet wvdial`
`#provider optus_multitech`
`#provider optus_sierra_MC8705`
`#provider telstra_multitech`
`provider `**`telstra_sierra_MC8705`**
`#provider telstra_sierra_MC7430`

Please notice the uncommented line with provider
**telstra_sierra_MC8705**. All lines with "#" at the beginning are
comments.

There are a few more configurations pre-set in file /etc/wvdial.conf.
The corresponding detailed section for our case looks like:

`[Dialer `**`telstra_sierra_MC8705`**`]`
`Stupid Mode = 1`
`Init3 = AT+CGDCONT=1,"IP","telstra.m2m"`
`Modem = /dev/ttyUSB4`
`Password = guest`
`Username = guest`
`Phone = *99#`

## Sierra MC7430 or MC7455 3G/4G modem, all DFNEXT camera systems

### Kernel configuration

Make sure following parameters are passed to kernel at boot time

`net.ifnames=0 `
`biosdevname=0`

How to check current state:

`cat /proc/cmdline `
`BOOT_IMAGE=/boot/vmlinuz-4.9.0-14-amd64 root=UUID=956f7edb-eeb2-4059-a682-efd1760c88f8 ro quiet net.ifnames=0 biosdevname=0 nomodeset usbcore.usbfs_memory_mb=1000`

How to set:

open /etc/default/grub. Add the parameters on the active grub
commandline:

`GRUB_CMDLINE_LINUX_DEFAULT="quiet net.ifnames=0 biosdevname=0 nomodeset usbcore.usbfs_memory_mb=1000"`

run command

`sudo update-grub`

and reboot the system (command 'reboot' or Ctrl+Alt+Del if heyboard
connected to the camera sysytem.

Now after the reboot you can make sure all is set as intended by 'cat
/proc/cmdline'.

### Install Qualcom libs and tools and dhcp client that supports RAW IP

Make sure following packages are installed

`sudo apt-get install libqmi-utils udhcpc socat`

Note: Since late April 2023, the repositories of Debian 9 Stretch (the
DFNEXT camera system is based on this Linux distribution) were removed
from the main mirrors, so you’ll need to switch to archive.debian.org.
Just modify the /etc/apt/sources.list config file to replace any
"deb.debian.org" and "ftp.au.debian.org" and "security.debian.org" with
"archive.debian.org" - make sure that file includes these lines:

`deb `[`http://archive.debian.org/debian`](http://archive.debian.org/debian)` stretch main contrib non-free`
`deb `[`http://archive.debian.org/debian-security`](http://archive.debian.org/debian-security)` stretch/updates main`
`deb-src `[`http://archive.debian.org/debian-security`](http://archive.debian.org/debian-security)` stretch/updates main`

### WWAN interface control script

Make sure following script is installed - should be done by regular auto
matic DFN SW updates.

`/usr/local/bin/qmi-network-raw-dfn`

If this script was missing, easiest is to manually run camera system SW
update

`dfn_down_install_sw_from_server.sh`

but for that your system must be connected to the Internet. So better to
get prepared in lab or have some alternative internet connection other
than the one you are just setting up.

### Configure APN, username and password in file

` /etc/qmi-network.conf`

This is specific for each operator. Examples below - only one should be
uncommented:

`### COMMON FOR ALL OR MOST OPARTORS`
`### USER & PASS usually not needed`
`'''APN_USER=guest`
`APN_PASS=guest'''`
`PROXY=yes`
`###-----------------------`
`### Australia Telstra`
**`APN=telstra.m2m`**
`###-----------------------`
`### Australia Optus`
`# APN=connect`
`###-----------------------`
`### Canada Telus`
`#APN=isp.telus.com`
`###-----------------------`
`### Oman Omantel`
`# APN=taif`
`# APN_USER=taif`
`# APN_PASS=taif`
`###-----------------------`
`### KSA Saudi Telecom Company (STC) "JAWALNet"`
`# APN=jawalnet.com.sa`
`###-----------------------`
`### Australia Aldimobile`
`# if the 1st one does not work, use the Telstra one below`
`# APN=mdata.net.au`
`# APN=Telstra.internet`
`###-----------------------`

Other apn codes [1](https://www.apnsettings.org/)
[2](https://apn.how/) - example for Australia:
[Telstra](https://apn.how/australia/telstra)

### Modify /etc/network/interfaces

Comment out the ppp0 interface start lines:

`#auto ppp0`
`#allow-hotplug ppp0`

Make sure there is wwan0 section as below, with interface start lines
uncommented:

`### alternative config for 3G/4G Sierra MC 7430/7455 via qmi lib`
`### needs wwan0 interface rather than ppp0`
`### in grub, pass kernel param net.ifnames=0 `
`### configure operator APN in /etc/qmi-network.conf`
`### depends on script /usr/local/bin/qmi-network-raw-dfn`
`### >>>> uncomment also 'allow-hotplug line <<<<`
`### >>>> not only 'auto' when enabling wwan0 <<<<`
`auto wwan0`
`allow-hotplug wwan0`
`iface wwan0 inet manual`
`     pre-up ifconfig wwan0 down`
`     pre-up ifconfig wwan1 down # only needed if the modem presents two wwan interfaces`
`     pre-up /usr/local/bin/qmi-network-raw-dfn printsignal`
`     pre-up /usr/local/bin/qmi-network-raw-dfn forcerestart`
`     pre-up udhcpc -p /var/run/udhcpc.wwan0.pid -S -t 5 -b -i wwan0`
`     post-up grep "nameserver 4.2.2.1" || echo "nameserver 4.2.2.1" >> /etc/resolv.conf`
`     post-down /usr/local/bin/qmi-network-raw-dfn stop`
``      post-down kill -TERM `head -1 /var/run/udhcpc.wwan0.pid` ``

### Add following line to crontab to make sure the wwan0 is restarted when it accidentally goes down

*Note: on some systems, this is necessary to bring the wwan intarfce up
at boot time.*

`*/5 * * * * /usr/local/bin/check_restart_wwan0.sh `

*Note: script /usr/local/bin/check_restart_wwan0.sh should be installed
automatically by all on-line DFNEXT camera systems in frame of auto
updates.*

## Sierra MC7430 or MC7455 3G/4G modem in DFNSMALL camera systems

*Note: this may work also with some DFNEXT camera systems where the
LE-37G PC is used and is running earlier BIOS version (Not sure,
probably 2016 build).*

The BIOS version can be printed using the following linux command:

`sudo dmidecode -s bios-version && sudo dmidecode -s bios-release-date`

Which prints eg:

`SKLU_4.0.0.362`
`09/19/2016`

The following box is wvdial.conf config section for Sierra MC7430 3G/4G
modem in Australia. Please mind to uncomment the appropriate line
**telstra_sierra_MC7430** in /etc/network/interfaces.

`[Dialer telstra_sierra_MC7430]`
`Stupid Mode = 1`
`Init3 = AT+CGDCONT=1,"IP","telstra.m2m"`
`Modem = /dev/ttyUSB2`
`Password = guest`
`Username = guest`
`Phone = *99#`

Please contact your service provider for details like AP name (here
"telstra.m2m"), Password, Username and Phone (number) to dial. The
"Modem" field is a virtual port - this parameter is specific for a modem
type. If you are using the modem MC7455 (for the Americas, Europe, the
Middle East and Africa), we believe that it can use the same
configuration as MC7430 (for Australia, Asia and Oceania). The only
lines that need to be changed are AP name (here "telstra.m2m"),
Password, Username and Phone (number) to dial. If your service
provider/operator does not have specific requirement for Password,
Username and Phone (number) to dial or if not sure, just leave these
fields as they are (guest, guest, \*99#) - please refer to the
telstra_sierra_MC8705 example above.

**WARNING: recently we found that some operators, eg Telus in Canada, do
not support wvdial style (PPP protcol) connection. We are working on
alternative connection method that involves Qualcom chipset proprietary
protocol & opensource qmi toolkit.**

Then for both MC7430 and MC7455 you would need to add another section to
/etc/wvdial.conf, copying some existing one, give it a unique name and
modify those lines:

`[Dialer `**`YOUR_OPERATOR_sierra_MC7455`**`]`
`Stupid Mode = 1`
`Init3 = AT+CGDCONT=1,"IP","`**`AP_NAME`**`"`
`Modem = /dev/ttyUSB2`
`Password = `**`YOUR_PASSWORD`**
`Username = `**`YOUR_USERNAME`**
`Phone = `**`YOUR_OPERATOR_DIALUP_PHONE_NUMBER`**

and then you open /etc/network/interfaces, comment out all the
'provider' lines and add your one:

`### Optus/Telstra mobile data GSM/3G/4G`
`### with modem/ppp interface`
`### primary network interface in the field, default route`
`auto ppp0`
`allow-hotplug ppp0`
`iface ppp0 inet wvdial`
`#provider optus_multitech`
`#provider optus_sierra_MC8705`
`#provider telstra_multitech`
`#provider telstra_sierra_MC8705`
`#provider telstra_sierra_MC7430`
`provider `**`YOUR_OPERATOR_sierra_MC7455`**

## Checking the quality of reception - signal strength

[Mobile signal strength check](Mobile_signal_strength_check.md "wikilink")
in the observatory PC operating system